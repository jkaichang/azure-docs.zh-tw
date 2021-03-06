---
title: 在 ML 管線中使用自動化 ML
titleSuffix: Azure Machine Learning
description: 此 AutoMLStep 可讓您在管線中使用自動化機器學習。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: laobri
author: lobrien
manager: cgronlun
ms.date: 06/15/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: 698fdf1295ec38fc7eca9a58e681d7fe13ec32fc
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319502"
---
# <a name="use-automated-ml-in-an-azure-machine-learning-pipeline-in-python"></a>在 Python 中的 Azure Machine Learning 管線中使用自動化 ML
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning 的自動化 ML 功能可協助您探索高效能的模型，而不需要重新實作不免每個可能的方法。 結合 Azure Machine Learning 管線，您可以建立可部署的工作流程，快速找出最適合您資料的演算法。 本文將說明如何有效率地將資料準備步驟加入自動化 ML 步驟。 自動化 ML 可以快速找出最適合您資料的演算法，同時讓您使用管線 MLOps 和模型生命週期運算化。

## <a name="prerequisites"></a>先決條件

* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請在開始前先建立免費帳戶。 立即試用[免費或付費版本的 Azure Machine Learning](https://aka.ms/AMLFree)。

* Azure Machine Learning 工作區。 請參閱[建立 Azure Machine Learning 工作區](how-to-manage-workspace.md)。  

* 基本熟悉 Azure 的[自動化機器學習](concept-automated-ml.md)服務和[機器學習管線](concept-ml-pipelines.md)設施和 SDK。

## <a name="review-automated-mls-central-classes"></a>回顧自動化 ML 的中央類別

管線中的自動化 ML 會以物件表示 `AutoMLStep` 。 `AutoMLStep` 類別是 `PipelineStep` 的子類別。 物件的圖形會 `PipelineStep` 定義 `Pipeline` 。

有數個子類別 `PipelineStep` 。 除了之外 `AutoMLStep` ，本文也會顯示 `PythonScriptStep` 資料準備和另一個註冊模型的。

最初將資料移_入_ML 管線的慣用方式是使用 `Dataset` 物件。 若要在步驟_之間_移動資料，慣用的方法是使用 `PipelineData` 物件。 若要搭配使用 `AutoMLStep` ， `PipelineData` 物件必須轉換成 `PipelineOutputTabularDataset` 物件。 如需詳細資訊，請參閱[ML 管線的輸入和輸出資料](how-to-move-data-in-out-of-pipelines.md)。

`AutoMLStep`是透過物件進行設定 `AutoMLConfig` 。 `AutoMLConfig`是彈性的類別，如在[Python 中設定自動化 ML 實驗](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#configure-your-experiment-settings)中所述。 

會 `Pipeline` 在中執行 `Experiment` 。 管線 `Run` 具有每個步驟的子系 `StepRun` 。 自動化 ML 的輸出 `StepRun` 是訓練計量和最高效能的模型。

為了實現具體意義，本文會針對分類工作建立簡單的管線。 此工作會預測鐵達尼號的生存，但我們不會討論資料或工作，但傳遞時除外。

## <a name="get-started"></a>開始使用

### <a name="retrieve-initial-dataset"></a>取出初始資料集

ML 工作流程通常會從既有的基準資料開始。 這是適用于已註冊資料集的良好案例。 資料集可在工作區中顯示、支援版本設定，而且可以互動方式進行探索。 建立和填入資料集的方法有很多種，如[建立 Azure Machine Learning 資料集](how-to-create-register-datasets.md)中所述。 因為我們將使用 Python SDK 來建立管線，請使用 SDK 來下載基準資料，並將其註冊為 ' titanic_ds ' 名稱。

```python
from azureml.core import Workspace, Dataset

ws = Workspace.from_config()
if not 'titanic_ds' in ws.datasets.keys() :
    # create a TabularDataset from Titanic training data
    web_paths = ['https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
                 'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv']
    titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

    titanic_ds.register(workspace = ws,
                                     name = 'titanic_ds',
                                     description = 'Titanic baseline data',
                                     create_new_version = True)

titanic_ds = Dataset.get_by_name(ws, 'titanic_ds')
```

程式碼會先登入**config.js**中定義的 Azure Machine Learning 工作區（如需說明，請參閱[教學課程：開始使用 Python SDK 建立您的第一個 ML 實驗](tutorial-1st-experiment-sdk-setup.md)）。 如果沒有名為「已註冊」的資料集 `'titanic_ds'` ，則會建立一個。 程式碼會從 Web 下載 CSV 資料，並使用它們來具現化， `TabularDataset` 然後向工作區註冊資料集。 最後，函式會將 `Dataset.get_by_name()` 指派 `Dataset` 給 `titanic_ds` 。 

### <a name="configure-your-storage-and-compute-target"></a>設定您的儲存體和計算目標

管線所需的其他資源是儲存體，通常是 Azure Machine Learning 計算資源。 

```python
from azureml.core import Datastore
from azureml.core.compute import AmlCompute, ComputeTarget

datastore = ws.get_default_datastore()

compute_name = 'cpu-cluster'
if not compute_name in ws.compute_targets :
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                                min_nodes=0,
                                                                max_nodes=1)
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)

    compute_target.wait_for_completion(
        show_output=True, min_node_count=None, timeout_in_minutes=20)

    # Show the result
    print(compute_target.get_status().serialize())

compute_target = ws.compute_targets[compute_name]
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

資料準備和自動化 ML 步驟之間的中繼資料可以儲存在工作區的預設資料存放區中，因此我們不需要 `get_default_datastore()` 在物件上進行多次呼叫 `Workspace` 。 

之後，程式碼會檢查 AML 計算目標是否 `'cpu-cluster'` 已存在。 如果不是，我們會指定我們想要以小型 CPU 為基礎的計算目標。 如果您打算使用自動化 ML 的深度學習功能（例如，具有 DNN 支援的文字特徵化），您應該選擇具有強式 GPU 支援的計算，如[GPU 優化的虛擬機器大小](https://docs.microsoft.com/azure/virtual-machines/sizes-gpu)中所述。 

程式碼會封鎖直到目標布建完成，然後再列印剛建立之計算目標的一些詳細資料。 最後，會從工作區抓取已命名的計算目標，並將其指派給 `compute_target` 。 

### <a name="configure-the-training-run"></a>設定定型執行

下一個步驟是確定遠端訓練回合具有訓練步驟所需的所有相依性。 相依性和執行時間內容是透過建立和設定物件來設定 `RunConfiguration` 。 

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import Environment 

aml_run_config = RunConfiguration()
# Use just-specified compute target ("cpu-cluster")
aml_run_config.target = compute_target

USE_CURATED_ENV = True
if USE_CURATED_ENV :
    curated_environment = Environment.get(workspace=ws, name="AzureML-Tutorial")
    aml_run_config.environment = curated_environment
else:
    aml_run_config.environment.python.user_managed_dependencies = False
    
    # Add some packages relied on by data prep step
    aml_run_config.environment.python.conda_dependencies = CondaDependencies.create(
        conda_packages=['pandas','scikit-learn'], 
        pip_packages=['azureml-sdk[automl,explain]', 'azureml-dataprep[fuse,pandas]'], 
        pin_sdk_version=False)
```

上述程式碼顯示兩個處理相依性的選項。 如所示，使用 `USE_CURATED_ENV = True` ，設定是以策劃環境為基礎。 策劃環境是「prebaked」，其中包含常見的相依程式庫，並可大幅加快上線的速度。 策劃環境已在[Microsoft Container Registry](https://hub.docker.com/publishers/microsoftowner)中預先建立 Docker 映射。 當您變更為時，所採用的路徑會 `USE_CURATED_ENV` `False` 顯示明確設定相依性的模式。 在該案例中，將會在資源群組內的 Azure Container Registry 中建立並註冊新的自訂 Docker 映射（請參閱[Azure 中的私用 Docker 容器登錄簡介](https://docs.microsoft.com/azure/container-registry/container-registry-intro)）。 建立和註冊此映射可能需要相當長的時間。 

## <a name="prepare-data-for-automated-machine-learning"></a>準備自動化機器學習的資料

### <a name="write-the-data-preparation-code"></a>撰寫資料準備程式碼

基準鐵達尼號資料集包含混合的數值和文字資料，但遺漏一些值。 若要為自動化機器學習服務準備它，資料準備管線步驟將會：

- 以亂數據或對應至「未知」的類別填滿遺漏的資料
- 將類別資料轉換成整數
- 卸載我們不想要使用的資料行
- 將資料分割成定型集和測試集
- 將轉換後的資料寫入 `PipelineData` 輸出路徑

```python
%%writefile dataprep.py
from azureml.core import Run

import pandas as pd 
import numpy as np 
import pyarrow as pa
import pyarrow.parquet as pq
import argparse

RANDOM_SEED=42

def prepare_age(df):
    # Fill in missing Age values from distribution of present Age values 
    mean = df["Age"].mean()
    std = df["Age"].std()
    is_null = df["Age"].isnull().sum()
    # compute enough (== is_null().sum()) random numbers between the mean, std
    rand_age = np.random.randint(mean - std, mean + std, size = is_null)
    # fill NaN values in Age column with random values generated
    age_slice = df["Age"].copy()
    age_slice[np.isnan(age_slice)] = rand_age
    df["Age"] = age_slice
    df["Age"] = df["Age"].astype(int)
    
    # Quantize age into 5 classes
    df['Age_Group'] = pd.qcut(df['Age'],5, labels=False)
    df.drop(['Age'], axis=1, inplace=True)
    return df

def prepare_fare(df):
    df['Fare'].fillna(0, inplace=True)
    df['Fare_Group'] = pd.qcut(df['Fare'],5,labels=False)
    df.drop(['Fare'], axis=1, inplace=True)
    return df 

def prepare_genders(df):
    genders = {"male": 0, "female": 1, "unknown": 2}
    df['Sex'] = df['Sex'].map(genders)
    df['Sex'].fillna(2, inplace=True)
    df['Sex'] = df['Sex'].astype(int)
    return df

def prepare_embarked(df):
    df['Embarked'].replace('', 'U', inplace=True)
    df['Embarked'].fillna('U', inplace=True)
    ports = {"S": 0, "C": 1, "Q": 2, "U": 3}
    df['Embarked'] = df['Embarked'].map(ports)
    return df
    
parser = argparse.ArgumentParser()
parser.add_argument('--output_path', dest='output_path', required=True)
args = parser.parse_args()
    
titanic_ds = Run.get_context().input_datasets['titanic_ds']
df = titanic_ds.to_pandas_dataframe().drop(['PassengerId', 'Name', 'Ticket', 'Cabin'], axis=1)
df = prepare_embarked(prepare_genders(prepare_fare(prepare_age(df))))

os.makedirs(os.path.dirname(args.output_path), exist_ok=True)
pq.write_table(pa.Table.from_pandas(df), args.output_path)

print(f"Wrote test to {args.output_path} and train to {args.output_path}")
```

上述程式碼片段是完整但最少的鐵達尼號資料資料準備範例。 程式碼片段的開頭為 Jupyter "魔術 command"，以將程式碼輸出至檔案。 如果您不是使用 Jupyter 筆記本，請移除該行並手動建立檔案。

`prepare_`上述程式碼片段中的各種函數會修改輸入資料集中的相關資料行。 當資料變更為 Pandas 物件之後，這些函式會對其運作 `DataFrame` 。 在每個案例中，遺失的資料可能會以代表「不明」的代表性亂數據或類別資料填入。 以文字為基礎的類別資料會對應至整數。 不再需要的資料行會遭到覆寫或捨棄。 

在程式碼定義資料準備函數之後，程式碼會剖析輸入引數，這是我們想要寫入資料的路徑。 （這些值將由 `PipelineData` 下一個步驟中將討論的物件決定）。程式碼會抓取已註冊的 `'titanic_cs'` `Dataset` ，將它轉換成 Pandas `DataFrame` ，並呼叫各種資料準備函數。 

由於 `output_path` 是完整的，因此 `os.makedirs()` 會使用函數來準備目錄結構。 此時，您可以使用 `DataFrame.to_csv()` 來寫入輸出資料，但 Parquet 檔案會更有效率。 這種效率可能與這類小型資料集無關，但使用**PyArrow**套件的和函式， `from_pandas()` 只需要 `write_table()` 比更多的按鍵 `to_csv()` 。

Parquet 檔案原本就受到下列所討論的自動化 ML 步驟所支援，因此不需要特殊處理就能使用它們。 

### <a name="write-the-data-preparation-pipeline-step-pythonscriptstep"></a>撰寫資料準備管線步驟（ `PythonScriptStep` ）

以上所述的資料準備程式碼必須與 `PythonScripStep` 要與管線搭配使用的物件相關聯。 寫入 Parquet 資料準備輸出的路徑是由物件所產生 `PipelineData` 。 先前準備的資源（例如、 `ComputeTarget` `RunConfig` 、和） `'titanic_ds' Dataset` 會用來完成規格。

```python
from azureml.pipeline.core import PipelineData
from azureml.pipeline.steps import PythonScriptStep

prepped_data_path = PipelineData("titanic_train", datastore).as_dataset()
prepped_data_path = PipelineData("titanic_train", datastore).as_dataset()

dataprep_step = PythonScriptStep(
    name="dataprep", 
    script_name="dataprep.py", 
    compute_target=compute_target, 
    runconfig=aml_run_config,
    arguments=["--output_path", prepped_data_path],
    inputs=[titanic_ds.as_named_input("titanic_ds")],
    outputs=[prepped_data_path],
    allow_reuse=True
)
```

`prepped_data_path`物件的類型為 `PipelineOutputFileDataset` 。 請注意，它是在 `arguments` 和 `outputs` 引數中指定。 如果您檢查上一個步驟，您會看到在資料準備程式碼中，引數的值 `'--output_path'` 是 Parquet 檔案寫入的檔案路徑。 

## <a name="train-with-automlstep"></a>使用 AutoMLStep 進行定型

您可以使用類別來設定自動化 ML 管線步驟 `AutoMLConfig` 。 在[Python 中設定自動化 ML 實驗](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train)中會說明此彈性類別。 資料輸入和輸出是在 ML 管線中需要特別注意之設定的唯一層面。 `AutoMLConfig`下圖會詳細討論管線中的輸入和輸出。 除了資料以外，ML 管線的優點是能夠針對不同的步驟使用不同的計算目標。 您可以選擇使用更強大的功能， `ComputeTarget` 只用于自動化 ML 程式。 這麼做就像指派更強大的 `RunConfiguration` 物件參數一樣簡單 `AutoMLConfig` `run_configuration` 。

### <a name="send-data-to-automlstep"></a>將資料傳送至`AutoMLStep`

在 ML 管線中，輸入資料必須是 `Dataset` 物件。 最高效能的方法是以物件的形式提供輸入資料 `PipelineOutputTabularDataset` 。 您可以使用或在上建立該類型的物件 `parse_parquet_files()` `parse_delimited_files()` `PipelineOutputFileDataset` ，例如 `prepped_data_path` 物件。

```python
# type(prepped_data_path) == PipelineOutputFileDataset
# type(prepped_data) == PipelineOutputTabularDataset
prepped_data = prepped_data_path.parse_parquet_files(file_extension=None)
```

上述程式碼片段會 `PipelineOutputTabularDataset` 從資料準備步驟的輸出中建立高效能 `PipelineOutputFileDataset` 。

另一個選項是使用 `Dataset` 工作區中註冊的物件：

```python
prepped_data = Dataset.get_by_name(ws, 'Data_prepared')
```

比較這兩種技術：

| 技巧 | 優點和缺點 | 
|-|-|
|`PipelineOutputTabularDataset`| 較高的效能 | 
|| 自然路由來源`PipelineData` | 
|| 管線執行後不會保存資料 |
|| [顯示技術的筆記本 `PipelineOutputTabularDataset`](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb) |
| 經過`Dataset` | 效能低 |
| | 可以透過許多方式產生 | 
| | 資料會持續存在，而且會在整個工作區中顯示 |
| | [顯示已註冊 `Dataset` 技術的筆記本](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/continuous-retraining/auto-ml-continuous-retraining.ipynb)

### <a name="specify-automated-ml-outputs"></a>指定自動化 ML 輸出

的輸出是效能 `AutoMLStep` 較高之模型和該模型本身的最終度量分數。 若要在進一步的管線步驟中使用這些輸出，請準備 `PipelineData` 物件以接收它們。

```python
from azureml.pipeline.core import TrainingOutput

metrics_data = PipelineData(name='metrics_data',
                           datastore=datastore,
                           pipeline_output_name='metrics_output',
                           training_output=TrainingOutput(type='Metrics'))
model_data = PipelineData(name='best_model_data',
                           datastore=datastore,
                           pipeline_output_name='model_output',
                           training_output=TrainingOutput(type='Model'))
```

上述程式碼片段會 `PipelineData` 為計量和模型輸出建立兩個物件。 每個都會命名為、指派給稍早所抓取的預設資料存放區，以及與中的特定相關聯的 `type` `TrainingOutput` `AutoMLStep` 。 因為我們 `pipeline_output_name` 對這些 `PipelineData` 物件指派，所以它們的值不只是來自個別管線步驟，而是從管線整體提供，如下面的「檢查管線結果」一節中所討論。 

### <a name="configure-and-create-the-automated-ml-pipeline-step"></a>設定並建立自動化 ML 管線步驟

一旦定義輸入和輸出，就可以建立 `AutoMLConfig` 和 `AutoMLStep` 。 設定的詳細資料將取決於您的工作，如在[Python 中設定自動化 ML 實驗](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train)中所述。 針對鐵達尼號的「生存分類」工作，下列程式碼片段示範簡單的設定。

```python
from azureml.train.automl import AutoMLConfig
from azureml.pipeline.steps import AutoMLStep

# Change iterations to a reasonable number (50) to get better accuracy
automl_settings = {
    "iteration_timeout_minutes" : 10,
    "iterations" : 2,
    "experiment_timeout_hours" : 0.25,
    "primary_metric" : 'AUC_weighted'
}

automl_config = AutoMLConfig(task = 'classification',
                             path = '.',
                             debug_log = 'automated_ml_errors.log',
                             compute_target = compute_target,
                             run_configuration = aml_run_config,
                             featurization = 'auto',
                             training_data = prepped_data,
                             label_column_name = 'Survived',
                             **automl_settings)

train_step = AutoMLStep(name='AutoML_Classification',
    automl_config=automl_config,
    passthru_automl_config=False,
    outputs=[metrics_data,model_data],
    allow_reuse=True)
```
此程式碼片段會顯示常用於的用法 `AutoMLConfig` 。 較流暢的引數（超參數-ish）會在個別的字典中指定，而較不可能變更的值則直接在此函式中指定 `AutoMLConfig` 。 在此情況下， `automl_settings` 指定 brief 執行：執行只會在2個反復專案或15分鐘後停止，視何者先發生。

`automl_settings`字典會以 kwargs 的形式傳遞至函式 `AutoMLConfig` 。 其他參數並不複雜：

- `task`在此範例中，會設定為 `classification` 。 其他有效的值為 `regression` 和`forecasting`
- `path`和 `debug_log` 會描述專案的路徑，以及要寫入調試資訊的本機檔案。 
- `compute_target`是先前所定義的， `compute_target` 在此範例中，是以成本實惠的 CPU 為基礎的電腦。 如果您使用的是 AutoML 的深度學習設備，您會想要將計算目標變更為以 GPU 為基礎的
- `featurization` 設定為 `auto`。 如需更多詳細資訊，請參閱自動化 ML 設定檔的[資料特徵化](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#data-featurization)一節。 
- `training_data`設定為 `PipelineOutputTabularDataset` 從資料準備步驟的輸出所建立的物件 
- `label_column_name`指出我們想要預測的資料行 

`AutoMLStep`本身會採用， `AutoMLConfig` 並以輸出的形式建立用 `PipelineData` 來保存計量和模型資料的物件。 

>[!Important]
> `passthru_automl_config` `False` 如果您 `AutoMLStep` 使用 `PipelineOutputTabularDataset` 物件進行輸入，您必須將設為。

在此範例中，自動化 ML 程式會在上執行交叉驗證 `training_data` 。 您可以使用引數來控制交叉驗證的數目 `n_cross_validations` 。 如果您已將定型資料分割為數據準備步驟的一部分，您可以將設定 `validation_data` 為自己的 `Dataset` 。

您可能偶爾會看到 `X` 資料功能和 `y` 資料標籤的使用。 這項技術已被取代，您應該使用 `training_data` 進行輸入。 

## <a name="register-the-model-generated-by-automated-ml"></a>註冊自動化 ML 所產生的模型 

基本 ML 管線中的最後一個步驟是註冊已建立的模型。 將模型新增至工作區的模型登錄，即可在入口網站中使用，而且可以進行版本設定。 若要註冊模型，請撰寫另一個 `PythonScriptStep` 接受 `model_data` 輸出的 `AutoMLStep` 。

### <a name="write-the-code-to-register-the-model"></a>撰寫程式碼以註冊模型

模型會在中註冊 `Workspace` 。 您可能很熟悉使用 `Workspace.from_config()` 來登入本機電腦上的工作區，但有另一種方法可以從執行中的 ML 管線內取得工作區。 會抓取 `Run.get_context()` 使用中的 `Run` 。 此 `run` 物件可讓您存取許多重要物件，包括 `Workspace` 此處所使用的。

```python
%%writefile register_model.py
from azureml.core.model import Model, Dataset
from azureml.core.run import Run, _OfflineRun
from azureml.core import Workspace
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("--model_name", required=True)
parser.add_argument("--model_path", required=True)
args = parser.parse_args()

print(f"model_name : {args.model_name}")
print(f"model_path: {args.model_path}")

run = Run.get_context()
ws = Workspace.from_config() if type(run) == _OfflineRun else run.experiment.workspace

model = Model.register(workspace=ws,
                       model_path=args.model_path,
                       model_name=args.model_name)

print("Registered version {0} of model {1}".format(model.version, model.name))
```

### <a name="write-the-pythonscriptstep-code"></a>撰寫 PythonScriptStep 程式碼

模型註冊會 `PythonScriptStep` `PipelineParameter` 針對其中一個引數使用。 管線參數是管線的引數，可在執行提交時輕鬆設定。 一旦宣告之後，它們就會當做一般引數傳遞。 

```python

from azureml.pipeline.core.graph import PipelineParameter

# The model name with which to register the trained model in the workspace.
model_name = PipelineParameter("model_name", default_value="TitanicSurvivalInitial")

register_step = PythonScriptStep(script_name="register_model.py",
                                       name="register_model",
                                       allow_reuse=False,
                                       arguments=["--model_name", model_name, "--model_path", model_data],
                                       inputs=[model_data],
                                       compute_target=compute_target,
                                       runconfig=aml_run_config)
```

## <a name="create-and-run-your-automated-ml-pipeline"></a>建立並執行自動化 ML 管線

建立和執行包含的管線 `AutoMLStep` 與一般管線並無不同。 

```python
from azureml.pipeline.core import Pipeline
from azureml.core import Experiment

pipeline = Pipeline(ws, [dataprep_step, train_step, register_step])

experiment = Experiment(workspace=ws, name='titanic_automl')

run = experiment.submit(pipeline, show_output=True)
run.wait_for_completion()
```

上述程式碼會將資料準備、自動化 ML 和模型註冊步驟結合成一個 `Pipeline` 物件。 接著，它會建立 `Experiment` 物件。 此函式 `Experiment` 會抓取已命名的實驗（如果有的話），或在必要時加以建立。 它會將提交 `Pipeline` 至 `Experiment` ，建立 `Run` 會以非同步方式執行管線的物件。 函式會 `wait_for_completion()` 封鎖，直到執行完成為止。

### <a name="examine-pipeline-results"></a>檢查管線結果 

完成後 `run` ，您可以取出已 `PipelineData` 指派的物件 `pipeline_output_name` 。 您可以下載結果並加以載入，以供進一步處理。  

```python
metrics_output_port = run.get_pipeline_output('metrics_output')
model_output_port = run.get_pipeline_output('model_output')

metrics_output_port.download('.', show_progress=True)
model_output_port.download('.', show_progress=True)
```

下載的檔案會寫入子目錄中 `azureml/{run.id}/` 。 計量檔案是 JSON 格式，而且可以轉換成 Pandas 資料框架進行檢查。

若要進行本機處理，您可能需要安裝相關的套件，例如 Pandas、Pickle、AzureML SDK 等等。 在此範例中，自動化 ML 找到的最佳模型很可能會取決於 XGBoost。

```bash
!pip install xgboost==0.90
```

```python
import pandas as pd
import json

metrics_filename = metrics_output._path_on_datastore
# metrics_filename = path to downloaded file
with open(metrics_filename) as f:
   metrics_output_result = f.read()
   
deserialized_metrics_output = json.loads(metrics_output_result)
df = pd.DataFrame(deserialized_metrics_output)
df
```

上述程式碼片段顯示從 Azure 資料存放區的位置載入計量檔案。 您也可以從下載的檔案載入它，如批註中所示。 當您將它還原序列化並轉換成 Pandas 資料框架之後，您就可以查看自動化 ML 步驟的每個反復專案的詳細計量。

模型檔案可以還原序列化為 `Model` 物件，供您用於推斷、進一步的計量分析等等。 

```python
import pickle

model_filename = model_output._path_on_datastore
# model_filename = path to downloaded file

with open(model_filename, "rb" ) as f:
    best_model = pickle.load(f)

# ... inferencing code not shown ...
```

如需載入及使用現有模型的詳細資訊，請參閱搭配[使用現有的模型與 Azure Machine Learning](how-to-deploy-existing-model.md)。

### <a name="download-the-results-of-an-automated-ml-run"></a>下載自動化 ML 執行的結果

如果您已遵循文章的指示，將會有具現化的 `run` 物件。 但是，您也可以藉 `Run` 由物件從抓取完成的物件 `Workspace` `Experiment` 。

工作區包含所有實驗和執行的完整記錄。 您可以使用入口網站來尋找並下載實驗的輸出或使用程式碼。 若要從歷史執行存取記錄，請使用 Azure Machine Learning 來尋找您感興趣的執行識別碼。 使用該識別碼，您可以透過和來選擇特定的 `run` `Workspace` `Experiment` 。

```python
# Retrieved from Azure Machine Learning web UI
run_id = 'aaaaaaaa-bbbb-cccc-dddd-0123456789AB'
experiment = ws.experiments['titanic_automl']
run = next(run for run in ex.get_runs() if run.id == run_id)
```

您必須將上述程式碼中的字串變更為歷程記錄執行的細節。 上述程式碼片段假設您已將指派給 `ws` `Workspace` 與正常的相關的 `from_config()` 。 會直接抓取感關注的實驗，然後程式碼會藉 `Run` 由比對值來尋找相關的 `run.id` 。

一旦擁有 `Run` 物件，您就可以下載計量和模型。 

```python
automl_run = next(r for r in run.get_children() if r.name == 'AutoML_Classification')
outputs = automl_run.get_outputs()
metrics = outputs['default_metrics_AutoML_Classification']
model = outputs['default_model_AutoML_Classification']

metrics.get_port_data_reference().download('.')
model.get_port_data_reference().download('.')
```

每個 `Run` 物件 `StepRun` 都包含物件，其中包含個別管線步驟執行的相關資訊。 `run`會搜尋的 `StepRun` 物件 `AutoMLStep` 。 系統會使用預設名稱來抓取計量和模型，即使您未將物件傳遞至的參數，也會提供這些標準 `PipelineData` `outputs` `AutoMLStep` 。 

最後，實際的計量和模型會下載至您的本機電腦，如上方「檢查管線結果」一節所述。

## <a name="next-steps"></a>後續步驟

- 執行此 Jupyter 筆記本，以在使用回歸來預測出租車車資的[管線中，顯示自動化 ML 的完整範例](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb)
- [不需要撰寫程式碼即可建立自動化 ML 實驗](how-to-use-automated-ml-for-ml-models.md)
- 探索[示範自動化 ML 的各種 Jupyter 筆記本](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)
- 閱讀將您的管線整合到[端對端 MLOps](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment#automate-the-ml-lifecycle)或調查[MLOps GitHub 存放庫](https://github.com/Microsoft/MLOpspython)中的相關資訊 

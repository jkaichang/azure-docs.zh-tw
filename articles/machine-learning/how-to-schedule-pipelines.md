---
title: 排程 Azure Machine Learning 管線
titleSuffix: Azure Machine Learning
description: 使用適用于 Python 的 Azure Machine Learning SDK 來排程 Azure Machine Learning 管線。 排定的管線可讓您將例行、耗時的工作自動化，例如資料處理、訓練和監視。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: laobri
author: lobrien
ms.date: 11/12/2019
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: c076ceafc35a11f172ff35b7ab5ce89d1bf99f17
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319553"
---
# <a name="schedule-machine-learning-pipelines-with-azure-machine-learning-sdk-for-python"></a>使用適用于 Python 的 Azure Machine Learning SDK 來排程機器學習管線

在本文中，您將瞭解如何以程式設計方式排程管線以在 Azure 上執行。 您可以選擇根據經過時間或檔案系統變更來建立排程。 以時間為基礎的排程可以用來處理例行的工作，例如監視資料漂移。 以變更為基礎的排程可用於回應異常或無法預期的變更，例如上傳的新資料或正在編輯的舊資料。 學習如何建立排程之後，您將瞭解如何取出和停用排程。

## <a name="prerequisites"></a>先決條件

* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://aka.ms/AMLFree)。

* Python 環境，其中安裝了適用于 Python 的 Azure Machine Learning SDK。 如需詳細資訊，請參閱[建立和管理可重複使用的環境，以使用 Azure Machine Learning 進行定型和部署。](how-to-use-environments.md)

* 具有已發行管線的 Machine Learning 工作區。 您可以使用內建的[AZURE MACHINE LEARNING SDK 來建立及執行機器學習管線](how-to-create-your-first-pipeline.md)。

## <a name="initialize-the-workspace--get-data"></a>& 取得資料來初始化工作區

若要排程管線，您需要工作區的參考、已發佈管線的識別碼，以及您要在其中建立排程的實驗名稱。 您可以使用下列程式碼來取得這些值：

```Python
import azureml.core
from azureml.core import Workspace
from azureml.pipeline.core import Pipeline, PublishedPipeline
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

experiments = Experiment.list(ws)
for experiment in experiments:
    print(experiment.name)

published_pipelines = PublishedPipeline.list(ws)
for published_pipeline in  published_pipelines:
    print(f"{published_pipeline.name},'{published_pipeline.id}'")

experiment_name = "MyExperiment" 
pipeline_id = "aaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" 
```

## <a name="create-a-schedule"></a>建立排程

若要定期執行管線，您必須建立排程。 會使 `Schedule` 管線、實驗和觸發程式產生關聯。 觸發程式可以是 `ScheduleRecurrence` ，它會描述執行之間的等候，或指定要監看是否有變更之目錄的資料存放區路徑。 不論是哪一種情況，您都需要管線識別碼和用來建立排程的實驗名稱。

在 python 檔案的頂端，匯入 `Schedule` 和 `ScheduleRecurrence` 類別：

```python

from azureml.pipeline.core.schedule import ScheduleRecurrence, Schedule
```

### <a name="create-a-time-based-schedule"></a>建立以時間為基礎的排程

此函式 `ScheduleRecurrence` 具有必要的 `frequency` 引數，必須是下列其中一個字串： "Minute"、"Hour"、"Day"、"Week" 或 "Month"。 它也需要整數 `interval` 引數，指定 `frequency` 排程開始之間應經過多少單位。 選擇性引數可讓您更明確地瞭解開始時間，如[SCHEDULERECURRENCE SDK](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedulerecurrence?view=azure-ml-py)檔中所述。

建立一 `Schedule` 開始每15分鐘執行一次的：

```python
recurrence = ScheduleRecurrence(frequency="Minute", interval=15)
recurring_schedule = Schedule.create(ws, name="MyRecurringSchedule", 
                            description="Based on time",
                            pipeline_id=pipeline_id, 
                            experiment_name=experiment_name, 
                            recurrence=recurrence)
```

### <a name="create-a-change-based-schedule"></a>建立以變更為基礎的排程

檔案變更所觸發的管線，可能比以時間為基礎的排程更有效率。 例如，您可能會想要在檔案變更時，或將新檔案新增至資料目錄時，執行前置處理步驟。 您可以監視資料存放區的任何變更，或資料存放區內特定目錄中的變更。 如果您監視特定的目錄，該目錄的子目錄內的變更將_不_會觸發執行。

若要建立檔案-回應 `Schedule` ，您必須在對 `datastore` [Schedule. create](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedule?view=azure-ml-py#create-workspace--name--pipeline-id--experiment-name--recurrence-none--description-none--pipeline-parameters-none--wait-for-provisioning-false--wait-timeout-3600--datastore-none--polling-interval-5--data-path-parameter-name-none--continue-on-step-failure-none--path-on-datastore-none---workflow-provider-none---service-endpoint-none-)的呼叫中設定參數。 若要監視資料夾，請設定 `path_on_datastore` 引數。

`polling_interval`引數可讓您指定資料存放區檢查變更的頻率（以分鐘為單位）。

如果管線是使用[資料路徑](https://docs.microsoft.com/python/api/azureml-core/azureml.data.datapath.datapath?view=azure-ml-py) [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelineparameter?view=azure-ml-py)所建立，您可以藉由設定引數，將該變數設定為已變更檔案的名稱 `data_path_parameter_name` 。

```python
datastore = Datastore(workspace=ws, name="workspaceblobstore")

reactive_schedule = Schedule.create(ws, name="MyReactiveSchedule", description="Based on input file change.",
                            pipeline_id=pipeline_id, experiment_name=experiment_name, datastore=datastore, data_path_parameter_name="input_data")
```

### <a name="optional-arguments-when-creating-a-schedule"></a>建立排程時的選擇性引數

除了先前討論的引數之外，您也可以將 `status` 引數設定為， `"Disabled"` 以建立非作用中的排程。 最後，可 `continue_on_step_failure` 讓您傳遞會覆寫管線預設失敗行為的布林值。

### <a name="use-azure-logic-apps-for-more-complex-workflows"></a>針對更複雜的工作流程使用 Azure Logic Apps

Azure Logic Apps 支援更複雜的工作流程，而且比 Azure Machine Learning 管線更廣泛整合。 如需詳細資訊，請參閱[從邏輯應用程式觸發 Machine Learning 管線的執行](how-to-trigger-published-pipeline.md)。

## <a name="view-your-scheduled-pipelines"></a>查看已排程的管線

在您的網頁瀏覽器中，流覽至 [Azure Machine Learning]。 從導覽面板的 [**端點**] 區段中，選擇 [**管線端點**]。 這會帶您前往工作區中發佈的管線清單。

![AML 的 [管線] 頁面](./media/how-to-schedule-pipelines/scheduled-pipelines.png)

在此頁面中，您可以查看工作區中所有管線的摘要資訊：名稱、描述、狀態等等。 在您的管線中按一下以深入瞭解。 在產生的頁面上，您會有更多關於管線的詳細資料，您可以向下切入到個別的執行。

## <a name="deactivate-the-pipeline"></a>停用管線

如果您有已 `Pipeline` 發佈但未排程的，您可以使用將它停用：

```python
pipeline = PublishedPipeline.get(ws, id=pipeline_id)
pipeline.disable()
```

如果已排程管線，您必須先取消排程。 從入口網站或執行下列程式，抓取排程的識別碼：

```python
ss = Schedule.list(ws)
for s in ss:
    print(s)
```

一旦您 `schedule_id` 想要停用，請執行：

```python
def stop_by_schedule_id(ws, schedule_id):
    s = next(s for s in Schedule.list(ws) if s.id == schedule_id)
    s.disable()
    return s

stop_by_schedule_id(ws, schedule_id)
```

如果您再次執行 `Schedule.list(ws)` ，您應該會得到空白清單。

## <a name="next-steps"></a>後續步驟

在本文中，您使用了適用于 Python 的 Azure Machine Learning SDK，以兩種不同的方式來排程管線。 一個排程會根據經過的時鐘時間而重複。 如果在指定的上 `Datastore` 或在該存放區的目錄內修改檔案，就會執行另一個排程。 您已瞭解如何使用入口網站來檢查管線和個別執行。 最後，您已瞭解如何停用排程，讓管線停止執行。

如需詳細資訊，請參閱

> [!div class="nextstepaction"]
> [使用 Azure Machine Learning 管線來進行批次評分](tutorial-pipeline-batch-scoring-classification.md)

* 深入瞭解[管線](concept-ml-pipelines.md)
* 深入瞭解[使用 Jupyter 探索 Azure Machine Learning](samples-notebooks.md)


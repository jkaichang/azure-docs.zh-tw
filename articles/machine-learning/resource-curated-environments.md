---
title: 策劃環境
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 中可用的策劃環境集合
services: machine-learning
author: luisquintanilla
ms.author: luquinta
ms.reviewer: luquinta
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/08/2020
ms.openlocfilehash: 72ccf2a765f50358635e4a803ed0b92e60bd7d19
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87012207"
---
# <a name="azure-machine-learning-curated-environments"></a>Azure Machine Learning 策劃環境

本文列出 Azure Machine Learning 中的策劃環境，以及預先安裝的套件和通道。 策劃環境是由 Azure Machine Learning 提供，而且預設會在您的工作區中提供。 它們是由快取的 Docker 映射所支援，可降低執行準備成本並允許更快的部署時間。 使用這些環境，快速開始使用各種機器學習架構。

> [!NOTE]
> 此清單會在2020年6月更新。 使用 Python SDK 來取得最新的清單。 如需詳細資訊，請參閱[環境一文](./how-to-use-environments.md#use-a-curated-environment)。

## <a name="azure-automl"></a>Azure AutoML

- [AzureML AutoML](#azureml-automl)
- [AzureML AutoML GPU](#azureml-automl-gpu)
- [AzureML AutoML DNN](#azureml-automl-dnn)
- [AzureML AutoML DNN GPU](#azureml-automl-dnn-gpu)
- [Azure AutoML DNN 願景 GPU](#azureml-automl-dnn-vision-gpu)

### <a name="azureml-automl"></a>AzureML AutoML

套件通道：
- anaconda
- conda-偽造
- pytorch

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-管線-核心 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-解讀 = = 1.8。0
  - azureml-說明-model = = 1.8。0
  - azureml-automl-核心 = = 1.8。0
  - azureml-automl-runtime = = 1.8。0
  - azureml-定型-automl-client = = 1.8。0
  - azureml-定型-automl-runtime = = 1.8.0. post1
  - 推斷-架構
  - pyarrow = = 0.17。0
  - .py-cpuinfo = = 5.0。0
- numpy>= 1.16.0，<= 1.16。2
- pandas>= 0.21.0，<= 0.23。4
- .py-xgboost<= 0.90
- fbprophet = = 0。5
- setuptools-git
- psutil>5.0.0，<6.0。0

### <a name="azureml-automl-gpu"></a>AzureML AutoML GPU

套件通道：
- anaconda
- conda-偽造
- pytorch

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-管線-核心 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-解讀 = = 1.8。0
  - azureml-說明-model = = 1.8。0
  - azureml-automl-核心 = = 1.8。0
  - azureml-automl-runtime = = 1.8。0
  - azureml-定型-automl-client = = 1.8。0
  - azureml-定型-automl-runtime = = 1.8.0. post1
  - 推斷-架構
  - pyarrow = = 0.17。0
  - .py-cpuinfo = = 5.0。0
- numpy>= 1.16.0，<= 1.16。2
- pandas>= 0.21.0，<= 0.23。4
- fbprophet = = 0。5
- setuptools-git
- psutil>5.0.0，<6.0。0

### <a name="azureml-automl-dnn"></a>AzureML AutoML DNN

套件通道：
- anaconda
- conda-偽造
- pytorch

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-管線-核心 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-解讀 = = 1.8。0
  - azureml-說明-model = = 1.8。0
  - azureml-automl-核心 = = 1.8。0
  - azureml-automl-runtime = = 1.8。0
  - azureml-定型-automl-client = = 1.8。0
  - azureml-定型-automl-runtime = = 1.8.0. post1
  - 推斷-架構
  - pytorch-轉換器 = = 1.0。0
  - spacy = = 2.1。8
  - https://aka.ms/automl-resources/packages/en_core_web_sm-2.1.0.tar.gz
  - pyarrow = = 0.17。0
  - .py-cpuinfo = = 5.0。0
- pandas>= 0.21.0，<= 0.23。4
- numpy>= 1.16.0，<= 1.16。2
- .py-xgboost<= 0.90
- fbprophet = = 0。5
- setuptools-git
- pytorch = 1.4。0
- cudatoolkit = 10.0.130
- psutil>5.0.0，<6.0。0

### <a name="azureml-automl-dnn-gpu"></a>AzureML AutoML DNN GPU

套件通道：
- anaconda
- conda-偽造
- pytorch

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-管線-核心 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-解讀 = = 1.8。0
  - azureml-說明-model = = 1.8。0
  - azureml-automl-核心 = = 1.8。0
  - azureml-automl-runtime = = 1.8。0
  - azureml-定型-automl-client = = 1.8。0
  - azureml-定型-automl-runtime = = 1.8.0. post1
  - 推斷-架構
  - horovod = = 0.19。4
  - pytorch-轉換器 = = 1.0。0
  - spacy = = 2.1。8
  - https://aka.ms/automl-resources/packages/en_core_web_sm-2.1.0.tar.gz
  - pyarrow = = 0.17。0
  - .py-cpuinfo = = 5.0。0
- pandas>= 0.21.0，<= 0.23。4
- numpy>= 1.16.0，<= 1.16。2
- fbprophet = = 0。5
- setuptools-git
- pytorch = 1.4。0
- cudatoolkit = 10.0.130
- psutil>5.0.0，<6.0。0

### <a name="azureml-automl-dnn-vision-gpu"></a>AzureML AutoML DNN 願景 GPU

從屬
- python = 3。7
- pip
  - azureml-automl-核心 = = 1.8。0
  - azureml-核心 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-automl-client = = 1.8。0
  - azureml-automl-runtime = = 1.8。0
  - azureml-解讀 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-說明-model = = 1.8。0
  - azureml-定型-automl-runtime = = 1.8.0. post1
  - azureml-定型-automl = = 1.8。0
  - azureml-contrib-dataset = = 1.8。0
  - azureml-管線-核心 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - azureml-管線-步驟 = = 1.8。0
  - azureml-管線 = = 1.8。0
  - azureml-定型 = = 1.8。0
  - azureml-sdk = = 1.8。0
  - azureml-contrib-automl-dnn-願景 = = 1.8。0

## <a name="azure-ml-designer"></a>Azure ML 設計工具

- [AzureML 設計工具](#azureml-designer)
- [AzureML 設計工具 CV](#azureml-designer-cv)
- [AzureML 設計工具 CV 轉換](#azureml-designer-cv-transform)
- [AzureML 設計工具 IO](#azureml-designer-io)
- [AzureML 設計工具 NLP](#azureml-designer-nlp)
- [AzureML 設計工具 PyTorch](#azureml-designer-pytorch)
- [AzureML 設計工具 PyTorch 訓練](#azureml-designer-pytorch-train)
- [AzureML 設計工具 R](#azureml-designer-r)
- [AzureML 設計工具推薦](#azureml-designer-recommender)
- [AzureML 設計工具分數](#azureml-designer-score)
- [AzureML 設計工具轉換](#azureml-designer-transform)

### <a name="azureml-designer"></a>AzureML 設計工具

套件通道：
- conda-偽造

從屬
- python = 3.6。8
- scikit-learn-驚喜 = 1.0。6
- pip
  - azureml-設計工具-傳統模組 = = 0.0.124
  - https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-2.1.0/en_core_web_sm-2.1.0.tar.gz#egg=en_core_web_sm
  - spacy = = 2.1。7

### <a name="azureml-designer-cv"></a>AzureML 設計工具 CV

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-設計工具-cv-模組 = = 0.0。6

### <a name="azureml-designer-cv-transform"></a>AzureML 設計工具 CV 轉換

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-設計工具-cv 模組 [pytorch] = = 0.0。6

### <a name="azureml-designer-io"></a>AzureML 設計工具 IO

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-dataprep>= 1。6
  - azureml-設計工具-dataio-模組 = = 0.0.30

### <a name="azureml-designer-nlp"></a>AzureML 設計工具 NLP

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-設計工具-傳統模組 = = 0.0.121
  - https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-2.1.0/en_core_web_sm-2.1.0.tar.gz#egg=en_core_web_sm
  - spacy = = 2.1。7

### <a name="azureml-designer-pytorch"></a>AzureML 設計工具 PyTorch

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-設計工具-pytorch-模組 = = 0.0。8

### <a name="azureml-designer-pytorch-train"></a>AzureML 設計工具 PyTorch 訓練

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-設計工具-pytorch-模組 = = 0.0。8

### <a name="azureml-designer-r"></a>AzureML 設計工具 R

套件通道：
- conda-偽造

從屬
- python = 3.6。8
- r-插入號 = 6。0
- r-catools = 1.17。1
- r-cluster = 2.1。0
- r-dplyr = 0.8.5 版
- r-e1071 = 1。7
- r-forcats = 0.5。0
- r-預測 = 8.12
- r-glmnet = 2。0
- r-igraph = 1.2.4 版
- r-矩陣 = 1。2
- r-mclust = 5.4。6
- r-mgcv = 1。8
- r-nlme = 3。1
- r-nnet = 7。3
- r-plyr = 1.8。6
- r-randomforest = 4。6
- r-reticulate = 1.12
- r-rocr = 1。0
- r-rodbc = 1。3
- r-rpart = 4。1
- r-stringr = 1.4。0
- r-tidyverse = 1.2。1
- r-timedate 時間 = 3043.102
- r-tseries = 0.10
- r = 3.5。1
- pip
  - azureml-設計工具-傳統模組 = = 0.0.124

### <a name="azureml-designer-recommender"></a>AzureML 設計工具推薦

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-設計工具-推薦-模組 = = 0.0.5 版

### <a name="azureml-designer-score"></a>AzureML 設計工具分數

套件通道：
- 預設值

從屬
- conda
- python = 3.6。8
- pip
  - azureml-設計工具-分數-模組 = = 0.0.5 版

### <a name="azureml-designer-transform"></a>AzureML 設計工具轉換

套件通道：
- 預設值

從屬
- python = 3.6。8
- pip
  - azureml-設計工具-datatransform-模組 = = 0.0.49

## <a name="azureml-hyperdrive-forecastdnn"></a>AzureML Hyperdrive ForecastDNN

從屬
- python = 3。7
- pip
  - azureml-核心 = = 1.8。0
  - azureml-管線-核心 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-automl-核心 = = 1.8。0
  - azureml-automl-runtime = = 1.8。0
  - azureml-定型-automl-client = = 1.8。0
  - azureml-定型-automl-runtime = = 1.8.0. post1
  - azureml-contrib-automl-dnn-預測 = = 1.8。0

## <a name="azureml-minimal"></a>最低 AzureML

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0

## <a name="azureml-sidecar"></a>AzureML 側車

套件通道：
- conda-偽造

從屬
- python = 3.6。2

## <a name="azureml-tutorial"></a>AzureML 教學課程

套件通道：
- anaconda
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - azureml-widget = = 1.8。0
  - azureml-管線-核心 = = 1.8。0
  - azureml-管線-步驟 = = 1.8。0
  - azureml-opendatasets = = 1.8。0
  - azureml-automl-核心 = = 1.8。0
  - azureml-automl-runtime = = 1.8。0
  - azureml-定型-automl-client = = 1.8。0
  - azureml-定型-automl-runtime = = 1.8.0. post1  
  - azureml-定型-automl = = 1.8。0
  - azureml-定型 = = 1.8。0
  - azureml-sdk = = 1.8。0
  - azureml-解讀 = = 1.8。0
  - azureml-tensorboard = = 1.8。0
  - azureml-mlflow = = 1.8。0
  - mlflow
  - sklearn-pandas
- pandas
- numpy
- tqdm
- scikit-learn
- matplotlib

## <a name="azureml-vowpalwabbit-880"></a>AzureML VowpalWabbit 8.8。0

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-dataprep [保險絲，pandas]

## <a name="dask"></a>Dask

- [AzureML Dask CPU](#azureml-dask-cpu)
- [AzureML Dask GPU](#azureml-dask-gpu)

### <a name="azureml-dask-cpu"></a>AzureML Dask CPU

套件通道：
- conda-偽造
- pytorch
- 預設值

從屬
- python = 3.6。9
- pip
  - adlfs
  - azureml-核心 = = 1.8。0
  - azureml-dataprep
  - dask [完成]
  - dask-ml [完成]
  - 分散式
  - fastparquet
  - fsspec
  - joblib
  - jupyterlab
  - lz4
  - mpi4py
  - notebook
  - pyarrow

### <a name="azureml-dask-gpu"></a>AzureML Dask GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。9
- pip
  - azureml-預設值 = = 1.8。0
  - adlfs
  - azureml-核心 = = 1.8。0
  - azureml-dataprep
  - dask [完成]
  - dask-ml [完成]
  - 分散式
  - fastparquet
  - fsspec
  - joblib
  - jupyterlab
  - lz4
  - mpi4py
  - notebook
  - pyarrow
- matplotlib

## <a name="chainer"></a>Chainer

- [AzureML Chainer 5.1.0 CPU](#azureml-chainer-510-cpu)
- [AzureML Chainer 5.1.0 GPU](#azureml-chainer-510-gpu)

### <a name="azureml-chainer-510-cpu"></a>AzureML Chainer 5.1.0 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - chainer = = 5.1。0
  - mpi4py = = 3.0。0

### <a name="azureml-chainer-510-gpu"></a>AzureML Chainer 5.1.0 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - chainer = = 5.1。0
  - 才能 cupy-cuda90 = = 5.1。0
  - mpi4py = = 3.0。0

## <a name="pyspark"></a>PySpark

### <a name="azureml-pyspark-mmlspark-015"></a>AzureML PySpark MmlSpark 0.15

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0

## <a name="pytorch"></a>PyTorch

- [AzureML PyTorch 1.0 CPU](#azureml-pytorch-10-cpu)
- [AzureML PyTorch 1.0 GPU](#azureml-pytorch-10-gpu)
- [AzureML PyTorch 1.1 CPU](#azureml-pytorch-11-cpu)
- [AzureML PyTorch 1.1 GPU](#azureml-pytorch-11-gpu)
- [AzureML PyTorch 1.2 CPU](#azureml-pytorch-12-cpu)
- [AzureML PyTorch 1.2 GPU](#azureml-pytorch-12-gpu)
- [AzureML PyTorch 1.3 CPU](#azureml-pytorch-13-cpu)
- [AzureML PyTorch 1.3 GPU](#azureml-pytorch-13-gpu)
- [AzureML PyTorch 1.4 CPU](#azureml-pytorch-14-cpu)
- [AzureML PyTorch 1.4 GPU](#azureml-pytorch-14-gpu)
- [AzureML PyTorch 1.5 CPU](#azureml-pytorch-15-cpu)
- [AzureML PyTorch 1.5 GPU](#azureml-pytorch-15-gpu)

### <a name="azureml-pytorch-10-cpu"></a>AzureML PyTorch 1.0 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。0
  - torchvision = = 0.2。1
  - mkl = = 2018.0。3
  - horovod = = 0.16。1

### <a name="azureml-pytorch-10-gpu"></a>AzureML PyTorch 1.0 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。0
  - torchvision = = 0.2。1
  - mkl = = 2018.0。3
  - horovod = = 0.16。1

### <a name="azureml-pytorch-11-cpu"></a>AzureML-PyTorch 1.1 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。1
  - torchvision = = 0.2。1
  - mkl = = 2018.0。3
  - horovod = = 0.16。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-11-gpu"></a>AzureML PyTorch 1.1 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。1
  - torchvision = = 0.2。1
  - mkl = = 2018.0。3
  - horovod = = 0.16。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-12-cpu"></a>AzureML PyTorch 1.2 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。2
  - torchvision = = 0.4。0
  - mkl = = 2018.0。3
  - horovod = = 0.16。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-12-gpu"></a>AzureML PyTorch 1.2 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。2
  - torchvision = = 0.4。0
  - mkl = = 2018.0。3
  - horovod = = 0.16。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-13-cpu"></a>AzureML PyTorch 1.3 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。3
  - torchvision = = 0.4。1
  - mkl = = 2018.0。3
  - horovod = = 0.18。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-13-gpu"></a>AzureML PyTorch 1.3 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1。3
  - torchvision = = 0.4。1
  - mkl = = 2018.0。3
  - horovod = = 0.18。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-14-cpu"></a>AzureML PyTorch 1.4 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1.4。0
  - torchvision = = 0.5。0
  - mkl = = 2018.0。3
  - horovod = = 0.18。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-14-gpu"></a>AzureML PyTorch 1.4 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1.4。0
  - torchvision = = 0.5。0
  - mkl = = 2018.0。3
  - horovod = = 0.18。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-15-cpu"></a>AzureML PyTorch 1.5 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1.5。0
  - torchvision = = 0.5。0
  - mkl = = 2018.0。3
  - horovod = = 0.19。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

### <a name="azureml-pytorch-15-gpu"></a>AzureML PyTorch 1.5 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - torch = = 1.5。0
  - torchvision = = 0.5。0
  - mkl = = 2018.0。3
  - horovod = = 0.19。1
  - tensorboard==1.14.0
  - 未來 = = 0.17。1

## <a name="scikit-learn"></a>Scikit-learn 學習

### <a name="azureml-scikit-learn-0203"></a>AzureML Scikit-learn-瞭解0.20。3

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - scikit-learn==0.20.3
  - scipy = = 1.2。1
  - joblib = = 0.13。2

## <a name="tensorflow"></a>TensorFlow

- [Azure ML TensorFlow 1.10 CPU](#azureml-tensorflow-110-cpu)
- [Azure ML TensorFlow 1.10 GPU](#azureml-tensorflow-110-gpu)
- [Azure ML TensorFlow 1.12 CPU](#azureml-tensorflow-112-cpu)
- [Azure ML TensorFlow 1.12 GPU](#azureml-tensorflow-112-gpu)
- [Azure ML TensorFlow 1.13 CPU](#azureml-tensorflow-113-cpu)
- [Azure ML TensorFlow 1.13 GPU](#azureml-tensorflow-113-gpu)
- [Azure ML TensorFlow 2.0 CPU](#azureml-tensorflow-20-cpu)
- [Azure ML TensorFlow 2.0 GPU](#azureml-tensorflow-20-gpu)
- [Azure ML TensorFlow 2.1 CPU](#azureml-tensorflow-21-cpu)
- [Azure ML TensorFlow 2.1 GPU](#azureml-tensorflow-21-gpu)

### <a name="azureml-tensorflow-110-cpu"></a>AzureML TensorFlow 1.10 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow = = 1.10
  - horovod = = 0.15。2

### <a name="azureml-tensorflow-110-gpu"></a>AzureML TensorFlow 1.10 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow-gpu = = 1.10。0
  - horovod = = 0.15。2

### <a name="azureml-tensorflow-112-cpu"></a>AzureML TensorFlow 1.12 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow = = 1.12
  - horovod = = 0.15。2

### <a name="azureml-tensorflow-112-gpu"></a>AzureML TensorFlow 1.12 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow-gpu = = 1.12。0
  - horovod = = 0.15。2

### <a name="azureml-tensorflow-113-cpu"></a>AzureML-TensorFlow 1.13 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow = = 1.13。1
  - horovod = = 0.16。1

### <a name="azureml-tensorflow-113-gpu"></a>AzureML TensorFlow 1.13 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow-gpu = = 1.13。1
  - horovod = = 0.16。1

### <a name="azureml-tensorflow-20-cpu"></a>AzureML TensorFlow 2.0 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow = = 2。0
  - horovod = = 0.18。1

### <a name="azureml-tensorflow-20-gpu"></a>AzureML TensorFlow 2.0 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow-gpu = = 2.0。0
  - horovod = = 0.18。1

### <a name="azureml-tensorflow-21-cpu"></a>AzureML TensorFlow 2.1 CPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow = = 2.1。0
  - horovod = = 0.19。1

### <a name="azureml-tensorflow-21-gpu"></a>AzureML TensorFlow 2.1 GPU

套件通道：
- conda-偽造

從屬
- python = 3.6。2
- pip
  - azureml-核心 = = 1.8。0
  - azureml-預設值 = = 1.8。0
  - azureml-遙測 = = 1.8。0
  - azureml-定型-restclients-hyperdrive = = 1.8。0
  - azureml-定型-核心 = = 1.8。0
  - tensorflow-gpu = = 2.1。0
  - horovod = = 0.19。1


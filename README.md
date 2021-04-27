# Project Overview

In this project the objective is to use Azure to configure a cloud-based machine learning production model, deploy it, and consume it. And also create, publish, and consume a pipeline. In the end, its needed to demonstrate all of work by creating a a documentation in two formats: README file and a screencast video. This project is part of the Udacity Azure ML Nanodegree. 

All project was created using availabe here:  [Bank Marketing dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)

The data Dictionary can be found [here](https://archive.ics.uci.edu/ml/datasets/bank+marketing).


## Index
1. [Architectural Diagram](#diagram)
2. [Key Steps](#keysteps)
    -  [Authentication](#auth)
    -  [AutoML Run](#automl)
    -  [Deploy Best Model](#bestmodel)
    -  [Enabling Logging](#logs)
    -  [Setup Swagger](#swagger)
    -  [Consume Model Endpoint](#modelconsume)
    -  [Create pipeline](#createpipe)
    -  [Consume pipeline](#consumepipe)
    -  [Publish pipeline](#publishepipe)
3. [Screen Recording](#screen)
4. [Standout Suggestions](#sugestions)
5. [Licensing and Acknowledgements](#Licensing)

## Architectural Diagram<a name="diagram"></a>
*TODO*: Provide an architectual diagram of the project and give an introduction of each step. An architectural diagram is an image that helps visualize the flow of operations from start to finish. In this case, it has to be related to the completed project, with its various stages that are critical to the overall flow. For example, one stage for managing models could be "using Automated ML to determine the best model". 

<p align="center">
  <img src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/fluxograma.png?raw=true" alt="Architect Diagram"/>
</p>


- ***Authentication :*** In this step, we need to create a Security Principal (SP) to interact with the Azure Workspace (this can be skipped while using workspace provided from Udacity).

- ***AutoML Run:*** this step was necessary to create an Automated ML model by selecting the Bank Marking dataset from Registered Dataset and create a compute cluster (Standard_DS12_v2) for processing with the specified configurations: `Type =  Classification, Exit Criterion = 1 hour and Concurrency = 1 (this was set auto when creating the compute cluster)`

- ***Select the Best Model and Deploy:*** After automl run it was need to select the best model on the top of experiment and deploy it  using "Azure Container Instances" (ACI) with  "Authentication" enabled.

- ***Enabling Logging with Application Insights:*** This phase was necessary to Enable Application Insights by downloading config.json file and changing the name of the model in `logs.py` file. By  running `python logs.py` in terminal  this enabled the application insights logs for deployed model.

- ***Setup Swagger :*** After enabling logging is possible to reach a swagger documentation endpoint provided by Azure for deployed model. By copying the it and create a `swagger.json` file in the same directory of `swagger.sh ` and `serve.py `. It was possible to initialize and test the swagger docummentation by executing `bash swagger.sh` and then `python serve.py`.

- ***Consume Model Endpoint:*** To consume the model is a good practice to check in deployed model section for consume tab, in this case you can get URI and Secret Key to fill the file `endpoint.py` and then run in terminal `python endpoint.py` 

    ***Obs.:In this section was made and extra step using Apache Benchmark*** 

- ***Create pipeline with Python SDK:*** With the notebook provided in the starter files it was need to make some modifications to create the pipeline with SDK (i.e.: including the Experiment created previusly).

- ***Consume Pipeline with Python SDK:*** Once the pipeline is created it was needed to run cells provided in notebook to consume the pipeline.

- ***Publish Pipeline with Python SDK:*** In this step it was need to run cells provided in notebook to Publish the pipeline.


## Key Steps<a name="keysteps"></a>
- ### Authentication <a name="auth"></a>
   This Step is opitional since If you are using the lab Udacity provided to you, you can skip this step because we are not authorized to create a security principal. 
- ### AutoML Run <a name="automl"></a>
   This step was need to create a dataset by uploading the csv data from `Bank Marketing dataset`.
   
  <p align="center">
        <img width="900" height="400" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/dataset_create.jpg?raw=true" alt="dataset creation"/>
  </p>
  <p align="center">
        <b><i>fig 1 - Creating a dataset by uploading a CSV file<i><b>
  </p>

  <p align="center">
        <img width="900" height="300" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/dataset_created.jpg?raw=true" alt="dataset creation completed"/>
  </p>
  <p align="center">
        <b><i>fig 2 -  Dataset Registered <i><b>
  </p>
   
   after registered the dataset we created an Automated ML model and then created a compute cluster for processing. Afeter everything is setting we must start run.
   
## Screen Recording<a name="screen"></a>
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions<a name="sugestions"></a>
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.

## Licensing and Acknowledgements<a name="Licensing"></a>

[MIT License](https://github.com/git/git-scm.com/blob/master/MIT-LICENSE.txt).

Thanks for Udacity for give the oportunity to work with this amazing content.

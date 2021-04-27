# Operationalizing Machine Learning

In this project the objective is to use Azure to configure a cloud-based machine learning production model, deploy it, and consume it. And also create, publish, and consume a pipeline. In the end, its needed to demonstrate all of work by creating a a documentation in two formats: README file and a screencast video. This project is part of the Udacity Azure ML Nanodegree. 

All project was created using availabe here:  [Bank Marketing dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)
The data Dictionary can be found [here](https://archive.ics.uci.edu/ml/datasets/bank+marketing).

## Architectural Diagram
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


## Key Steps
*TODO*: Write a short discription of the key steps. Remeber to include all the screenshots required to demonstrate key steps. 

## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.

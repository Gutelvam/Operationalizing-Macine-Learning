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
    -  [Create, Consume and Publish pipeline](#createpipe)
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
        <i><b>Fig 1</b> - Creating a dataset by uploading a CSV file</i>
  </p>
 
  <p align="center">
        <img width="900" height="350" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/dataset_create.jpg?raw=true" alt="dataset creation"/>
  </p>
  
  <p align="center">
        <i><b>Fig 2</b> -  Dataset Registered </i>
  </p>
  
  <p align="center">
        <img width="900" height="250" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/dataset_created.jpg?raw=true" alt="dataset creation completed"/>
  </p>
  
   
   With registered dataset we can create an Automated ML experiment with a compute cluster for processing. After everything is setting we must start run the experiment and when it's completed you can check at experiment section. like the image below:
   <p align="center">
        <i><b>Fig 3</b> -  AutoML Experiment completed </i>
  </p>
  
  <p align="center">
        <img width="1050" height="200" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/exp_show_completed.jpg?raw=true" alt="dataset creation completed"/>
  </p>
  
   
- ### Deploy Best Model <a name="bestmodel"></a>   
   After run the experiment we can now check the details of the run and look for the best performance model:
   <p align="center">
        <i><b>Fig 4</b> -  AutoML Experiment Details </i>
  </p>
  
  <p align="center">
        <img width="1050" height="400" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/exp_completed.jpg?raw=true" alt="experiment  completed"/>
  </p>
  
  In this image above we can notice that VotingEnsemble had the best performance but you can check the best model in details, the best will be at top of the list (Fig 5).
  
  <p align="center">
        <i><b>Fig 5</b> -  AutoML Best Model </i>
  </p>
  
  <p align="center">
        <img width="100%" height="250" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/best_model_name.jpg?raw=true" alt="best model name"/>
  </p>
  
  We can look for some extra metrics about the `VotingEnsemble` model(Fig 6) and some explanation like feature importance(Fig 7).
  <p align="center">
        <i><b>Fig 6</b> -  VotingEnsemble Metrics </i>
  </p>
  
  <p align="center">
        <img width="100%" height="400" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/best_model_metrics_1.jpg?raw=true" alt="VotingEnsemble Metrics"/>
  </p>
  
  
  <p align="center">
        <i><b>Fig 7</b> -  VotingEnsemble Feature Importance Explanation </i>
  </p>
  
  <p align="center">
        <img width="1050" height="400" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/best_model_featureImp.jpg?raw=true" alt="VotingEnsemble Feature Importance Explanation"/>
  </p>
  
  So after reach in a good model its necessay to deploy the model to continue the project.
  
  ***Deploying the model***
  
  The model was deployed with "Azure Container Instances" (ACI) and "Authentication" enabled, so cheking the endpoints section we can see the model deployed(Fig 8). 
  
  <p align="center">
        <i><b>Fig 8</b> -  Best model Deploy </i>
  </p>
  
  <p align="center">
        <img width="1050" height="250" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/best_deploy.jpg?raw=true" alt="best_deploy"/>
  </p>
  
- ### Enabling Logging <a name="logs"></a>  
    This phase was necessary to Enable Application Insights  changing the name of model in the file `logs.py` (fig 9) and runing it in terminal (Fig 10).
    
  <p align="center">
        <i><b>Fig 9</b> -  logs.py </i>
  </p>
  
  <p align="center">
        <img width="100%" height="550" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/logs_py.jpg?raw=true" alt="set logs"/>
  </p>
  
  <p align="center">
        <i><b>Fig 10</b> -  logs.py Execution </i>
  </p>
  
  <p align="center">
        <img width="100%" height="550" src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/logs_py_exec.jpg?raw=true" alt="execution logs"/>
  </p>

    After execution of this script we can follow to endpoit again to check out the status of Application Insights (Fig 11).
    
  <p align="center">
        <i><b>Fig 11</b> -  Application Insights Enabled </i>
  </p>
  
  <p align="center">
        <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/insight_enabled.jpg?raw=true" alt="insight enabled"/>
  </p>
    

- ### Setup Swagger <a name="swagger"></a>  

 First of all to make swagger documentation we needed to copy and create a swagger.json file from `swagger URI`  provided by `Azure` when looking at endpoint section (the `swagger.json` created must be in the same folder than `swagger.sh` and `serve.py`). To setup swagger it was necessary to change the port to 9000 in the `swagger.sh` file because `80` was already taken.  So using bash we can run the .sh file `bash swagger.sh` it starts swagger, after that with a new terminal we run `python serve.py` to simulate the endpoint calls with documentation.
 
There are two type of HTTP therms, get (Fig 12) and post(Fig 13) that we can interact with.

<p align="center">
        <i><b>Fig 12</b> -  Swagger Get interaction </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/swagger_get_interaction.jpg?raw=true" alt="swagger_get_interaction"/>
</p>

<p align="center">
        <i><b>Fig 13</b> -  Swagger Post interaction </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/swagger_post_interaction.jpg?raw=true" alt="swagger_post_interaction"/>
</p>
    
- ###  Consume Model Endpoint <a name="modelconsume"></a>  

Finally, now we can interact with the model and test it by feeding with data. It's possible to do this by providing the scoring_uri and the Primarykey to the `endpoint.py` script and then execute it (Fig 14, 15).


<p align="center">
        <i><b>Fig 14</b> - endpoint.py </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/endpoint.jpg?raw=true" alt="endpoint"/>
</p>

<p align="center">
        <i><b>Fig 15</b> - endpoint.py Execution </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/execution_endpy.jpg?raw=true" alt="endpoint execution"/>
</p>

***Apache Benchmark***

First you need to make sure that you have Apache Benchmak command-line tool installed and available in your path, in the `endpoint.py` replace the key and URI again and run it, after that u may see a file called data.json in your folder. The next step is to run the `benchmark.sh` file with `bash benchmark.sh` and u may see a similar result but with more details (Fig 16).


<p align="center">
        <i><b>Fig 16</b> - Apache Benchmark </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/benchmarketing.jpg?raw=true" alt="Apache Benchmark"/>
</p>


- ###  Create, Consume and Publish pipeline <a name="createpipe"></a>  
In this step was necessary to use the Jupyter Notebook provided for this project named `aml-pipelines-with-automated-machine-learning-step`, therefore it was necessary to use Python SDK (Fig 17).

<p align="center">
        <i><b>Fig 17</b> - Pipiline Creation </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/pipeline_run.jpg?raw=true" alt="Pipiline Creation"/>
</p>

<p align="center">
        <i><b>Fig 18</b> - Pipiline schedule </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/pipeline_schedule.jpg?raw=true" alt="Pipiline schedule"/>
</p>

<p align="center">
        <i><b>Fig 19</b> - Pipiline Completed </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/pipeline_completed.jpg?raw=true" alt="Pipiline Completed"/>
</p>

To be able to consume the model via Python SDK we first saved the best Pipelile as `Pickle` Format the steps in the pipeline can be noticed below:

<p align="center">
        <i><b>Fig 20</b> - Pipiline Steps </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/pipeline_steps.jpg?raw=true" alt="pipeline_steps"/>
</p>

<p align="center">
        <i><b>Fig 21</b> - Publish pipeline and Create the REST endpoint</i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/pipeline_endpoint.jpg?raw=true" alt="pipeline_endpoint"/>
</p>

<p align="center">
        <i><b>Fig 22</b> - Test the REST endpoint</i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/pipeline_endpoint_test.jpg?raw=true" alt="pipeline_endpoint_test"/>
</p>

<p align="center">
        <i><b>Fig 23</b> - verify experiment run of pipeline REST endpoint</i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/pipeline_experiment.jpg?raw=true" alt="pipeline_experiment"/>
</p>


## Screen Recording<a name="screen"></a>
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions<a name="sugestions"></a>
In this project we noticed that we reached a realy good accuracy and AUC, but if u look at some details you can se that the dataset is highly unbalanced a good approach to reduce the bias of this would be utilize some technices like random-sampling, under-sampling or over-sampling. Below you can see the confusion matrix and it's possible to validade the unbalanciment.

<p align="center">
        <i><b>Fig 24 </b> - Confusion Matrix </i>
</p>

<p align="center">
    <img  src="https://github.com/Gutelvam/Operationalizing-Macine-Learning/blob/readme/img/best_model_confusion_matrix.jpg?raw=true" alt="Confusion Matrix"/>
</p>

Other Aproach is to reduce dimensionality based on feature importance and look if model perform better or get a better interpretation.

> "Feature selection helps to avoid both of these problems by reducing the number of features in the model, trying to optimize the model performance. In doing so, feature selection also provides an extra benefit: Model interpretation. With fewer features, the output model becomes simpler and easier to interpret, and it becomes more likely for a human to trust future predictions made by the model." by [Gabriel Azevedo](https://towardsdatascience.com/feature-selection-techniques-for-classification-and-python-tips-for-their-application-10c0ddd7918b#:~:text=Feature%20selection%20helps%20to%20avoid,an%20extra%20benefit%3A%20Model%20interpretation)



## Licensing and Acknowledgements<a name="Licensing"></a>

[MIT License](https://github.com/git/git-scm.com/blob/master/MIT-LICENSE.txt).

Thanks for Udacity for give the oportunity to work with this amazing content.

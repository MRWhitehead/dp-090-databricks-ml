---
lab:
    title: 'Running experiments in Azure Machine Learning'
---
# Running experiments in Azure Machine Learning

Machine Learning is primarily about training models that you can use to provide predictive services to applications. In this exercise, you will learn to run experiments in Azure Machine Learning from Azure Databricks. To begin, you need to have access to an Azure Databricks workspace with an interactive cluster. If you do not have a workspace and/or the required cluster, follow the instructions below. Otherwise, you can skip to the section [Install libraries on the Azure Databricks Cluster](#Install-libraries-on-the-Azure-Databricks-Cluster).

## Unit Pre-requisites

**Microsoft Azure Account**: You will need a valid and active Azure account for the Azure labs. If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/free/)

- If you are a Visual Studio Active Subscriber, you are entitled to Azure credits per month. You can refer to this [link](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) to find out more including how to activate and start using your monthly Azure credit.

- If you are not a Visual Studio Subscriber, you can sign up for the FREE [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) program to create Azure free account (includes 1 year of free services, $200 for 1st month).

## Create the required resources

To complete this exercise, you will need to deploy both Azure Databricks workspace and Azure Machine Learning workspace in your Azure subscription.

### Deploy an Azure Databricks workspace

1. Click the following button to open the Azure Resource Manager template in the Azure Portal.
   [Deploy Databricks from the Azure Resource Manager Template](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-databricks-workspace%2Fazuredeploy.json)

1. Provide the required values to create your Azure Databricks workspace:

   - **Subscription**: Choose the Azure Subscription in which to deploy the workspace.
   - **Resource Group**: Leave at Create new and provide a name for the new resource group.
   - **Location**: Select a location near you for deployment. For the list of regions supported by Azure Databricks, see [Azure services available by region](https://azure.microsoft.com/regions/services/).
   - **Workspace Name**: Provide a name for your workspace.
   - **Pricing Tier**: Ensure `standard` is selected.

1. Select **Review + create**.
1. Select **Create**.
1. The workspace creation takes a few minutes. During workspace creation, the portal displays the Submitting deployment for Azure Databricks tile on the right side. You may need to scroll right on your dashboard to see the tile. There is also a progress bar displayed near the top of the screen. You can watch either area for progress.

### Create a cluster

1. When your Azure Databricks workspace creation is complete, select the link to go to the resource.

1. Select **Launch Workspace** to open your Databricks workspace in a new tab.

1. In the left-hand menu of your Databricks workspace, select **Clusters**.

1. Select **Create Cluster** to add a new cluster.

    ![The create cluster page](images/02-azure-databricks-create-cluster.png 'Create New Cluster Dialog')

1. Enter a name for your cluster. Use your name or initials to easily differentiate your cluster from your coworkers.

1. Select the **Databricks RuntimeVersion**: **Runtime: 7.3 LTS ML (Scala 2.12, Spark 3.0.1)** (remember to select the **ML** version).

1. Select the values for the cluster configuration.
    - **Enable autoscaling**: **Uncheck** this option.

    - **Auto Termination**: Leave **checked** and in the text box enter `120`.

    - **Worker Type**: **Standard_DS3_v2**

    - **Workers**: `1`

    - **Driver Type**: **Same as worker**

1. Select **Create Cluster**.

### Install libraries on the Azure Databricks Cluster

The notebooks you will run depends on certain Python libraries that will need to be installed in your cluster. The following steps walk you through adding these dependencies.

- From within the Azure Databricks workspace, from the `Clusters` section, select your cluster. Make sure the state of the cluster is `Running`.
- Select the **Libraries** link and then select **Install New**.
- In the Library Source, select **PyPi** and in the `Package` text box type `azureml-sdk[databricks]` and select **Install**.
- Next install `sklearn-pandas==2.1.0`
- Next install `azureml-mlflow`

    ![Install library dialog](images/04-azure-databricks-install-library.png 'Install Library Dialog')

### Upload the Databricks notebook archive

1. If you have already uploaded the Databricks notebook archive **04 - Integrating Azure Databricks and Azure Machine Learning.dbc** to your workspace, you can skip to the section [Upload the model training data](#Upload-the-model-training-data).

1. Select the link below to download the `Databricks notebook archive` file to your local computer:

   [04 - Integrating Azure Databricks and Azure Machine Learning.dbc](https://github.com/MicrosoftLearning/dp-090-databricks-ml/blob/master/04%20-%20Integrating%20Azure%20Databricks%20and%20Azure%20Machine%20Learning.dbc?raw=true)

1. Within the Azure Databricks Workspace, using the command bar on the left, select **Workspace**, **Users** and select your username (the entry with house icon).

1. In the blade that appears, select the downwards pointing chevron next to your name, and select **Import**.

    ![The Import menu item can be accessed by selecting your username from the list of users in the workspace.](images/02-azure-databricks-import-menu.png "Import Menu")

1. On the Import Notebooks dialog, browse and open the `04 - Integrating Azure Databricks and Azure Machine Learning.dbc` file from your local computer and then select **Import**.

    ![Obtaining a zip archive of the repository to access the notebook for upload into the Databricks workspace.](images/04-azure-databricks-import-repository.png "Obtaining a local copy of the repository")

1. A folder named after the archive should appear. Select that folder.

1. The folder will contain one or more notebooks. These are the notebooks you will use in completing this exercise.

### Upload the model training data

1. If you have already created the table **nyc_taxi** in your workspace, you can skip to the section [Deploy an Azure Machine Learning workspace](#Deploy-an-Azure-Machine-Learning-workspace).

1. Open the link below in a new browser tab and then **right-click + Save as** to download the data file to your local computer. Save the file as **csv**, and name it `nyc-taxi.csv`.

   [nyc-taxi.csv](https://github.com/MicrosoftLearning/dp-090-databricks-ml/blob/master/data/nyc-taxi.csv?raw=true)

1. Within the Azure Databricks Workspace, select **Import & Explore Data**.

    ![Azure Databricks landing page](images/02-azure-databricks-landing-page.png 'Import & Explore Data')

1. Upload the `nyc-taxi.csv` file from your local compute and then select **Create Table with UI**.

    ![Create new table page](images/02-azure-databricks-upload-file.png 'Create New Table')

1. Select your cluster and then select **Preview Table**.

    ![Create new table page](images/02-azure-databricks-preview-table.png 'Preview Table')

1. Provide the following information then select **Create Table**.

    - **Table Name**: `nyc_taxi`
    - **File Type**: `csv`
    - **Column Delimiter**: `,`
    - **First row is header**: `checked`
    - **Infer schema**: `checked`
    - **Multi-line**: `unchecked`

    ![Create new table](images/02-azure-databricks-create-table.png 'Create Table')

### Deploy an Azure Machine Learning workspace

1. If you have already created an Azure Machine Learning workspace in your subscription, you can skip to the section [Exercise: Running experiments in Azure Machine Learning](#Exercise-Running-experiments-in-Azure-Machine-Learning).

1. In the [Azure Portal](https://portal.azure.com/#home), select **+ Create a resource**, then type `Machine Learning` into the search bar.

1. Select the product **Machine Learning** and then select **Create**.

    ![Create Azure Machine Learning workspace](images/04-create-aml-ws-01.png 'Create AML workspace')

1. In the Create Machine Learning Workspace dialog that appears, provide the following values:

   - **Subscription**: Choose your Azure subscription.
   - **Resource group**: Select the resource group in which you deployed your Azure Databricks workspace.
   - **Workspace Name**: `aml-ws`
   - **Region**: Choose a region closest to you (it is OK if the Azure Databricks Workspace and the Azure Machine Learning Workspace are in different locations).

1. Select **Review + Create** and then select **Create** when the form values passes validation.

    ![Create Azure Machine Learning workspace](images/04-create-aml-ws-02.png 'Create AML workspace')

## Exercise: Running experiments in Azure Machine Learning

In this exercise, you will learn to run experiments in Azure Machine Learning from Azure Databricks.

1. Within the Azure Databricks Workspace, using the command bar on the left, select **Workspace**, **Users** and select your username (the entry with house icon). Open the folder named **04 - Integrating Azure Databricks and Azure Machine Learning** to find the notebook **1.0 Running experiments in Azure Machine Learning**.

1. Then read the notes in the notebook, running each code cell in turn. After completing the exercises in the notebook continue below to review the training metrics and artifacts for your experiment.

1. From within the Azure Machine Learning studio, navigate to the **Experiments** tab, and open the experiment run that corresponds to the MLflow experiment. In the **Metrics** tab of the run, you will observe the model metrics that were logged via MLflow tracking APIs.

    ![Model metrics](images/04-01-03-01-AML-metrics.png 'Model Metrics')

1. Next, when you open the **Outputs + logs** tab you will observe the model artifacts that were logged via MLflow tracking APIs.

    ![Model artifacts](images/04-01-03-01-AML-artifacts.png 'Model Artifacts')

## Clean-up

If you're finished working with Azure Databricks for now, in Azure Databricks workspace, on the **Clusters** page, select your cluster and select **Terminate** to shut it down. Otherwise, leave it running for the next exercise.
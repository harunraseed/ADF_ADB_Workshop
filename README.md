---
title: Run a Databricks Notebook with the activity
description: "Learn how you can use the Databricks Notebook Activity in an Azure data factory to run a Databricks notebook against the databricks jobs cluster."
---

# Run a Databricks notebook with the Databricks Notebook Activity in Azure Data Factory


In this Workshop, you use the Azure portal to create an Azure Data Factory pipeline that executes a Databricks notebook against the Databricks jobs cluster. It also passes Azure Data Factory parameters to the Databricks notebook during execution.

You perform the following steps in this tutorial:

  - Create a data factory.

  - Create a pipeline that uses Databricks Notebook Activity.

  - Trigger a pipeline run.

  - Monitor the pipeline run.

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) before you begin.


  - **Azure Databricks workspace**. [Create a Databricks workspace](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal) or use an existing one. You create a Python notebook in your Azure Databricks workspace. Then you execute the notebook and pass parameters to it using Azure Data Factory.

## Create a data factory

1. Launch **Microsoft Edge** or **Google Chrome** web browser. Currently, Data Factory UI is supported only in Microsoft Edge and Google Chrome web browsers.

1. Select **Create a resource** on the Azure portal menu, select **Integration**, and then select **Data Factory**.
![new-azure-data-factory-menu](https://user-images.githubusercontent.com/20214629/210101139-b2094315-547b-457a-9822-a95e15a035be.png)

   

1. On the **Create Data Factory** page, under **Basics** tab, select your Azure **Subscription** in which you want to create the data factory.

1. For **Resource Group**, take one of the following steps:
    
    1. Select an existing resource group from the drop-down list.
    
    1. Select **Create new**, and enter the name of a new resource group.


1. For **Region**, select the location for the data factory.

    The list shows only locations that Data Factory supports, and where your Azure Data Factory meta data will be stored. The associated data stores (like Azure Storage and Azure SQL Database) and computes (like Azure HDInsight) that Data Factory uses can run in other regions.

1. For **Name**, enter **ADFTutorialDataFactory**.
    
    The name of the Azure data factory must be *globally unique*. If you see the following error, change the name of the data factory (For example, use **&lt;yourname&gt;ADFTutorialDataFactory**). 
![name-not-available-error](https://user-images.githubusercontent.com/20214629/210101196-de1c30ff-d423-4ceb-92b7-f0b7b506edc8.png)


1. For **Version**, select **V2**.

1. Select **Next: Git configuration**, and then select **Configure Git later** check box.

1. Select **Review + create**, and select **Create** after the validation is passed. 

1. After the creation is complete, select **Go to resource** to navigate to the **Data Factory** page. Select the **Open Azure Data Factory Studio** tile to start the Azure Data Factory user interface (UI) application on a separate browser tab.

![data-factory-home-page](https://user-images.githubusercontent.com/20214629/210101204-746458c6-6bb4-4d8c-a8e7-fdbfc091eab6.png)

## Create Azure Databricks

In this section, You are going to create a Azure Databricks service from Azure portal.

![My Image](Images/ADB_Create.PNG)

![My Image](Images/ADB_Create_01.PNG)

![My Image](Images/ADB_Create_02.PNG)

## Create linked services

In this section, you author a Databricks linked service. This linked service contains the connection information to the Databricks cluster:

### Create an Azure Databricks linked service

1.  On the home page, switch to the **Manage** tab in the left panel.
![get-started-page-manage-button](https://user-images.githubusercontent.com/20214629/210101217-6ffe3311-4d6d-40d7-b314-507d937f8bc4.png)


1.  Select **Linked services** under **Connections**, and then select **+ New**.
    

![databricks-notebook-activity-image-6](https://user-images.githubusercontent.com/20214629/210101223-3577167b-636a-482e-83bb-b152e3b44301.png)

1.  In the **New linked service** window, select **Compute** &gt; **Azure Databricks**, and then select **Continue**.
    
![databricks-notebook-activity-image-7](https://user-images.githubusercontent.com/20214629/210101232-69203136-886d-438c-b0e4-08b856a83837.png)


1.  In the **New linked service** window, complete the following steps:
    
    1.  For **Name**, enter ***AzureDatabricks\_LinkedService***.
    
    1.  Select the appropriate **Databricks workspace** that you will run your notebook in.

    1.  For **Select cluster**, select **New job cluster**.
    
    1.  For **Databrick Workspace URL**, the information should be auto-populated.

    1.  For **Access Token**, generate it from Azure Databricks workplace. You can find the steps [here](https://docs.databricks.com/api/latest/authentication.html#generate-token).

    1.  For **Cluster version**, select the version you want to use.

    1.  For **Cluster node type**, select **Standard\_D3\_v2** under **General Purpose (HDD)** category for this tutorial. 
    
    1.  For **Workers**, enter **2**.
    
    1.  Select **Create**.

![new-databricks-linked-service](Images/ADB_Linkedservice.PNG)


## Create a pipeline

1.  Select the **+** (plus) button, and then select **Pipeline** on the menu.

![databricks-notebook-activity-image-9](https://user-images.githubusercontent.com/20214629/210101266-070e5d74-8301-4299-bd95-3275ccf13b00.png)


1.  Create a **parameter** to be used in the **Pipeline**. Later you pass this parameter to the Databricks Notebook Activity. In the empty pipeline, select the **Parameters** tab, then select **+ New** and name it as '**name**'.
![databricks-notebook-activity-image-10](https://user-images.githubusercontent.com/20214629/210101275-e0262b60-e6f9-4ed3-81cd-b892fd62a14c.png)


![databricks-notebook-activity-image-11](https://user-images.githubusercontent.com/20214629/210101280-c26a434e-949c-498a-8009-755c996d8644.png)



1.  In the **Activities** toolbox, expand **Databricks**. Drag the **Notebook** activity from the **Activities** toolbox to the pipeline designer surface.

![new-adf-pipeline](https://user-images.githubusercontent.com/20214629/210101511-8df7ca47-1ce9-472c-af77-5c367c8ce528.png)

![new-adf-pipeline_config](Images/ADB_Select_LS_ADF.PNG)

![new-adf-pipeline_config_01](Images/ADB_Notebook.jpg)


1.  In the properties for the **Databricks** **Notebook** activity window at the bottom, complete the following steps:

    1. Switch to the **Azure Databricks** tab.

    1. Select **AzureDatabricks\_LinkedService** (which you created in the previous procedure).

    1. Switch to the **Settings** tab.

    1. Browse to select a Databricks **Notebook path**. Let’s create a notebook and specify the path here. You get the Notebook Path by following the next few steps.

       1. Launch your Azure Databricks Workspace.

       1. Create a **New Folder** in Workplace and call it as **adftutorial**.

             ![databricks-notebook-activity-image13](https://user-images.githubusercontent.com/20214629/210101539-50d2db66-4390-465f-bfc7-08b95702002c.png)
    

       1. [Screenshot showing how to create a new notebook.](https://docs.databricks.com/user-guide/notebooks/index.html#creating-a-notebook) (Python), let’s call it **mynotebook** under **adftutorial** Folder, click **Create.**

           ![databricks-notebook-activity-image14](https://user-images.githubusercontent.com/20214629/210101559-f95f3973-4d2f-488f-aacd-17f3757cf7db.png)

![databricks-notebook-activity-image15](https://user-images.githubusercontent.com/20214629/210101571-cd5eb829-c7d1-4418-becf-d583e061b480.png)

          

       1. In the newly created notebook "mynotebook'" add the following code:

           ```
           # Creating widgets for leveraging parameters, and printing the parameters
           dbutils.widgets.text("input", "","")
           y = dbutils.widgets.get("input")
           print ("Param -\'input':")
           print (y)
           ```

       The **Notebook Path** in this case is **/adftutorial/mynotebook**.


![databricks-notebook-activity-image16](https://user-images.githubusercontent.com/20214629/210101587-813d6502-1214-49ea-a479-0969b0217407.png)

1.  Switch back to the **Data Factory UI authoring tool**. Navigate to **Settings** Tab under the **Notebook1** activity.

    a.  Add a **parameter** to the Notebook activity. You use the same parameter that you added earlier to the **Pipeline**.
![new-adf-parameters](https://user-images.githubusercontent.com/20214629/210101599-533a9a3c-4514-4391-af64-21486b800914.png)


    b.  Name the parameter as **input** and provide the value as expression **\@pipeline().parameters.name**.

1.  To validate the pipeline, select the **Validate** button on the toolbar. To close the validation window, select the **Close** button.

![databricks-notebook-activity-image-18](https://user-images.githubusercontent.com/20214629/210101614-9b32c290-41c2-4f49-804c-96e26299ebd4.png)


1.  Select **Publish all**. The Data Factory UI publishes entities (linked services and pipeline) to the Azure Data Factory service.
![databricks-notebook-activity-image-19](https://user-images.githubusercontent.com/20214629/210101630-61bea116-d1cf-4481-aa17-babb8a7521ad.png)



## Trigger a pipeline run

Select **Add trigger** on the toolbar, and then select **Trigger now**.
![databricks-notebook-activity-image-20](https://user-images.githubusercontent.com/20214629/210101640-294aae04-8e7d-45b4-9207-78b7a1d69719.png)


The **Pipeline run** dialog box asks for the **name** parameter. Use **/path/filename** as the parameter here. Select **OK**.

![databricks-notebook-activity-image-21](https://user-images.githubusercontent.com/20214629/210101648-9c52441c-5d1e-4b92-9b2c-19e24fa92f34.png)


## Monitor the pipeline run

1.  Switch to the **Monitor** tab. Confirm that you see a pipeline run. It takes approximately 5-8 minutes to create a Databricks job cluster, where the notebook is executed.

![databricks-notebook-activity-image-22](https://user-images.githubusercontent.com/20214629/210101661-71448849-91a2-4819-b863-771832b76bbd.png)


1.  Select **Refresh** periodically to check the status of the pipeline run.

1.  To see activity runs associated with the pipeline run, select **pipeline1** link in the **Pipeline name** column.

1. In the **Activity runs** page, select **Output** in the **Activity name** column to view the output of each activity, and you can find the link to Databricks logs in the **Output** pane for more detailed Spark logs.

1. You can switch back to the pipeline runs view by selecting the **All pipeline runs** link in the breadcrumb menu at the top.

## Verify the output

You can log on to the **Azure Databricks workspace**, go to **Clusters** and you can see the **Job** status as *pending execution, running, or terminated*.
![databricks-notebook-activity-image24](https://user-images.githubusercontent.com/20214629/210101678-ec009bbe-9aaf-456e-882f-f0df713059d5.png)



You can click on the **Job name** and navigate to see further details. On successful run, you can validate the parameters passed and the output of the Python notebook.
![databricks-output](https://user-images.githubusercontent.com/20214629/210101685-c5dfa667-d48d-4837-924f-8bc99e1dd294.png)



## Next steps

The pipeline in this sample triggers a Databricks Notebook activity and passes a parameter to it. You learned how to:

  - Create a data factory.

  - Create a pipeline that uses a Databricks Notebook activity.

  - Trigger a pipeline run.

  - Monitor the pipeline run.

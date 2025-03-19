<img src = "../assets/images/microsoft-logo.png" height = 75/>

# End-to-End Data Engineering:<br>Modern Data Warehousing on Microsoft Fabric

## Lab 5 - Orchestrating warehouse operations
In this lab you will do something. 

### 5.1 - Building an orchestration pipeline

1. Return to the **Modern Data Warehousing on Microsoft Fabric** workspace item list by selecting the workspace name from the left navigation bar. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Select **New item**.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. From the **All items** view, locate the **Get data** section and select the **Data pipeline** tile.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. On the **New pipeline** dialog box, enter the name **Load Dimensional Model** and select **Create**. The pipeline will be created and open to a blank canvas with a set of links to get started quickly. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Select the **Pipeline activity** tile and select **Script**.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Select the newly created **Script1** activity on the canvas. The bottom pane will change to display General and Settings for the script activity.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. With the newly created activity selected, on the **General** page, enter **SQL Load Stage FactSale** for the the **Name** of the activity. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Navigate to the **Settings** page.

    - From the **Connection** dropdown select the **WideWorldImportersDW** warehouse.
    - Leave the **Script** option as the default, **Query**.
    - Select the **pencil icon** to the right of the script box to open the editor.
    - Enter the following code, which was also used in Lab 3 to copy the sales data from the lakehouse to the warehouse stage table.

    ``` sql
    TRUNCATE TABLE stage.FactSale

    INSERT INTO stage.FactSale
    SELECT
        [WWICityID]
        ,[WWICustomerID]
        ,[WWIBillToCustomerID]
        ,[WWIStockItemID]
        ,[InvoiceDateKey]
        ,[DeliveryDateKey]
        ,[WWISalespersonID]
        ,[WWIInvoiceID]
        ,[Description]
        ,[Package]
        ,[Quantity]
        ,[UnitPrice]
        ,[TaxRate]
        ,[TotalExcludingTax]
        ,[TaxAmount]
        ,[Profit]
        ,[TotalIncludingTax]
        ,[TotalDryItems]
        ,[TotalChillerItems]
    FROM WideWorldImporters.dbo.Sale
    ```

    - Select **OK**.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. From the ribbon, navigate to the **Activities** tab. Select the **Script** activity to add it to the canvas. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. With the newly created activity selected, on the **General** page, enter **SQL Load Stage Dimensions** for the the **Name** of the activity. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Navigate to the **Settings** page.

    - From the **Connection** dropdown select the **WideWorldImportersDW** warehouse.
    - Leave the **Script** option as the default, **Query**.
    - Select the **pencil icon** to the right of the script box to open the editor.
    - Enter the following code, which is a modified version of what was used in Lab 3 to copy the data from Azure storage to the warehouse stage tables for all the dimensions.

    ``` sql
    TRUNCATE TABLE stage.DimCity
    TRUNCATE TABLE stage.DimCustomer
    TRUNCATE TABLE stage.DimEmployee
    TRUNCATE TABLE stage.DimStockItem

    COPY INTO [stage].[DimCity] FROM 'https://scbradlstorage01.dfs.core.windows.net/sampledata/WWI/DimCity.parquet' WITH (FILE_TYPE = 'PARQUET');
    COPY INTO [stage].[DimCustomer] FROM 'https://scbradlstorage01.dfs.core.windows.net/sampledata/WWI/DimCustomer.parquet' WITH (FILE_TYPE = 'PARQUET');
    COPY INTO [stage].[DimEmployee] FROM 'https://scbradlstorage01.dfs.core.windows.net/sampledata/WWI/DimEmployee.parquet' WITH (FILE_TYPE = 'PARQUET');
    COPY INTO [stage].[DimStockItem] FROM 'https://scbradlstorage01.dfs.core.windows.net/sampledata/WWI/DimStockItem.parquet' WITH (FILE_TYPE = 'PARQUET');
    ```

    - Select **OK**.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. From the ribbon, navigate to the **Activities** tab. Select the **Script** activity one final time to add it to the canvas. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. With the newly created activity selected, on the **General** page, enter **SQL Transform to Dimensional Model** for the the **Name** of the activity. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Navigate to the **Settings** page.

    - From the **Connection** dropdown select the **WideWorldImportersDW** warehouse.
    - Leave the **Script** option as the default, **Query**.
    - Select the **pencil icon** to the right of the script box to open the editor.
    - Enter the following code, which is a modified version of what was used in Lab 3 to copy the data from Azure storage to the warehouse stage tables for all the dimensions.

    ``` sql
    EXEC dbo.UpdateDimCity;
    EXEC dbo.UpdateDimCustomer;
    EXEC dbo.UpdateDimEmployee;
    EXEC dbo.UpdateDimStockItem;
    EXEC dbo.UpdateFactSale;
    ```

    - Select **OK**.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. With all three scripts configured, it is time to connect them to ensure the proper processing order is followed. Stage tables should be loaded before the dimensional model is updated. Locate the **checkbox** on the right side of the **SQL Load Stage FactSale** script activity. Click and hold on the checkbox to select the **On success** output. While continuing to hold the mouse buttong, drag and drop the arrow onto the **SQL Transform to Dimensional Model** script activity. A green line should now connect the two activities.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Repeat the prior step to connect the **SQL Load Stage Dimensions** script activity to the **SQL Transform to Dimensional Model** script activity. Click and hold the **on success** checkbox on the right side of the SQL Load Stage Dimensions activity, then drag and drop it on the SQL Transform to Dimensional model script activity. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Click the auto align canvas button on the canvas settings to better align the activities. Verify the pipeline is configured to load stage successfully before moving to update the dimensional model. The diagram should look similar to the image below.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Navigate to the **Home** tab on the ribbon and select **Save**.

### 5.2 - Scheduling the pipeline

1. While still in the pipeline editor, select **Schedule** from the **Home** tab of the ribbon.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Configure the schedule with the following settings.

    - Change the **Scheduled run** radio button to **On**.
    - Repeat: **By the minute**
    - Every: **1** minute(s)
    - Start date and time: Select the calendar icon and select **today's date**. This should populate with the current date and time. Change the time to **9:00 AM** to align with the start time of the workshop.
    - End date and time: Select the calendar icon and select **today's date**. This should populate with the current date and time. Change the time to **4:00 PM** to align with the conclusion of the workshop.
    - Time zone: **(UTC-8:00) Pacific Time (US and Canada)** to align with the time zone of the workshop.
    - Note at the top of the screen the next refresh delayed for should say **less than a minute**.
    - Select **Apply**.

1. Wait for at least 1 minute for the next scheduled run of the pipeline to start then select **View run history** from the **Home** tab of the ribbon.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Verify the pipeline's schedule is working by reviewing the recent runs list. This list is slightly delayed and may show a status of **Not started** even though the pipeline has completed. Select the **X** in the top right corner to close the recent runs pane. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

## Next steps
In this lab, we did something.

- Continue to lab the [Lab 6 - Data transformation using T-SQL](<04 - Data transformation using T-SQL.md>) lab.

### Additional Resources
<li><a href="https://learn.microsoft.com/en-us/fabric/fundamentals/create-workspaces" targer="_blank">Create a workspace</a></li>
<li><a href="https://learn.microsoft.com/en-us/fabric/data-warehouse/create-warehouse" targer="_blank">Create a Warehouse in Microsoft Fabric</a></li>
<img src = "../assets/images/microsoft-logo.png" height = 75/>

# End-to-End Data Engineering:<br>Modern Data Warehousing on Microsoft Fabric

## Lab 4 - Data transformation using T-SQL
In this lab you will do something. 

### 4.1 - Stored procedures

1. If you just completed Lab 3, remain in The Workshop notebook. If necessary, reopen the notebook by selecting **The Workshop** notebook from the workspace's item list view. 

1. Create the stored procedures to perform an UPSERT or MERGE operation on the dimensional model tables by running the fist cell in the **4.1 - Stored procedures** section of The Workshop notebook.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. In the **Explorer**, refresh the warehouse object list by selecting the ellipsis (**...**) that appears when hovering the mouse of the warehouse name WideWorldImportersDW, then selecting **Refresh**.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Expand **WideWorldImportersDW -> Schemas -> dbo -> Stored Procedures** to validate that all the procedures were created successfully.

    - dbo.UpdateDimCity
    - dbo.UpdateDimCustomer
    - dbo.UpdateDimEmployee
    - dbo.UpdateDimStockItem
    - dbo.UpdateFactSale

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

### 4.2 - Incrementally updating tables

1. Execute all the stored procedures created in the previous section by running the code in the cell in the **4.2 - Incrementally updating tables** section of The Workshop notebook.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

2. Upon completion, compare the results with those in the screenshot below. The code in the executed cell performed several steps:

    - Store the record count for each of the dimensional model tables before any data is transformed from stage -> dimensional model.
    - Run all the five stored procedures to load the dimensional model tables (one procedure per table).
    - Display the record count from before the data load and after the data load to ensure data was properly loaded.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

## Next steps
In this lab, we did something.

- Continue to lab the [Lab 5 - Orchestrating warehouse operations](<05 - Orchestrating warehouse operations.md>) lab.

### Additional Resources
<li><a href="https://learn.microsoft.com/en-us/fabric/fundamentals/create-workspaces" targer="_blank">Create a workspace</a></li>
<li><a href="https://learn.microsoft.com/en-us/fabric/data-warehouse/create-warehouse" targer="_blank">Create a Warehouse in Microsoft Fabric</a></li>
<img src = "../assets/images/microsoft-logo.png" height = 75/>

# End-to-End Data Engineering:<br>Modern Data Warehousing on Microsoft Fabric

## Lab 6 - Advanced query techniques
In this lab you will do something. 

### 6.1 - Time travel

1. Return to the **Modern Data Warehousing on Microsoft Fabric** workspace item list by selecting the workspace name from the left navigation bar. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. From the item list, select The Workshop notebook.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Run the first cell in the **6.1 Time travel** section of The Workshop notebook to check the number of records in *dbo.FactSale* and *stage.FactSale* for January 1, 2013.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Note from the results that the data for January 1, 2013 no longer exists in the stage table. This means if the data were deleted or changed there would be no way to easily recover it from the source system. Let's delete the data! Run the next cell in The Workshop notebook. This cell deletes the 89 records from January 1, 2013 and verifies they were removed by performing a count. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. We can validate that the data looked a certain way in the past using time travel. Run the next cell in the notebook which uses a dynamic SQL statement to do a count on the table as it looked 30 minutes ago. Using this query, we can verify there are in fact 89 records that are missing.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

Now that we know the data exists, let's look at how table clones can help us recover. 

### 6.2 - Clone a table

1. Run the first cell in the **6.2 - Clone a table** section of The Workshop notebook which uses a dynamic SQL statement to create a clone of the dbo.FactSale table as of about 30 minutes ago when the data from January 1, 2013 was still present in the table. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Run the next cell to count the number of records in the cloned table and compare it to the record count in the stage and dimensional model tables. Again, notice there are no records in stage.FactSale or dbo.FactSale but there are 89 records in dbo.FactSale_Recovery. 

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

1. Next, add the records back to the dbo.FactSale table and verify the operation had the intended effect by comparing records counts one final time by running the next cell in the notebook.

    <img src = "../assets/images/microsoft-logo.png" height = 75/>

## Next steps
In this lab, we did something.

- Continue to lab the [Lab 7 - Data warehouse management](<07 - Data warehouse management.md>) lab.

### Additional Resources
<li><a href="https://learn.microsoft.com/en-us/fabric/fundamentals/create-workspaces" targer="_blank">Create a workspace</a></li>
<li><a href="https://learn.microsoft.com/en-us/fabric/data-warehouse/create-warehouse" targer="_blank">Create a Warehouse in Microsoft Fabric</a></li>
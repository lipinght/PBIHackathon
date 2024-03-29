# Lab 2 - Use Composite Model
This lab walks through how to connect with DirectQuery to a dataset published to Power BI Service and how to enrich this model by including other data connections in Import mode.

**Note:** In order to do this lab, you will need access to a Power BI workspace. Do the [prerequisite](https://github.com/lipinght/PBIHackathon/blob/ninamun-compmodels-wip/CompositeModels/Prerequisite.md) before starting the lab.
This lab requires completion of the **Lab 1 - DirectQuery for Power BI datasets** lab.

1. Download locally on your computer the following Excel spreadsheet: [Promotions.xlxs](https://github.com/lipinght/PBIHackathon/blob/main/CompositeModels/Promotions.xlsx)

2. In Power BI Desktop, open the .pbix file from the previous lab **Lab 1 - DirectQuery for Power BI datasets**

3. Click on **Get Data**, choose **Excel Workbook** and then navigate to the **Promotions.xlxs** spreadsheet you downloaded locally

4. Select the **Promotions** checkbox in the Navigator window and click **Load** button

![cm1.png](images/cm1.png)

5. A warning window about a potential security risk shows up, since now you are mixing local data with data in the cloud and queries will go back and forth, exchanging data. Click **OK**

![cm2.png](images/cm2.png)

6. Promotions data from the local spreadsheet is now loading into Power BI Desktop and will show up in the **Fields** panel

7. Navigate to the **Data** view and click on the **Promotions** table in the **Fields** panel. The new table contains the data that’s imported from Excel.

![cm3.png](images/cm3.png)

![cm4.png](images/cm4.png)

8. Navigate to **Model** view in Power BI Desktop, you can now see an additional table called **Promotions**, not related to any of the other tables in the model

![cm5.png](images/cm5.png)

9. We now need to relate the **Promotions** table to the other tables in the model. To create a relationship between the **Sales** table from the Contoso PBI dataset and the imported **Promotions** table, click on the **Manage Relationships** menu item, then click on **New…**

10. In the **Create relationship**  window select **Sales**. Automatically the window gets populated with sample Sales records and sample Promotions records, with PromotionKey field highlighted in both tables.

![cm6.png](images/cm6.png)

11. Check **Cardinality** and make sure it is set to **Many to one(*:1)**. Click **OK**. Click **Close**

12. Click on **Promotions** table, right click **PromotionsKey** field and select **Hide in report view**

13. Navigate back to the **Report** view

14.	Create a new report page, keep the default name **Page 2**

15. From the **Visualisations** panel click on **Table** visual

16. From the **Fields** panel expand Product and select **BrandName**

17. From the **Fields** panel expand Sales and select **SalesAmount**

18.	From the **Fields** panel expand Promotions and select **PromotionName**

19. From the **Fields** panel expand Promotions and select **DiscountPercent**

20. Navigate to the **Data** view, click on the **DiscountPercentage** column, click on the Percentage button on the Menu ribbon then change the number of decimal places shown for this column to 0

![cm7.png](images/cm7.png)

21. Navigate to the **Report** view. Now the **DiscountPercent** values should be displayed as percentages on your report

22. From the **Visualizations** panel add a **Slicer** visual

![cm7_1.png](images/cm7_1.png)

23. In the **Fields** panel expand Calendar and select Year to populate the slicer

24. Click on the Year slicer visual on the report and from the right upper corner, click on the arrow to select **Dropdown** option

![cm8.png](images/cm8.png)

25. From the **Visualizations** panel add a Slicer visual

26. In the **Fields** panel expand Promotions and select StartDate to populate the slicer

27. Click on the StartDate slicer visual on the report and from the right upper corner click on the arrow to select **Dropdown** option

![cm9.png](images/cm9.png)

28. Select 2013 in the **Year** slicer and 01/11/2013 in the **StartDate** one

![cm9_1.png](images/cm9_1.png)

# Refresh the Composite Model

29. Open the local **Promotions.xlxs** spreadsheet. Look for **Asian Holiday Promotion**, with PromotionLabel = 23. Modify **DiscountPercentage** from 0.15 to 0.25. Save then close the spreadsheet

30. In Power BI Desktop, click the **Refresh** button on the menu ribbon. The refresh operation is loading again only the data from the Promotions spreadsheet. 

31. Now the DiscountPercent values are changed in the report.

![cm10.png](images/cm10.png)

32. In Power BI Desktop, save your changes.



# Add measures to a Composite Model

33. In the same Power BI Desktop .pbix file, create a new report page by right click-ing on the current page name **Page 2** and select **Duplicate Page**

34. Rename the new page **Page 3**

35. Go to **Page 3** and in your report click on the table visual; in the **Visualisations** panel under Columns, remove column BrandName

36. Click on your table visual from **Page 3**, and in the **Fields** panel right click on Sales, then select **New Measure**

37. Copy and Paste the following DAX code for the new measure **NumberOfProducts**, then hit **Enter**
 
<mark>NumberOfProducts = COUNTROWS ('Product')</mark>

38. Add the new measure to your report by clicking on your table visual and in the **Fields** panel > **Sales**, select **NumberOfProducts**

![cm11.png](images/cm11.png)

39. In Power BI Desktop, create a new report page by right click-ing on the current page name **Page 3** and select **Duplicate Page**; rename the new page as **Page 4**

40. Click on your table visual in **Page 4**, and in the **Visualisations** panel under Columns, remove columns SalesAmount, NumberOfProducts

41. Click on your table visual in **Page 4**, and in the **Fields** panel right click on Sales, then select **New Measure**

42. Copy and Paste the following DAX code for the new measure **SumOfSales**, then hit **Enter**

<mark>SumOfSales = SUM(Sales[SalesAmount])</mark>

43. Click on your table visual, and in the **Fields** panel right click on Sales, then select **New Measure**

44. Copy and Paste the following DAX code for the new measure **YTDSumOfSales**, then hit **Enter** 

<mark>YTDSumOfSales = TOTALYTD([SumOfSales],'Calendar'[DateKey])</mark>

45. Click on your table visual, and in the **Fields** panel > **Sales**, select **YTDSumOfSales**

46. On the menu ribbon, **Measure** tools, format YTDSumOfSales measure by adding comma as a thousands separator and add a currency symbol

![cm12.png](images/cm12.png)


# Use Many-to-many Relationships in a Composite Model

47. Download locally on your computer the following Excel spreadsheet: [ProductManagers.xlxs](https://github.com/lipinght/PBIHackathon/blob/main/CompositeModels/ProductManagers.xlsx)

48. In Power BI Desktop, click on **Get Data**, choose **Excel Workbook** and then navigate to the **ProductManagers.xlxs** spreadsheet you downloaded locally

49. Select the **ProductManagers** checkbox in the Navigator window and click **Transform Data** button to update the column names Column1 and Column2 to something meaningful

![cm13.png](images/cm13.png)

50. In the Power Query Editor window, click on the table icon left of Column1 and select **Use First Row as Headers** then **Close & Apply**

![cm14.png](images/cm14.png)

51. We now need to relate the **ProductManagers** table to the other tables in the model. In the **Model** view, to create a relationship between the **Product** table from the Contoso PBI dataset and the imported **ProductManagers** table, click on the **Manage Relationships** menu item, then click on **New…**

52. In the **Create relationship**  window select **Product** in the first dropdown and **ProductManagers** in the second one.

53. By default all relationships that go across source default to many-to-many cardinality. Set the **Cross filter direction** to Single. Click **OK**. Click **Close**

![cm15.png](images/cm15.png)

54. Click on **ProductManagers** table, right click **BrandName** field and select **Hide in report view**

55. Navigate to the **Report** view and create a new report page. Add a **Table** visual

56. In the **Fields** panel > **ProductManagers**, select **ProductManager**

57. In the **Fields** panel > **Sales**, select **SumOfSales**

![cm16.png](images/cm16.png)

58. Save your changes
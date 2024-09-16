# The Basics of Power BI using the Contoso Dataset

# Anatomy of PowerBI

Power BI is a comprehensive data visualization tool, and understanding its key components is essential to making the most of it.

- **Canvas view**: The central area of Power BI where visualizations are placed and arranged. It serves as the workspace for creating your report. You can drag fields from the Fields pane into the canvas to create visualizations, or use pre-built visualizations from the Visualizations pane. 🖼️
  
- **Visualisations pane**: Found on the right side of the screen, the Visualizations pane is where you select and customize the visual types (e.g., bar charts, line graphs, pie charts). You can adjust formatting, colors, and axes for each visualization here. 📊

- **Filter pane**: This pane allows you to apply filters to visualizations and reports. You can apply filters at the visual, page, or report level, controlling what data is displayed and how. Found to the right of the canvas. 🔍

- **Fields pane**: Located beneath the Filter pane, this section displays the data fields you have available in your current data model. You can drag fields from here onto the canvas to create visualizations or into the filter area to filter your data. 📁

- **Ribbons**: These are found at the top of Power BI and offer various options such as importing data, managing relationships, and publishing reports. The ribbons include tabs like Home, View, and Modeling, each providing different actions and settings. 🏅

- **PowerQuery**: This is where data preparation happens in Power BI. You can access Power Query by selecting "Transform Data" from the ribbon. It allows you to connect to multiple data sources, clean and transform data, and load it into Power BI for analysis. 🛠️

- **Tabular view**: Accessible through the Data view tab on the left side of Power BI, this view displays your dataset in a spreadsheet-like format. It allows you to inspect your data in a tabular form to ensure everything looks correct before building visualizations. 📋

- **Data model view**: This view shows the relationships between different tables in your dataset. It’s useful for managing and building relationships between your data tables. You can access this by selecting the Model view tab (third icon on the left). 🔗


## Importing Data



## Preparing the Data for Visualisation

- **Relationships 101**
  - Star schema/snowflake schema
  - Can you have more than one fact table? (Yes!) 
  - Relationship cardinality (1:1 - 1:*, and *:* advisability)
  - Creating a relationship in PowerBI - how-to: 
    - Candidate: DimDate `datekey` to Fact Table `datekey`
    - Delete and re-make
    - Date Table - `DimDateKey` label as a date table - this allows PowerBI to pick up the date format of the date data in this table. 📅

## Power Query Tour

Power Query is used for data preparation and transformation within Power BI.
- Key Tasks:
  - **Data Source Connection**: Connect to various data sources such as Excel files, CSV files, databases, web services, and more. 🌐
  - **Data Import**: Import data from the connected data sources into Power BI for analysis and visualization.
  - **Data Cleaning**: Cleanse and standardize data by removing duplicates, correcting errors, and formatting data types. 🧹
  - **Data Transformation**: Perform various transformations on the data, such as splitting columns, merging tables, filtering rows, and transposing data. 🔄
  - **Data Enrichment**: Enrich the dataset by adding calculated columns, applying formulas, and creating custom columns based on existing data.
  - **Data Aggregation**: Aggregate and summarise data by grouping rows, calculating totals, averages, counts, and other summary statistics.
  - **Data Merging and Appending**: Combine multiple datasets by merging or appending them together to create a unified dataset for analysis. 🔗
  - **Data Pivot and Unpivot**: Pivot or unpivot data to restructure it for better analysis and visualisation.

## DimGeography Hierarchy

- **What is a hierarchy and why is it useful in PowerBI?**
  - Hierarchies enable users to drill down into data at different levels of granularity.
  - Create hierarchy in DimGeography - Right-click on Continent, rename hierarchy:
    - Continent 🌍
    - Country 🏳️
    - State 🏙️
    - City 🏘️

## Measures

- **Create Measures table**:
  - Why? Keeps Measures segregated and easy to find and use.
  - Click ‘Enter Data’ and create a blank table - this will be an organisational object only.

### Basic Measures:

- **CostOfGoodsSold (COGS)** = `SUM(Sales[TotalCost])`
- **Total Sales** = `SUM(Sales[Sales])`
- **Net Profit** = `[Total Sales] - [COGS]`
- **Quantity Sold** = `SUM(Sales[SalesQuantity])`
- **Profit Margin** = `DIVIDE([Net Profit],[Total Sales],0)`

### Time Intelligence Measures:

- **Sales Year to Date**:
  ```DAX
  Sales Year to Date = CALCULATE([Total Sales], DATESYTD(DimDate[Datekey]))
  ```
- **Sales Last Year**:
  ```DAX
  Sales LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(DimDate[Datekey]))
  ```
- **Net Profits Running Total**:
  ```DAX
  Net Profits Running Total =
  CALCULATE(
      [Net Profit],
      FILTER(
          ALLSELECTED('DimDate'[Datekey]),
          ISONORAFTER('DimDate'[Datekey], MAX('DimDate'[Datekey]), DESC)
      )
  )
  ```

## Report Elements

- Good practice:
  - Unified visual style 🎨
  - Consistent colour palettes 🖌️
  - Consistent fonts and font sizes ✏️
  - Consistent approach to labelling
  - Using appropriate units (e.g., currency symbols, orders of magnitude)
  - Colour-blind and other visual impairment adjustments ♿

## Visuals

- **Contoso Sales Report Page 1:**
  - **Sales by Country** (Map Visual):
    - Requires a geographical hierarchy with valid locations (e.g., country, state, city).
    - Demonstrate drill-down capability with USA.
  - **Net Profit by Continent** (Ring Chart): Hierarchy for geography.
  - **Net Profit by Channel** (Ring Chart): By sales channel.
  - **Sales by City** (Stacked Bar Chart).
  - **Sales by Product Category** (Bar Chart).
  - **Total Sales by Date** (Line Chart).

- **Contoso Sales Report Page 2: Profit Analysis**:
  - **City Sales Analysis Table**: Columns: `CityName`, `Net profit`, `Profit Margin`.
  - **ProductCategory Analysis Table**: Columns: `ProductCategoryName`, `Net profit`, `Profit Margin`.
  - **Sales and Profit Margin by Product Category** (Scatter Chart).
  - **Net Profits Running Total Month by Month** (Line Chart).
  - **Net Profit by City** (Map Visual).



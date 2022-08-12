# SplitQVDGeneration
A Qlik Cloud technique to split QVD generation into smaller, faster workloads


# This archive contains 3 components
1. Qlik App Automation Template
2. Qlik Sense App Template for Split QVD Generation
3. Qlik Sense App Template to determine the dimension values for the QVD splitting 

# Steps to Setup

Setup a Dimensional QVD App
1. Import the "Dimensional QVD App" into Qlik SaaS
2. Modify the load script to load data values from a data source that represent the split QVD data sets. For example, if you have data with a region name (like Americas, EMEA, ASIAPAC) , load the Regions in the load script.  The QVDs will be built using the same names (Americas, EMEA, ASIAPAC) and the values will be used to filter the QVD loads.  TIP:  It is a good idea to pick a dimension that with values that evenly distribute the data volume. A week, month, or day field may be a good candidate. 
3. Reload the "Dimensional QVD App"
4. In the Sheet Editor,  edit the master items / dimensions to include the dimension you would like to split the QVDs on

NOTES: At runtime, the dimensional QVD app will be dynamically reloaded prior to fetching the dimension values for the QVD split. Ensure the load script is fast. The load script can be static (INLINE loads) or it can be dynamic (loading from a SQL source or other data source)

Setup a Split QVD Generator - Template App
1. Import the "Split QVD Generator - Template App" into Qlik SaaS
2. This is a QVD Generator app. Copy in the load statements from your existing QVD generator app.  Or author your data load to contain the data you wish to store in QVD format

NOTES:  When the template app is copied at runtime , it will be given a name that is equivalent to a dimensional value.  The template app load script, grabs the app name (documenttitle()) as a variable which can be manipulated or used as is in the WHERE clause of the load statement(s).  If the data is split unevenly whereby most of the data is loaded into just one of the QVDs, then the performance improvement vs. a monolithic QVD will be minimal. Performance is improved by splitting on dimensional values that result in similar data volumes and splitting into more QVDs. 

Setup the Split QVD Generation Automation Template
1. Create a new Qlik App Automation and choose 'blank Template'
2. Edit the Automation
3. In the blank work space, right click and select 'Import workspace'
4. Select the 'SplitQVDGeneration.json' automation
5. Once imported, there are 4 blocks that need updating:
   5A. "Do Reload" Block - Update the "AppId" .. Choose the DimensionalQVDApp
   5B.  "List Dimensional Values" Block - Update the "AppId" and the "Dimension Id"... CHoose the DimensionalQVDApp and choose the dimension you would like to split the QVDs on
   5C.  "Copy App" Bloack - Update the 'Space Id' and the 'App Id' ... choose a space where the "Split QVD Generator - Template App" resides.  choose the "Split QVD Generator - Template App" for the AppId

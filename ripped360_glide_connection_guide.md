# Guide: Using Your Ripped360 Data with Google Sheets and Glide

This guide will walk you through uploading the provided CSV files into Google Sheets and then connecting that Google Sheet to Glide to build your Ripped360 app.

## Part 1: Uploading CSV Files to Google Sheets

You have been provided with the following CSV files, each representing a tab in your Ripped360 app data:

- `ripped360_users.csv`
- `ripped360_workouts.csv`
- `ripped360_exercises.csv`
- `ripped360_supplements.csv`
- `ripped360_user_workouts.csv`
- `ripped360_user_supplements.csv`

Follow these steps to import them into a single Google Sheet:

1.  **Create a New Google Sheet:**
    *   Go to [Google Sheets](https://sheets.google.com).
    *   Click on "Blank" to create a new spreadsheet.
    *   Rename the spreadsheet to "Ripped360 App Data" by clicking on the default title (e.g., "Untitled spreadsheet") at the top left.

2.  **Import the First CSV (Users):**
    *   The first sheet is usually named "Sheet1". Right-click on the "Sheet1" tab at the bottom and select "Rename". Rename it to "Users".
    *   Click on "File" in the top menu, then "Import".
    *   Go to the "Upload" tab in the import dialog.
    *   Drag the `ripped360_users.csv` file into the upload area, or click "Select a file from your device" and locate it.
    *   In the "Import file" dialog that appears:
        *   For "Import location", choose "Replace current sheet".
        *   Ensure "Separator type" is "Detect automatically" or "Comma".
        *   Ensure "Convert text to numbers, dates, and formulas" is checked (usually the default).
        *   Click "Import data".
    *   The data from `ripped360_users.csv` (which is currently just the headers) will now populate the "Users" tab.

3.  **Import Remaining CSVs into New Tabs:**
    *   For each of the remaining CSV files (`ripped360_workouts.csv`, `ripped360_exercises.csv`, `ripped360_supplements.csv`, `ripped360_user_workouts.csv`, `ripped360_user_supplements.csv`), follow these sub-steps:
        *   Click the "+" button (Add Sheet) at the bottom left to create a new tab.
        *   Right-click the new sheet tab (e.g., "Sheet2") and rename it to match the data you are about to import (e.g., "Workouts", "Exercises", "Supplements", "UserWorkouts", "UserSupplements").
        *   Select the newly created and renamed tab.
        *   Click on "File" > "Import".
        *   Go to the "Upload" tab.
        *   Upload the corresponding CSV file (e.g., `ripped360_workouts.csv` for the "Workouts" tab).
        *   In the "Import file" dialog:
            *   For "Import location", choose "Replace current sheet".
            *   Click "Import data".

4.  **Verify All Tabs:**
    *   You should now have the following tabs in your "Ripped360 App Data" Google Sheet, each with the correct column headers:
        *   Users
        *   Workouts
        *   Exercises
        *   Supplements
        *   UserWorkouts
        *   UserSupplements

5.  **Populate with Data (Your Task):**
    *   Now, you need to **manually copy and paste your actual data** into each respective tab under the headers. The CSV files provided only contain the header rows.

6.  **Optional: Auto-ID Formula for Users Tab:**
    *   As mentioned in your guide, for the "Users" tab, you can add an auto-ID formula.
    *   In cell A2 (under the `UserID` header), enter the formula: `=IF(LEN(B2),"U"&TEXT(ROW()-1,"000"),"")`
    *   Drag the fill handle (the small square at the bottom-right of cell A2) down for as many rows as you anticipate having users. This will automatically generate UserIDs like "U001", "U002", etc., when a name is entered in column B.

## Part 2: Connecting Your Google Sheet to Glide

Once your "Ripped360 App Data" Google Sheet is prepared and populated with your data, follow these steps to create your app in Glide:

1.  **Sign Up/Log In to Glide:**
    *   Go to [Glide](https://www.glideapps.com/).
    *   Sign up for a free account if you don't have one, or log in.

2.  **Create a New App:**
    *   On your Glide dashboard, click "New App".
    *   Glide might ask for a name for your app here, or later. You can name it "Ripped360".
    *   Choose "Start with data" or "Start from spreadsheet" (the wording might vary slightly).

3.  **Select Google Sheets as Data Source:**
    *   Choose "Google Sheets" as the source of your data.
    *   You may need to authorize Glide to access your Google Drive/Sheets if you haven't done so before. Follow the prompts.

4.  **Choose Your Spreadsheet:**
    *   Glide will display a list of your Google Sheets. Select the "Ripped360 App Data" spreadsheet you prepared.
    *   Click "Select" or "Create App".

5.  **Glide Imports Your Data:**
    *   Glide will automatically detect all the tabs (Users, Workouts, Exercises, etc.) from your Google Sheet and import them as tables into your Glide app.
    *   It will usually try to create a basic layout for your app based on the data.

6.  **Rename Your App (if not done already):**
    *   If you haven't named your app "Ripped360" yet, find the app name (usually at the top left in the Glide editor) and change it.

Your data is now connected! You can proceed with configuring the app layout, design, screens, and logic within Glide as detailed in the subsequent parts of your Ripped360 App Creation Guide.

Remember to refer to the `todo.md` checklist for the next steps in building your app's features and interface in Glide.


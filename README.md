## Using the `getHeaders` Function

### Overview

The `getHeaders` function is a powerful utility for retrieving specific column headers from a Google Sheet. It works in a case-insensitive manner, ensuring that you get the correct column numbers for the headers you need.

### How to Use

1. **Include the Function**: Ensure you have the `getHeaders` function in your project. You can copy and paste it from the code snippet below.

   ```javascript
    function getHeaders(sheet, headerArray) {
        const sheetHeader = sheet.getRange("1:1").getValues();

        const lowercaseHeaders = sheetHeader[0].map(header => header.toString().toLowerCase());

        const columnNumbers = {};

        for (const column of headerArray) {
            const columnName = column.toString().toLowerCase();
            const columnNumber = lowercaseHeaders.indexOf(columnName);
            if (columnNumber !== -1) {
            columnNumbers[column] = columnNumber;
            } else {
            Logger.log(`Column "${column}" not found in the headers.`);
            throw new Error("Header Not Found !!");
            }
        }

        return columnNumbers;
    }
   ```

2. **Call the Function**:

   - Pass two arguments to the `getHeaders` function:

     - `sheet`: The Google Sheet you want to extract headers from.
     - `headerArray`: An array specifying the headers or columns you want to retrieve.

   ```javascript
   const spreadSheet = SpreadsheetApp.openById("your_spreadsheet_id");
   const sheet = spreadSheet.getSheetByName("your_sheet_name");

   const headerArray = ["Batch Name", "col6", "col9", "col10"];

   let columnNumbers = null;

   try {
     columnNumbers = getHeaders(sheet, headerArray);
   } catch (e) {
     Logger.log(e.toString());
     // Handle the error, such as appending it to the sheet if headers are not found and use ErrorLog Library
     return;
   }
   ```

3. **Handle Missing Columns**:

   If any of the specified columns are not found in the headers, the function will throw an error with a descriptive message. You can catch this error and handle it as needed, such as logging it and taking appropriate action.

   ```javascript
   try {
     // Attempt to get column numbers
     columnNumbers = getHeaders(sheet, headerArray);
   } catch (e) {
     Logger.log(e.toString());
     // Handle the error, such as appending it to the sheet if headers are not found
     return;
   }
   ```

### Example

Here's an example usage of the function:

```javascript
    const spreadSheet = SpreadsheetApp.openById("sheetId");
    const sheet = spreadSheet.getSheetByName("Ref");

    const headerArray = ["Batch Name", "col6", "col9", "col10"];

    let columnNumbers = null;

    try {
    columnNumbers = getHeaders(sheet, headerArray);
    } catch (e) {
    Logger.log(e.toString())
    // Define Error Log here, and append it to sheet if -> header not found !!
    }

    Logger.log(columnNumbers);
```

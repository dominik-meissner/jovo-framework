# Google Sheets CMS Integration

Learn how to use Google Sheets as CMS for your Alexa Skills and Google Actions.

* [Introduction](#introduction)
* [Configuration](#configuration)
   * [Public Spreadsheets](#public-spreadsheets)
   * [Private Spreadsheets](#private-spreadsheets)
* [Default Sheet Types](#default-sheet-types)
   * [Default](#default)
   * [Responses](#responses)
   * [KeyValue](#keyvalue)
* [Defining your own Sheet Type](#defining-your-own-sheet-type)


## Introduction

With this Jovo CMS integration, you can manage all the content of your Alexa Skills and Google Actions in a Google Spreadsheet. This makes collaboration easier and enables you update and add content faster.

Here is what a sample spreadsheet could look like:
![Google Sheets CMS for Alexa and Google Assistant](../../img/cms-sample-google-sheets.jpg)

> [You can use this Spreadsheet as a starter template](https://docs.google.com/spreadsheets/d/1vgz5oZca1J7a37qV_nkwWK2ryYSCyh8008SsjLf5-Sk/).

## Configuration

To get started, install the following package:

```sh
$ npm install --save jovo-cms-googlesheets
```

Add it to your `app.js` file and register it with the `use` command:

```javascript
const { GoogleSheetsCMS } = require('jovo-cms-googlesheets');

app.use(new GoogleSheetsCMS());
```

Next, add configurations like the `spreadsheetId` to your `config.js` file:

```javascript
// config.js file
cms: {
    GoogleSheetsCMS: {
        spreadsheetId: '<YourSpreadsheetId>',
        access: '<public|private>',
        sheets: [
            {
                name: '<sheetName>',
                type: '<SheetType>',
            },
        ]
    }
},
```
Each sheet can be added as an object that includes both a `name` and a `type`. [Learn more about Sheet Types below](#default-sheet-types).


Additional configuration might differ depending if you want to use a publicly accessible or private spreadsheet:

* [Public Spreadsheets](#public-spreadsheets)
* [Private Spreadsheets](#private-spreadsheets)

### Public Spreadsheets

> [Tutorial: Use Google Sheets as CMS for your Voice App](https://www.jovo.tech/tutorials/google-sheets-cms)

Public spreadsheets allow you to get started quickly whithout having to care about credentials. We recommend setting up a public spreadsheet first and then turning to [private spreadsheets](#private-spreadsheets) later.

For public spreadsheets, you need to add the following to your `config.js` file:

```javascript
// config.js file
cms: {
    GoogleSheetsCMS: {
        spreadsheetId: '<YourSpreadsheetId>',
        access: 'public',
        sheets: [
            {
                name: '<sheetName>',
                type: '<SheetType>',
                position: 1,
            },
        ]
    }
},
```

The additional information you need to add for public spreadsheets is the `position` of the sheet. It is the position of the tab the sheet is located in.


### Private Spreadsheets

> [Tutorial: Use Private Google Spreadsheets as a CMS](https://www.jovo.tech/tutorials/google-spreadsheet-private-cms)

With private spreadsheets, you can control who has access to your content. This comes with the price of a few more extra steps to set it up.

For private spreadsheets, you need to add the following to your `config.js` file:

```javascript
// config.js file
cms: {
    GoogleSheetsCMS: {
        spreadsheetId: '<YourSpreadsheetId>',
        access: 'private',
        credentialsFile: './path/to/credentials.json',
        sheets: [
            {
                name: 'responses',
                type: 'Responses',
            },
        ]
    }
},
```

To make private spreadsheets work, you need to create a service account and security credentials. These can be downloaded as a JSON file and then referenced in the `credentialsFile` element (default is `./credentials.json`).


## Default Sheet Types

Google Sheets offers flexible ways to structure data. This is why the Jovo CMS integration supports several sheet types that are already built in:

* [Default](#default)
* [Responses](#responses)
* [KeyValue](#keyvalue)

### Default

If you don't define a sheet type in the `config.js`, you receive an array of arrays that can be accessed like this:

```javascript
this.$cms.sheetName
```

### Responses

If you define the sheet type as `Responses`, the integration expects a spreadsheet of at least two columns:
* a `key`
* a locale, e.g. `en`, `en-US`, or `de-DE`

For this locale, you can then access the responses like this:

```javascript
// i18n notation
this.t('key')

// Alternative
this.$cms.t('key')
```

You can add as many locales as you want by adding additional columns for each key.


### KeyValue

If you define the sheet type as `KeyValue`, the integration expects a spreadsheet of at least two columns:
* a `key`
* a `value`

For every key, this will return the value as a string:

```javascript
this.$cms.sheetName.key
```



## Defining your own Sheet Type

You can extend the [`Default` sheet](#default) and pass it to the `SpreadsheetCMS` object like this:

```javascript
const { GoogleSheetsCMS } = require('jovo-cms-googlesheets');
const { YourSheetType } = require('./path/to/module');

app.use(
    new GoogleSheetsCMS().use(new YourSheetType())
);
```
You can then reference the sheet type by its name in the `config.js` file:

```javascript
// config.js file
cms: {
    GoogleSheetsCMS: {
        spreadsheetId: '<YourSpreadsheetId>',
        access: '<public|private>',
        sheets: [
            {
                name: '<sheetName>',
                type: 'YourSheetType',
            },
        ]
    }
},
```



<!--[metadata]: {"description": "Learn how to use Google Sheets as CMS for your Alexa Skills and Google Actions.",
"route": "cms/google-sheets" }-->

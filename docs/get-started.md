---
id: get-started
title: Get Started
sidebar_label: Get Started
---

dataPipe is data transformation and analytical library inspired by LINQ (C#) and Pandas - (Python). It provides a facilities for data loading, data transformation and other helpful data manipulation functions. Originally DataPipe project was created to power [JSPython](https://github.com/jspython-dev/jspython) and [Worksheet Systems](https://worksheet.systems) related projects, but it is also can be used as a standalone library for your data-driven JavaScript or JSPython applications on both the client (web browser) and server (NodeJS).

## Get started

A quick way to use it in html

```
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/datapipe-js/dist/data-pipe.min.js"></script>
```

or npm

```
npm install datapipe-js
```

## A quick example

JavaScript / TypeScript

[StackBlitz example](https://stackblitz.com/edit/datapipe-js-examples?file=index.js)

```js
const { dataPipe, avg, first } = require('datapipe-js');
const fetch = require('node-fetch');

async function main() {

    const dataUrl = "https://raw.githubusercontent.com/FalconSoft/sample-data/master/CSV/sample-testing-data-100.csv";
    const csv = await (await fetch(dataUrl)).text();

    return dataPipe()
        .fromCsv(csv)
        .groupBy(r => r.Country)
        .select(g => ({
            country: first(g).Country,
            sales: dataPipe(g).sum(i => i.Sales),
            averageSales: avg(g, i => i.Sales),
            count: g.length
        })
        )
        .where(r => r.sales > 5000)
        .sort("sales DESC")
        .toArray();
}

main()
    .then(console.log)
    .catch(console.error)
```

## Data management functions

All utlity functions can be used as a chaining (pipe) methods as well as a separately. In an example you will notice that to sum up `sales` we created a new dataPipe, but for an `averageSales` we used just a utility method `avg`. 

### Data Loading

Loading and parsing data from a common file formats like: CSV, JSON, TSV either from local variable

 - [**dataPipe**](/docs/datapipe#datapipe) - accepts a JavaScript array
 - [**fromCsv**](/docs/datapipe#fromcsv)(csvContent[, options]) - it loads a string content and process each row with optional but robust configuration options and callbacks e.g. skipRows, skipUntil, takeWhile, rowSelector, rowPredicate etc. This will automatically convert all types to numbers, datetimes or booleans if otherwise is not specified

### Data Transformation

 - [**select**](/docs/datapipe-js-array#select)(elementSelector) synonym **map** - creates a new element for each element in a pipe based on elementSelector callback.
 - [**where**](/docs/datapipe-js-array#where)(predicate) / **filter** - filters elements in a pipe based on predicate
 - [**groupBy**](/docs/datapipe-js-array#groupby)(keySelector) - groups elements in a pipe according to the keySelector callback-function. Returns a pipe with new group objects.
 - [**pivot**](/docs/datapipe-js-array#pivot)(array, rowFields, columnField, dataField, aggFunction?, columnValues?) - Returns a reshaped (pivoted) array based on unique column values.
 - [**transpose**](/docs/datapipe-js-array#transpose)(array) - Transpose rows to columns in an array
 - [**innerJoin**](/docs/datapipe-js-array#innerjoin)(leftArray, rightArray, leftKey, rightKey, resultSelector) - Joins two arrays together by selecting elements that have matching values in both arrays. The array elements that do not have matche in one array will not be shown!
 - [**leftJoin**](/docs/datapipe-js-array#leftjoin)(leftArray, rightArray, leftKey, rightKey, resultSelector) - Joins two arrays together by selrcting all elements from the left array (leftArray), and the matched elements from the right array (rightArray). The result is NULL from the right side, if there is no match.
 - [**fullJoin**](/docs/datapipe-js-array#fulljoin)(leftArray, rightArray, leftKey, rightKey, resultSelector) - Joins two arrays together by selrcting all elements from the left array (leftArray), and the matched elements from the right array (rightArray). The result is NULL from the right side, if there is no match.
 - [**merge**](/docs/datapipe-js-array#merge)(targetArray, sourceArray, targetKey, sourceKey) - merges elements from two arrays. It takes source elements and append or override elements in the target array.Merge or append is based on matching keys provided
 - [**sort**](/docs/datapipe-js-array#sort)([fieldName(s)]) - Sort array of elements according to a field and direction specified. e.g. sort(array, 'name ASC', 'age DESC')


### Aggregation and other numerical functions

 - [**avg**](/docs/datapipe-js-array#avg)([propertySelector, predicate]) synonym **average** - returns an average value for a gived array. With `propertySelector` you can choose the property to calculate average on. And with `predicate` you can filter elements if needed. Both properties are optional.
 - [**max**](/docs/datapipe-js-array#max)([propertySelector, predicate]) synonym **maximum** - returns a maximum value for a gived array. With `propertySelector` you can choose the property to calculate maximum on. And with `predicate` you can filter elements if needed. Both properties are optional.
 - [**min**](/docs/datapipe-js-array#min)([propertySelector, predicate]) synonym **minimum** - returns a minimum value for a gived array. With `propertySelector` you can choose the property to calculate minimum on. And with `predicate` you can filter elements if needed. Both properties are optional.
 - [**count**](/docs/datapipe-js-array#count)([predicate]) - returns the count for an elements in a pipe. With `predicate` function you can specify criteria
 - [**first**](/docs/datapipe-js-array#first)([predicate]) - returns a first element in a pipe. If predicate function provided. Then it will return the first element in a pipe for a given criteria.
 - [**last**](/docs/datapipe-js-array#last)([predicate]) - returns a last element in a pipe. If predicate function provided. Then it will return the first element in a pipe for a given criteria.
 - [**mean**](/docs/datapipe-js-array#mean)(array, [propertySelector]) - returns a mean in array.
 - [**quantile**](/docs/datapipe-js-array#quantile)(array, [propertySelector]) - returns a quantile in array.
 - [**variance**](/docs/datapipe-js-array#variance)(array, [propertySelector]) - returns a sample variance of an array.
 - [**stdev**](/docs/datapipe-js-array#stdev)(array, [propertySelector]) - returns a standard deviation in array.
 - [**median**](/docs/datapipe-js-array#median)(array, [propertySelector]) - returns a median in array.
 
### DataPipe Output

 - [**toArray**](/docs/datapipe#toarray)() - output your pipe result into JavaScript array.
 - [**toObject**](/docs/datapipe#toobject)(nameSelector, valueSelector) - output your pipe result into JavaScript object, based of name and value selectors.
 - [**toCsv**](/docs/datapipe#tocsv)([delimiter]) - output pipe result into string formated as CSV


### Other helpful utilities for working with data in JavaScript or JSPython
 - [**parseDatetimeOrNull**](/docs/datapipe-js-utils#parsedatetimeornull)(dateString[, formats]) - a bit wider date time parser than JS's `parseDate()`. Be aware. It gives UK time format (dd/MM/yyyy) a priority! e.g. '8/2/2019' will be parsed to 8th of February 2019
 - [**dateToString**](/docs/datapipe-js-utils#datetostring)(date) - converts date to string without applying time zone. It returns ISO formated date with time (if time present). Otherwise it will return just a date - yyyy-MM-dd
 - [**parseNumberOrNull**](/docs/datapipe-js-utils#parsenumberornull)(value: string | number): convert to number or returns null
 - [**parseBooleanOrNull**](/docs/datapipe-js-utils#parsebooleanornull)(val: boolean | string): convert to Boolean or returns null. It is treating `['1', 'yes', 'true', 'on']` as true and `['0', 'no', 'false', 'off']` as false 
 - [**deepClone**](/docs/datapipe-js-utils#deepclone) returns a deep copy of your object or array.

## License
A permissive [MIT](https://github.com/FalconSoft/dataPipe/blob/master/LICENSE) (c) - FalconSoft Ltd

 


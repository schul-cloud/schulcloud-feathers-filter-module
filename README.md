# Schul-Cloud Filter Module

an universal configurable filter module that will fire an event 
and gives you a FeathersJS query that you should apply.

[![forthebadge](http://forthebadge.com/images/badges/made-with-vue.svg)](http://forthebadge.com)
[![forthebadge](http://forthebadge.com/images/badges/built-with-love.svg)](http://forthebadge.com)

![filter-module](https://user-images.githubusercontent.com/22987140/36727522-db3ee16e-1bbd-11e8-91d2-426e0bebb306.PNG)

## Usage
simply include the module into your project and you can use it.
Configure it by apply some of the following html-attributes.
Then add an eventListener to watch for new querys.
```html
<html>
  <header><meta charset="utf-8"></header>
  <body>
    <feathers-filter id="filter" add-label="more filter"/>
    <script src="./feathers-filter.js"></script>
    <script>
      document.getElementById("filter").addEventListener('newFilter', (e) => {
        // APPLY QUERE HERE...
        console.log("filter:",e.detail[0]);
      })
    </script>
  </body>
</html>
```

## configuration

### add-label `add-label="..."`

the label of the "add more filter" button.

> `{type: String, default: "Add Filter"}`

### apply-label `apply-label="..."`

the label of the apply button of each filter dialog.

> `{type: String, default: "apply"}`

### cancle-label `cancle-label="..."`

the label of the cancle button of each filter dialog.

> `{type: String, default: "cancle"}`

### handleUrl `handle-url="..."`

should the component update the url, of the window it is mounted, 
for you or do you wan't to handle it yourself?

> `{type: Boolean, default: false}`

### filter `filter="[...]"`
you can use the "filter" property to configure the available filter. 
The property should be a stringified JSON Object. 

> `{type: Array, default: []}`

You can use as many of each type as you want, but at the moment you only have the following filter types. 

Options marked with `WIP` are `Work in Progress` and are currently not working.
#### date
filter for an date range
```javascript
{
  type: "date",                             // required
  title: 'Created at'                       // required
  displayTemplate: 'created from %1 to %2', // required ~ %1=fromDate, %2=toDate
  property: 'createdAt',                    // required
  mode: 'from',                             // required 'from', 'to', 'fromto'
  autoOrder: false,                         // optional, default: true
  hideOnSelect: false,                      // WIP ~ optional, default: true
  minDate: (UNIX TIMESTAMP),                // WIP ~ optional, default: today
  maxDate: (UNIX TIMESTAMP),                // WIP ~ optional, default: today
  fromLabel: "STRING",                      // optional, default: "from"
  toLabel: "STRING",                        // optional, default: "to"
  defaultFromDate: (UNIX TIMESTAMP),        // optional, default: undefined
  defaultToDate: (UNIX TIMESTAMP)           // optional, default: undefined
}
```
if you set minDate or maxDate to `false` the related input is hidden.

#### select value ...
let the user choose an value for a variable
```javascript
{
  type: "select",                // required
  title: 'Class'                 // required
  displayTemplate: 'class: %1',  // required
  property: 'classId',           // required
  multiple: true,                // optional, default: false
  expanded: true                 // optional, default: false
  options: [                     // required, minLength: 1!
    [123, "Class A"],
    [456, "Class B"],
    [789, "Class C"],
  ],
  defaultSelection: [123, 456]  // optional, if multiple disabled the first value is applied
} 
```

#### sort by ...
let the user order the result
```javascript
{
  type: "sort",                   // required
  title: 'Sort'                   // required
  displayTemplate: 'Sort by: %1', // required
  options: [                      // required, minLength: 1!
    ['propertyA', "Sort by A"],
    ['propertyB', "Sort by B"],
    ['propertyC', "Sort by C"],
  ],
  defaultSelection: 'propertyA'  // optional, default: undefined
  defaultOrder: 'DESC'           // optional, default: 'DESC'
}
```

#### boolean
toggle if an boolean value should be true or false
```javascript
{
  type: "boolean",               // required
  title: 'more'                  // required
  options: {                     // required, minLength: 1!
    'propertyA': "Label A",
    'propertyB': "Label B",
    'propertyC': "Label C"
  },
  defaultSelection: {            // optional, default: undefined
    'propertyA': false, 
    'propertyC': true
  },
  applyNegated: {               // optional, default: [false, false]
    'propertyA': [true, true],  // false: not true, true: not false
    'propertyA': [false, true], // false: false, true: not false
    'propertyA': [true, false], // false: not true, true: true
  }
}
```
applyNegated tells the filter how to query for false/true selections and negates the query according to your settings.
e.g. if the user selects true, and you set the property to `[false, true]` the query is looking for `not false` 
resulting in `property[$ne]=false` instead of `property=true`.

#### limit
limit the result to the selected amount of items
```javascript
{
  type: "limit",                 // required
  title: 'Anzahl der Einträge'   // required
  displayTemplate: '%1',         // required
  options: [                     // required, minLength: 1!
    10, 25, 50, 100 
  ],
  defaultSelection: 25           // optional, default: undefined
}
```

## Development Setup

``` bash
# clone repo to your device
> git clone https://github.com/schul-cloud/schulcloud-filter-module.git

# go to directory
> cd schulcloud-filter-module

# install dependencies
> npm install
```

## Build & Development

``` bash
# serve with hot reload at localhost:8080
> npm run dev

# build for production with minification
> npm run build
```

If you don't want the build-files to be located at the root directory 
take a look at the [webpack documentation](https://webpack.js.org/guides/public-path/)

## How to name your branch

1. Take the id of your github issue (e.g. 2 for [this issue](https://github.com/schul-cloud/schulcloud-content-editor/issues/2))
2. add a short description <br>
=> result: e.g. Branch: "2-real-Loginform"

## Commiting

Default branch: master

1. Go into project folder
2. Run the tests (see above)
3. Commit with a meanigful commit message(!) even at 4 a.m. and not stuff like "dfsdfsf"
4. Checkout to master branch
5. Run `git pull`
6. Checkout to the branch you want to upload
7. run `git rebase -p develop` (not `git merge`!) and solve merge conflicts if needed
8. run `git push`

## Testing
``` bash
# check bundlesize
> npm run test

# run build & check bundlesize
> npm run travis
```

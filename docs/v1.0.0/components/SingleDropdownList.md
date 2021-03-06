{"bigh3": true}

## SingleDropdownList

![Image to be displayed](https://i.imgur.com/PGYPXf6.png)

`SingleDropdownList` creates a dropdown list based single select UI component. It is used for filtering results based on the current selection from a list of items.

`Note:` This component is exactly like the [SingleList](/v1.0.0/component/SingleList.html) component except the UI is based on a dropdown, ideal for showing additional UI filters while conserving screen space.

Example uses:
* select a category from a list of categories for filtering e-commerce search results.
* filtering restaurants by a cuisine choice.

### Usage

#### Basic Usage

```js
<SingleDropdownList
  componentId="CitySensor"
  appbaseField="group.group_city.raw"
  title="Cities"
/>
```

#### Usage With All Props

```js
<SingleDropdownList
  componentId="CitySensor"
  appbaseField="group.group_city.raw"
  title="Cities"
  size={100}
  sortBy="count"
  defaultSelected="London"
  showCount={true}
  placeholder="Search City"
  initialLoader="Loading cities list.."
  showFilter={true}
  filterLabel="City"
  URLParams={false}
/>
```

### Props

- **componentId** `String`  
    unique identifier of the component, can be referenced in other components' `react` prop.
- **appbaseField** `String`  
    data field to be connected to the component's UI view.
- **title** `String or HTML` [optional]  
    title of the component to be shown in the UI.
- **size** `Number` [optional]  
    control how many items to display in the List. Defaults to 100.
- **sortBy** `String` [optional]  
    property that decides on how to sort the list items, accepts one of `count`, `asc` or `desc` as valid values. `count` sorts the list based on the count occurences, with highest value at the top. `asc` sorts the list in the ascending order of the list item (Alphabetical). `desc` sorts the list in the descending order of the term. Defaulted to `count`.
- **defaultSelected** `string` [optional]  
    default selected value pre-selects an option from the list.
- **showCount** `Boolean` [optional]  
    show count of number of occurences besides an item. Defaults to `true`.
- **placeholder** `String` [optional]  
    placeholder to be displayed in the dropdown searchbox.
- **initialLoader** `String or HTML` [optional]  
    display text while the data is being fetched, accepts `String` or `HTML` markup.
- **showFilter** `Boolean` [optional]  
    show as filter when a value is selected in a global selected filters view. Defaults to `true`.
- **filterLabel** `String` [optional]  
    An optional label to display for the component in the global selected filters view. This is only applicable if `showFilter` is enabled. Default value used here is `componentId`.
- **URLParams** `Boolean` [optional]  
    enable creating a URL query string parameter based on the selected value of the list. This is useful for sharing URLs with the component state. Defaults to `false`.

### Syntax

<p data-height="500" data-theme-id="light" data-slug-hash="prVZKP" data-default-tab="js" data-user="divyanshu013" data-embed-version="2" data-pen-title="SingleDropdownList docs example" class="codepen">See the Pen <a href="https://codepen.io/divyanshu013/pen/prVZKP/">SingleDropdownList docs example</a> by Divyanshu (<a href="https://codepen.io/divyanshu013">@divyanshu013</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### Styles

All reactivebase components are `rbc` namespaced.

![Annotated image](https://i.imgur.com/8FY18nw.png)

### Extending

`SingleDropdownList` component can be extended to
1. customize the look and feel with `componentStyle`,
2. update the underlying DB query with `customQuery`,
3. connect with external interfaces using `beforeValueChange` and `onValueChange`.

```
<SingleDropdownList
  ...
  componentStyle={{"paddingBottom": "10px"}}
  customQuery={
    function(value) {
      return {
        query: {
          match: {
            data_field: "this is a test"
          }
        }
      }
    }
  }
  beforeValueChange={
    function(value) {
      // called before the value is set
      // returns a promise
      return new Promise((resolve, reject) => {
        // update state or component props
        resolve()
        // or reject()
      })
    }
  }
  onValueChange={
    function(value) {
      console.log("current value: ", value)
      // set the state
      // use the value with other js code
    }
  }
/>
```

- **componentStyle** `Object`
    CSS styles to be applied to the **SingleDropdownList** component.
- **customQuery** `Function`
    takes **value** as a parameter and **returns** the data query to be applied to the component, as defined in Elasticsearch v2.4 Query DSL.
    `Note:` customQuery is called on value changes in the **SingleDropdownList** component as long as the component is a part of `react` dependency of at least one other component.
- **beforeValueChange** `Function`  
    is a callback function which accepts component's future **value** as a parameter and **returns** a promise. It is called everytime before a component's value changes. The promise, if and when resolved, triggers the execution of the component's query and if rejected, kills the query execution. This method can act as a gatekeeper for query execution, since it only executes the query after the provided promise has been resolved.
- **onValueChange** `Function`  
    is a callback function which accepts component's current **value** as a parameter. It is called everytime the component's value changes. This prop is handy in cases where you want to generate a side-effect on value selection. For example: You want to show a pop-up modal with the valid discount coupon code when a list item is selected in a "Discounted Price" SingleDropdownList.

### Examples

<p data-height="500" data-theme-id="light" data-slug-hash="prVZKP" data-default-tab="result" data-user="divyanshu013" data-embed-version="2" data-pen-title="SingleDropdownList docs example" class="codepen">See the Pen <a href="https://codepen.io/divyanshu013/pen/prVZKP/">SingleDropdownList docs example</a> by Divyanshu (<a href="https://codepen.io/divyanshu013">@divyanshu013</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

1. [List with all the default props](../playground/?knob-title=SingleDropdownList&knob-defaultSelected=London&knob-selectAllLabel=All%20Cities&knob-queryFormat=or&knob-sortBy=count&knob-showCheckbox=true&knob-size=100&knob-showCount=true&knob-placeholder=Select%20a%20City&knob-showSearch=true&selectedKind=map%2FSingleDropdownList&selectedStory=Basic&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

2. [List with a 'Select All' option](../playground/?knob-title=SingleDropdownList&knob-defaultSelected=London&knob-selectAllLabel=All%20Cities&knob-queryFormat=or&knob-sortBy=count&knob-showCheckbox=true&knob-size=100&knob-showCount=true&knob-placeholder=Select%20a%20City&knob-showSearch=true&selectedKind=map%2FSingleDropdownList&selectedStory=With%20Select%20All&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

3. [List with pre-selected options](../playground/?knob-title=SingleDropdownList&knob-defaultSelected=London&knob-selectAllLabel=All%20Cities&knob-queryFormat=or&knob-sortBy=count&knob-showCheckbox=true&knob-size=100&knob-showCount=true&knob-placeholder=Select%20a%20City&knob-showSearch=true&selectedKind=map%2FSingleDropdownList&selectedStory=With%20Default%20Selected&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

4. [Playground (with all knob actions)](../playground/?knob-title=SingleDropdownList&knob-defaultSelected=London&knob-selectAllLabel=All%20Cities&knob-queryFormat=or&knob-sortBy=count&knob-showCheckbox=true&knob-size=100&knob-showCount=true&knob-placeholder=Select%20a%20City&knob-showSearch=true&selectedKind=map%2FSingleDropdownList&selectedStory=Playground&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

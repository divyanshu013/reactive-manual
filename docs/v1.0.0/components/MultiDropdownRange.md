{"bigh3": true}

## MultiDropdownRange

![Image to be displayed](https://i.imgur.com/Xwo2Aog.png)

`MultiDropdownRange` creates a dropdown based multiple numeric range UI component that is connected to a database field.

`Note:` It is exactly like the [MultiRange](/v1.0.0/components/MultiRange.html) component but displayed in a dropdown, ideal for showing additional UI filters while conserving screen space

Example uses:
* filtering search results by prices in an e-commerce or food delivery experience.
* browsing movies by a ratings filter.

### Usage

#### Basic Usage

```js
<MultiDropdownRange
  componentId="PriceSensor"
  appbaseField="price"
  title="Prices"
  data={
    [{"start": 0, "end": 10, "label": "Cheap"},
     {"start": 11, "end": 20, "label": "Moderate"},
     {"start": 21, "end": 50, "label": "Pricey"},
     {"start": 51, "end": 1000, "label": "First Date"}]
  }
/>
```

#### Usage With All Props

```js
<MultiDropdownRange
  componentId="PriceSensor"
  appbaseField="price"
  title="Prices"
  defaultSelected={["Cheap", "Moderate"]}
  data={
    [{"start": 0, "end": 10, "label": "Cheap"},
     {"start": 11, "end": 20, "label": "Moderate"},
     {"start": 21, "end": 50, "label": "Pricey"},
     {"start": 51, "end": 1000, "label": "First Date"}]
  }
  placeholder="Select price ranges"
  showFilter={true}
  filterLabel="Price"
  URLParams={false}
/>
```

### Props

- **componentId** `String`  
    unique identifier of the component, can be referenced in other components' `react` prop.
- **appbaseField** `String`  
    data field to be connected to the component's UI view. The range items are filtered by a database query on this field.
- **title** `String or HTML` [optional]  
    title of the component to be shown in the UI.
- **defaultSelected** `Array` [optional]  
    pre-select one or more labels from the `data` array.
- **data** `Object Array`  
    collection of UI `labels` with associated `start` and `end` range values.
- **placeholder** `String` [optional]  
    set the placeholder to show for the dropdown UI, useful when no option is `defaultSelected`. The default placeholder value is set to "Select...".
- **showFilter** `Boolean` [optional]  
    show as filter when a value is selected in a global selected filters view. Defaults to `true`.
- **filterLabel** `String` [optional]  
    An optional label to display for the component in the global selected filters view. This is only applicable if `showFilter` is enabled. Default value used here is `componentId`.
- **URLParams** `Boolean` [optional]  
    enable creating a URL query string parameter based on the selected value of the list. This is useful for sharing URLs with the component state. Defaults to `false`.

### Syntax

<p data-height="500" data-theme-id="light" data-slug-hash="brKeGP" data-default-tab="js" data-user="divyanshu013" data-embed-version="2" data-pen-title="MultiDropdownRange docs example" class="codepen">See the Pen <a href="https://codepen.io/divyanshu013/pen/brKeGP/">MultiDropdownRange docs example</a> by Divyanshu (<a href="https://codepen.io/divyanshu013">@divyanshu013</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### Styles

All reactivebase components are `rbc` namespaced.

![Annotated image](https://i.imgur.com/qO1I0Th.png)

### Extending

`MultiDropdownRange` component can be extended to
1. customize the look and feel with `componentStyle`,
2. update the underlying DB query with `customQuery`,
3. connect with external interfaces using `beforeValueChange` and `onValueChange`.

```
<MultiDropdownRange
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
    CSS styles to be applied to the **MultiDropdownRange** component.
- **customQuery** `Function`  
    takes **value** as a parameter and **returns** the data query to be applied to the component, as defined in Elasticsearch v2.4 Query DSL.
    `Note:` customQuery is called on value changes in the **MultiDropdownRange** component as long as the component is a part of `react` dependency of at least one other component.
- **beforeValueChange** `Function`  
    is a callback function which accepts component's future **value** as a parameter and **returns** a promise. It is called everytime before a component's value changes. The promise, if and when resolved, triggers the execution of the component's query and if rejected, kills the query execution. This method can act as a gatekeeper for query execution, since it only executes the query after the provided promise has been resolved.
- **onValueChange** `Function`  
    is a callback function which accepts component's current **value** as a parameter. It is called everytime the component's value changes. This prop is handy in cases where you want to generate a side-effect on value selection. For example: You want to show a pop-up modal with the valid discount coupon code(s) when range item(s) is/are selected in a "Prices" MultiDropdownRange.

### Examples

<p data-height="500" data-theme-id="light" data-slug-hash="brKeGP" data-default-tab="result" data-user="divyanshu013" data-embed-version="2" data-pen-title="MultiDropdownRange docs example" class="codepen">See the Pen <a href="https://codepen.io/divyanshu013/pen/brKeGP/">MultiDropdownRange docs example</a> by Divyanshu (<a href="https://codepen.io/divyanshu013">@divyanshu013</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

1. [Range with default props](../playground/?knob-title=MultiDropdownList&knob-URLParams%20%28not%20visible%20in%20storybook%29=false&knob-filterLabel=City%20filter&knob-defaultSelected%5B0%5D=London&knob-defaultSelected%5B1%5D=Melbourne&knob-selectAllLabel=All%20Cities&knob-showFilter=true&knob-sortBy=count&knob-size=100&knob-showCount=true&knob-placeholder=Select%20Cities&selectedKind=map%2FMultiDropdownRange&selectedStory=Basic&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

2. [Range with pre-selected values](../playground/?knob-title=MultiDropdownList&knob-URLParams%20%28not%20visible%20in%20storybook%29=false&knob-filterLabel=City%20filter&knob-defaultSelected%5B0%5D=London&knob-defaultSelected%5B1%5D=Melbourne&knob-selectAllLabel=All%20Cities&knob-showFilter=true&knob-sortBy=count&knob-size=100&knob-showCount=true&knob-placeholder=Select%20Cities&selectedKind=map%2FMultiDropdownRange&selectedStory=With%20Default%20Selected&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

3. [Playground (with all knob actions)](../playground/?knob-title=MultiDropdownRange%3A%20Earthquake%20Magnitude&knob-URLParams%20%28not%20visible%20in%20storybook%29=false&knob-filterLabel=City%20filter&knob-defaultSelected%5B0%5D=London&knob-defaultSelected%5B1%5D=Melbourne&knob-selectAllLabel=All%20Cities&knob-showFilter=true&knob-sortBy=count&knob-size=100&knob-showCount=true&knob-placeholder=Search%20places&selectedKind=map%2FMultiDropdownRange&selectedStory=Playground&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

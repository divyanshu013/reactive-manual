{"bigh3": true}

## MultiDataList

![Image to be displayed](https://i.imgur.com/cEAUorS.png)

`MultiDataList` creates a multiple checkbox list UI component that is connected to a database field. It is used for filtering results based on the current selection(s) from a list of data items.

`Note:` This component behaves similar to the [MultiList](/v1.0.0/components/MultiList.html) component except the list items are user defined with the `data` prop, ideal for showing curated items in a list layout.

Example uses:
* select one or multiple items from a list of categories for filtering e-commerce search results.
* filtering restaurants by one or more cuisine choices.

### Usage

#### Basic Usage

```js
<MultiDataList
  componentId="MeetupTops"
  appbaseField="group.group_topics.topic_name_raw.raw"
  title="Meetups"
  data={
    [{
      label: "Social",
      value: "Social"
    }, {
      label: "Travel",
      value: "Travel"
    }, {
      label: "Outdoors",
      value: "Outdoors"
    }]
  }
/>
```

#### Usage With All Props

```js
<MultiDataList
  componentId="MeetupTops"
  appbaseField="group.group_topics.topic_name_raw.raw"
  title="Meetups"
  data={
    [{
      label: "Social",
      value: "Social"
    }, {
      label: "Travel",
      value: "Travel"
    }, {
      label: "Outdoors",
      value: "Outdoors"
    }]
  }
  showSearch={true}
  showCheckbox={true}
  placeholder="Filter meetups"
  defaultSelected={["Social"]}
  selectAllLabel="All meetups"
  showFilter={true}
  filterLabel="Price"
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
- **data** `Object Array`  
    collection of UI `labels` with associated `value` to be matched against the database field.
- **showSearch** `Boolean` [optional]  
    whether to display a searchbox to filter the data list. Defaults to `false`.
- **showCheckbox** `Boolean` [optional]  
    whether to display a checkbox button beside the list item. Defaults to `true`.
- **placeholder** `String` [optional]  
    placeholder to be displayed in the searchbox. Defaults to "Search".
- **defaultSelected** `String Array` [optional]  
    default selected value(s) pre-selects option(s) from the list.
- **selectAllLabel** `String` [optional]  
    if provided displays an additional option to selct all list values.
- **showFilter** `Boolean` [optional]  
    show as filter when a value is selected in a global selected filters view. Defaults to `true`.
- **filterLabel** `String` [optional]  
    An optional label to display for the component in the global selected filters view. This is only applicable if `showFilter` is enabled. Default value used here is `componentId`.
- **URLParams** `Boolean` [optional]  
    enable creating a URL query string parameter based on the selected value of the list. This is useful for sharing URLs with the component state. Defaults to `false`.

### Syntax

<p data-height="500" data-theme-id="light" data-slug-hash="WEKmEq" data-default-tab="js" data-user="divyanshu013" data-embed-version="2" data-pen-title="MultiDataList docs example" class="codepen">See the Pen <a href="https://codepen.io/divyanshu013/pen/WEKmEq/">MultiDataList docs example</a> by Divyanshu (<a href="https://codepen.io/divyanshu013">@divyanshu013</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### Styles

All reactivebase components are `rbc` namespaced.

![Annotated image](https://i.imgur.com/IYGEhao.png)

### Extending

`MultiDataList` component can be extended to
1. customize the look and feel with `componentStyle`,
2. update the underlying DB query with `customQuery`,
3. connect with external interfaces using `beforeValueChange` and `onValueChange`.

```
<MultiDataList
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
    CSS styles to be applied to the **MultiDataList** component.
- **customQuery** `Function`
    takes **value** as a parameter and **returns** the data query to be applied to the component, as defined in Elasticsearch v2.4 Query DSL.
    `Note:` customQuery is called on value changes in the **MultiDataList** component as long as the component is a part of `react` dependency of at least one other component.
- **beforeValueChange** `Function`  
    is a callback function which accepts component's future **value** as a parameter and **returns** a promise. It is called everytime before a component's value changes. The promise, if and when resolved, triggers the execution of the component's query and if rejected, kills the query execution. This method can act as a gatekeeper for query execution, since it only executes the query after the provided promise has been resolved.
- **onValueChange** `Function`  
    is a callback function which accepts component's current **value** as a parameter. It is called everytime the component's value changes. This prop is handy in cases where you want to generate a side-effect on value selection. For example: You want to show a pop-up modal with the valid discount coupon code when list item(s) is/are selected in a "Discounted Price" MultiDataList.

### Examples

<p data-height="500" data-theme-id="light" data-slug-hash="WEKmEq" data-default-tab="result" data-user="divyanshu013" data-embed-version="2" data-pen-title="MultiDataList docs example" class="codepen">See the Pen <a href="https://codepen.io/divyanshu013/pen/WEKmEq/">MultiDataList docs example</a> by Divyanshu (<a href="https://codepen.io/divyanshu013">@divyanshu013</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

1. [List with all the default props](../playground/?knob-title=Topics&knob-URLParams%20%28not%20visible%20in%20storybook%29=false&knob-filterLabel=Custom%20Filter%20Name&knob-defaultSelected=Social&knob-selectAllLabel=Select%20All&knob-showRadio=true&knob-queryFormat=epoch_millis&knob-numberOfMonths=2&knob-componentStyle=%7B"paddingBottom"%3A"10px"%7D&knob-URLParams%20%28not%20visible%20on%20storybook%29=false&knob-showFilter=true&knob-sortBy=count&knob-dataLabel=★%20%20A%20customizable%20UI%20widget%20★&knob-allowAllDates=true&knob-size=100&knob-extra=%7B"withFullScreenPortal"%3Atrue%2C"showClearDate"%3Atrue%7D&knob-visible=true&knob-showCount=true&knob-placeholder=Search%20topics&knob-showSearch=true&selectedKind=search%2FMultiDataList&selectedStory=Basic&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

2. [List with a 'Select All' option](../playground/?knob-title=Topics&knob-URLParams%20%28not%20visible%20in%20storybook%29=false&knob-filterLabel=Custom%20Filter%20Name&knob-defaultSelected=Social&knob-selectAllLabel=Select%20All&knob-showRadio=true&knob-queryFormat=epoch_millis&knob-numberOfMonths=2&knob-componentStyle=%7B"paddingBottom"%3A"10px"%7D&knob-URLParams%20%28not%20visible%20on%20storybook%29=false&knob-showFilter=true&knob-sortBy=count&knob-dataLabel=★%20%20A%20customizable%20UI%20widget%20★&knob-allowAllDates=true&knob-size=100&knob-extra=%7B"withFullScreenPortal"%3Atrue%2C"showClearDate"%3Atrue%7D&knob-visible=true&knob-showCount=true&knob-placeholder=Search%20topics&knob-showSearch=true&selectedKind=search%2FMultiDataList&selectedStory=With%20selectAllLabel&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

3. [List with pre-selected options](../playground/?knob-title=Topics&knob-URLParams%20%28not%20visible%20in%20storybook%29=false&knob-filterLabel=Custom%20Filter%20Name&knob-defaultSelected%5B0%5D=Social&knob-defaultSelected%5B1%5D=Travel&knob-selectAllLabel=Select%20All&knob-showRadio=true&knob-queryFormat=epoch_millis&knob-numberOfMonths=2&knob-componentStyle=%7B"paddingBottom"%3A"10px"%7D&knob-URLParams%20%28not%20visible%20on%20storybook%29=false&knob-showFilter=true&knob-sortBy=count&knob-dataLabel=★%20%20A%20customizable%20UI%20widget%20★&knob-allowAllDates=true&knob-size=100&knob-extra=%7B"withFullScreenPortal"%3Atrue%2C"showClearDate"%3Atrue%7D&knob-visible=true&knob-showCount=true&knob-placeholder=Search%20topics&knob-showSearch=true&selectedKind=search%2FMultiDataList&selectedStory=With%20defaultSelected&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

4. [Playground (with all knob actions)](../playground/?knob-title=Topics&knob-URLParams%20%28not%20visible%20in%20storybook%29=false&knob-filterLabel=Custom%20Filter%20Name&knob-defaultSelected%5B0%5D=Social&knob-defaultSelected%5B1%5D=Travel&knob-selectAllLabel=Select%20All&knob-showRadio=true&knob-queryFormat=or&knob-numberOfMonths=2&knob-componentStyle=%7B"paddingBottom"%3A"10px"%7D&knob-URLParams%20%28not%20visible%20on%20storybook%29=false&knob-showFilter=true&knob-sortBy=count&knob-dataLabel=★%20%20A%20customizable%20UI%20widget%20★&knob-allowAllDates=true&knob-showCheckbox=true&knob-size=100&knob-extra=%7B"withFullScreenPortal"%3Atrue%2C"showClearDate"%3Atrue%7D&knob-visible=true&knob-showCount=true&knob-placeholder=Search%20topics&knob-showSearch=true&selectedKind=search%2FMultiDataList&selectedStory=Playground&full=0&down=1&left=1&panelRight=0&downPanel=storybooks%2Fstorybook-addon-knobs)

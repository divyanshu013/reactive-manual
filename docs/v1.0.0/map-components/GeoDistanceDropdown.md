{"bigh3": true}

## GeoDistanceDropdown

![Image to be displayed](https://i.imgur.com/SidQoYY.png)

`GeoDistanceDropdown` creates a location search based dropdown UI component that is connected to a database field. It is used for distance based filtering.

Example uses:

* finding restaurants in walking distance from your location.
* discovering things to do near a landmark.

### Usage

#### Basic Usage

```js
<GeoDistanceDropdown
  componentId="LocationUI"
  appbaseField="location"
  title="Location Dropdown Selector"
  data={
    [
      { "start": 0, "end": 20, "label": "< 20 miles" },
      { "start": 0, "end": 50, "label": "< 50 miles" },
      { "start": 0, "end": 100, "label": "< 100 miles" },
    ]
  }
/>
```

#### Usage With All Props

```js
<GeoDistanceDropdown
  componentId="locationUI"
  appbaseField="location"
  title="Location Dropdown Selector"
  data={
    [
      { "start": 0, "end": 20, "label": "< 20 miles" },
      { "start": 0, "end": 50, "label": "< 50 miles" },
      { "start": 0, "end": 100, "label": "< 100 miles" },
    ]
  }
  defaultSelected={{
    location: "SOMA, San Francisco"
    label: "< 20 miles"
  }}
  placeholder="Select a distance range.."
  unit="mi"
  autoLocation={true}
  showFilter={true}
  filterLabel="Location"
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
    collection of UI `labels` with associated `start` and `end` range values.
- **defaultSelected** `Object` [optional]  
    pre-select values of the search query with `label` and `location` keys.
- **placeholder** `String` [optional]  
    set the placeholder to show in the location search box, useful when no option is `defaultSelected`.
- **unit** `String` [optional]  
    unit for distance measurement, uses `mi` (for miles) by default. Distance units can be specified from the following:  
    ![screenshot](https://i.imgur.com/STbeagk.png)
- **autoLocation** `Boolean` [optional]  
    when enabled, preset the user's current location in the location search box. Defaults to `true`.
- **showFilter** `Boolean` [optional]  
    show as filter when a value is selected in a global selected filters view. Defaults to `true`.
- **filterLabel** `String` [optional]  
    An optional label to display for the component in the global selected filters view. This is only applicable if `showFilter` is enabled. Default value used here is `componentId`.
- **URLParams** `Boolean` [optional]  
    enable creating a URL query string parameter based on the selected value of the list. This is useful for sharing URLs with the component state. Defaults to `false`.

### Syntax

TBD

### Styles

All reactivebase and reactivemaps components are `rbc` namespaced.

![Annotated image](https://i.imgur.com/St1dgTz.png)


### Extending

`GeoDistanceDropdown` component can be extended to
1. customize the look and feel with `componentStyle`,
2. update the underlying DB query with `customQuery`,
3. connect with external interfaces using `beforeValueChange` and `onValueChange`.

```
<GeoDistanceDropdown
  ...
  componentStyle={{"paddingBottom": "10px"}}
  customQuery={
    function(value) {
      return {
        query: {
          // query in the format of Elasticsearch Query DSL
          geo_distance: {
            distance: (value.end - value.start)
                      + value.unit,
            location_appbaseField: {
              lat: value.location.split(",")[0]
              lon: value.location.split(",")[1]
            }
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
    CSS styles to be applied to the **GeoDistanceDropdown** component.
- **customQuery** `Function`  
    takes **value** as a parameter and **returns** the data query to be applied to the component, as defined in Elasticsearch v2.4 Query DSL.
    `Note:` customQuery is called on value changes in the **GeoDistanceDropdown** component as long as the component is a part of `react` dependency of at least one other component.
- **beforeValueChange** `Function`  
    is a callback function which accepts component's future **value** as a parameter and **returns** a promise. It is called every time before a component's value changes. The promise, if and when resolved, triggers the execution of the component's query and if rejected, kills the query execution. This method can act as a gatekeeper for query execution, since it only executes the query after the provided promise has been resolved.
- **onValueChange** `Function`  
    is a callback function which accepts component's current **value** as a parameter. It is called every time the component's value changes. This prop is handy in cases where you want to generate a side-effect on value selection. For example:  You want to show a pop-up modal with the valid discount coupon code when a user searches within a specific location area.


### Examples

1. GeoDistance dropdown with all the default props

2. GeoDistance dropdown with a title

3. GeoDistance dropdown with a defaultSelected value

4. Playground (with all knob actions)

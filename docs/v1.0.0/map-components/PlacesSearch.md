{"bigh3": true}

## PlacesSearch

![Image to be displayed](https://i.imgur.com/XnuaS4T.png)

`PlacesSearch` creates a location search UI component that is connected to a database field, which queries the distance around this location.

Example uses:

* search for landmarks near you.
* search for cities, states, countries.

### Usage

#### Basic Usage

```js
<PlacesSearch
  componentId="PlacesUI"
  appbaseField="venue_names"
  title="Venue Search"
/>
```

#### Usage With All Props

```js
<PlacesSearch
  componentId="PlacesUI"
  appbaseField="venue_names"
  title="Venue Search"
  placeholder="Enter a venue name.."
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
    DB data field to be mapped with the component's UI view, used when a database query is made on this field.
- **title** `String or HTML` [optional]  
    title of the component to be shown in the UI.
- **placeholder** `String` [optional]  
    set the placeholder text to be shown in the searchbox field. Defaults to "Search..".
- **unit** `String` [optional]  
    unit for distance measurement, uses `mi` (for miles) by default. Distance units can be specified from the following:  
    ![screenshot](https://i.imgur.com/STbeagk.png)
- **autoLocation** `Boolean` [optional]  
    defaults to `true`, fetches the user's current location and prefills it in the searchbox.
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

![Annotated image](https://i.imgur.com/lNFcxSA.png)

### Extending

`PlacesSearch` component can be extended to
1. customize the look and feel with `componentStyle`,
2. update the underlying DB query with `customQuery`,
3. connect with external interfaces using `beforeValueChange` and `onValueChange`.

```
<PlacesSearch
  ...
  componentStyle={{"paddingBottom": "10px"}}
  customQuery={
    function(value) {
      return {
        query: {
          // query in the format of Elasticsearch Query DSL
          geo_distance: {
            distance: "20mi",
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
    CSS styles to be applied to the **PlacesSearch** component.
- **customQuery** `Function`  
    takes **value** as a parameter and **returns** the data query to be applied to the component, as defined in Elasticsearch v2.4 Query DSL.
    `Note:` customQuery is called on value changes in the **PlacesSearch** component as long as the component is a part of `react` dependency of at least one other component.
- **beforeValueChange** `Function`  
    is a callback function which accepts component's future **value** as a parameter and **returns** a promise. It is called every time before a component's value changes. The promise, if and when resolved, triggers the execution of the component's query and if rejected, kills the query execution. This method can act as a gatekeeper for query execution, since it only executes the query after the provided promise has been resolved.
- **onValueChange** `Function`  
    is a callback function which accepts component's current **value** as a parameter. It is called every time the component's value changes. This prop is handy in cases where you want to generate a side-effect on value selection. For example:  You want to show a pop-up modal with the valid discount coupon code when a user searches within a specific location area.


### Examples

1. PlacesSearch component with all the default props

2. Using PlacesSearch for navigating directions from Point A to Point B

3. Playground (with all knob actions)

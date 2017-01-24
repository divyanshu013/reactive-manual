{"bigh3": true}

{% raw %}

## PaginatedResultList

![Image to be displayed](https://i.imgur.com/s2VIShU.png)

A `PaginatedResultList` component creates a result list widget. Unlike other sensors whose states can be filtered by the end user, `PaginatedResultList` behaves as an actuator that displays the results of other sensor components in a list.

```js
<PaginatedResultList
  sensorId="SearchResult"
  appbaseField={this.props.mapping.searchKey}
  title="Result List"
  paginationAt="both"
  sortBy="desc"
  from={0}
  size={10}
  containerStyle={{height:'500px', overflow:'auto'}}
  onData={this.onData}
  depends={{
    "CitySensor": {
      "operation": "must"
    },
    "SearchSensor": {
      "operation": "must"
    }
  }}
/>
```

### Props

- **sensorId** : `String`: Unique id of the sensor.  
- **appbaseField** : `String`: Data field associated with the `ResultList` sensor, useful for providing a sort context.
- **title**: `String`: Set the title of the component, to be shown in the UI.
- **paginationAt**: `String`: Determines the position where to show the pagination. Accepts one of `top`, `bottom` or `both` as valid values.
-  **sortBy**: `String`: Allows sorting of results by `default`, `asc` or `desc` order. The list is already sorted by relevancy of matches to the applied sensors. `asc` sorts the list in the ascending order of the associated appbaseField's value (Alphabetical). `desc` sorts the list based on the appbaseField value in the descending order.
- **from**: `Number`: is the starting point from where to fetch the results. Useful in a pagination context. Defaults to 0.
- **size**: `Number`: is the number of results to be shown per page. Defaults to 20.
- **containerStyle**: `Object`: CSS Styles to be applied to the **PaginatedResultList** component.
- **onData**: `Function`: A callback function where user can define how to render the view based on the data changes.
- **depends**: `Object`: An object defining the sensor components that trigger the `PagintatedResultList` query. [read more](https://appbaseio.github.io/reactive-maps-docs/v1/getting-started/Dependency.html).


### Extending PaginatedResultList

`onData` prop registers a function callback which is triggered every time there is a change in the data results so that the user can render the `PaginatedResultList` view.

```js
// Register a callback function with the `onData` prop.
<PaginatedResultList ... onData={this.onData} ... />

// Callback function returns an Arry of HTML elements to be rendered as PaginatedResultList items.
this.onData(res, [err]) {
  console.log(res.mode, res.newData, res.currentData, res.appliedQuery);
  if (res.mode === "historic") {
    console.log("time taken for response is: "+took+"ms");
    return res.currentData + res.newData; // infinite scroll functionality
  } else {
    console.log("New streaming update: ", res.newData);
    res.currentData.unshift(res.newData);
    // add markup per element.
    return res.currentData;               // show streaming update functionality
  }
}
```

The callback function returns an Array of HTML elements (think list items) which are then rendered to the view as PaginatedResultList items.  

**Usage**:  

- **res**: `Object` Response object containing:
  - **mode**: `String`: "historic" when results are returned from the existing dataset and "streaming" when new results are matched.
  - **newData**: `Object`: An object array when returning historic data results or a single object for streaming mode updates.
  - **currentData**: `Array Object`: An array of the result objects being shown in the current component view.
  - **appliedQuery**: `Object`: Raw query object that triggered the function callback, useful for debugging.
  - **took**: `Number`: (Optional) Time taken in milliseconds, only passed when the mode is "historic".
- **err**: `Object` Error object.

### CSS Styles



### Examples

1. PaginatedResultList with all the default props and top / down pagition with a single sensor filter.

2. Playground (with all knob actions).

{% endraw %}
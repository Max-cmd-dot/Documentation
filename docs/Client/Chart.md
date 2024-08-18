# ChartComponent Documentation

## Overview

`ChartComponent` is a versatile React component designed to render different types of charts using `react-chartjs-2`. It integrates with `Chart.js` and supports features like customizable chart types, API data fetching, loading states, and more. This component is highly customizable and can be used in various scenarios, such as displaying line charts, scatter plots, or any other supported chart types.

## Key Components and Features

### 1. Chart Rendering

The `ChartComponent` leverages `react-chartjs-2` to render charts. It primarily uses the `Line` and `Scatter` chart components from the library, but it's flexible enough to support other chart types with minimal adjustments.

- **Line Chart**: Displays data points connected by straight lines. Ideal for time-series data or any data requiring a continuous line representation.
- **Scatter Plot**: Displays data points as individual dots on the chart, useful for showing correlations between two variables.

### 2. State Management

The component utilizes React's `useState` and `useEffect` hooks for managing local state and side effects. Global state management is handled through Redux with `useSelector` and `useDispatch`.

- **Local State**: Handles internal component states such as `loading`, `data`, and `chart options`.
- **Global State**: Manages state that needs to be shared across multiple components or pages, such as user information or selected filters.

### 3. API Interaction

Data is fetched from an external API using `axios`. The base URL of the API is configured through environment variables, ensuring flexibility across different environments (development, staging, production).

- **Dynamic Data Fetching**: The component fetches data based on `groupId` and `userId` stored in cookies, making it contextually aware of the user's session.
- **Polling**: Data fetching occurs at regular intervals, ensuring that the chart remains up-to-date with the latest information.

### 4. Loading State

A loading spinner (`ClipLoader` from `react-spinners`) is displayed while data is being fetched, providing a visual cue to users that data is being loaded.

- **Loading Indicator**: The component automatically manages its loading state, showing a spinner when fetching data and hiding it when the data is ready.

### 5. Error Handling

The component includes basic error handling that logs errors to the console if the data fetching process fails.

## Usage Examples

### Example 1: Basic Line Chart

In this example, the `ChartComponent` is used to render a basic line chart. The `chartName` prop is set to `"MyLineChart"`.

```
import React from "react";
import ChartComponent from "./chart";

const App = () => {
  return (
    <div>
      <h1>My Line Chart</h1>
      <ChartComponent chartName="MyLineChart" />
    </div>
  );
};

export default App;
```

### Example 2: Displaying a Scatter Plot

To display a scatter plot, simply set the `chartName` prop to `"MyScatterPlot"`. This tells the component to render a scatter plot instead of a line chart.

```
import React from "react";
import ChartComponent from "./chart";

const App = () => {
  return (
    <div>
      <h1>Scatter Plot</h1>
      <ChartComponent chartName="MyScatterPlot" />
    </div>
  );
};

export default App;
```

### Example 3: Handling Loading State with a Spinner

The `ChartComponent` automatically handles its loading state. This example shows how the component displays a loading spinner while data is being fetched.

```
import React from "react";
import ChartComponent from "./chart";

const App = () => {
  return (
    <div>
      <h1>Chart with Loading Indicator</h1>
      <ChartComponent chartName="LoadingChart" />
    </div>
  );
};

export default App;
```

### Example 4: Customizing Chart Options

You can pass additional props to customize chart options, such as the appearance of the chart or the data being displayed.

```
import React from "react";
import ChartComponent from "./chart";

const App = () => {
  const chartOptions = {
    responsive: true,
    scales: {
      x: {
        type: "time",
        time: {
          unit: "month",
        },
      },
    },
  };

  return (
    <div>
      <h1>Custom Chart Options</h1>
      <ChartComponent chartName="CustomChart" options={chartOptions} />
    </div>
  );
};

export default App;
```

### Example 5: Polling Data for Real-Time Updates

The component supports polling, allowing the chart to refresh at regular intervals. This is particularly useful for real-time data visualization.

```
import React from "react";
import ChartComponent from "./chart";

const App = () => {
  return (
    <div>
      <h1>Real-Time Chart</h1>
      <ChartComponent chartName="RealTimeChart" pollingInterval={5000} />
    </div>
  );
};

export default App;
```

## Prop Details

- **`chartName`**: A string that determines the name or type of the chart to display (e.g., `"MyLineChart"`, `"MyScatterPlot"`). This prop is used to switch between different chart types or data sets.
- **`options`**: An object containing custom chart options. This can be used to configure aspects like scales, tooltips, or legends.
- **`pollingInterval`**: A number (in milliseconds) that sets the interval for polling the API for new data. By default, the component does not poll, but you can enable this feature by passing a value (e.g., `5000` for 5 seconds).

## Conclusion

The `ChartComponent` is a powerful and flexible solution for rendering dynamic charts in React applications. Whether you need a simple line chart or a complex, real-time data visualization, this component can be tailored to meet your needs with minimal configuration.

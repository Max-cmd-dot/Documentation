# `History.jsx` Component Documentation

## Overview

The `History.jsx` component is designed for managing and visualizing historical data through dynamically created and customizable charts. It allows users to create new charts, switch between existing charts, and delete charts they no longer need. This component is especially useful in applications requiring interactive data visualization.

## Prerequisites

Before using the `History` component, ensure that the following dependencies are installed:

```
npm install js-cookie react-redux
```

Set the API URL in your `.env` file:

```
REACT_APP_API_URL=https://yourapiurl.com
```

## Basic Usage

To integrate the `History` component into your application, follow these steps:

### 1. Import the Component

First, import the `History` component into your application file where you intend to use it.

```
import React from "react";
import History from "./components/History"; // Adjust the path as necessary
```

### 2. Rendering the Component

Include the `History` component within your JSX to render it on the page.

```
const App = () => {
  return (
    <div>
      <History />
    </div>
  );
};

export default App;
```

### 3. Handling Dynamic Charts

The `History` component automatically manages dynamic charts based on user actions. Here's how to use it:

#### Adding a New Chart

When the "+" button is clicked, a new chart is created. The component automatically determines a unique name for the chart.

```
const handleAddChart = () => {
  // Logic to add a new chart
};
```

#### Switching Between Charts

Clicking on any existing chart button will display the selected chart.

```
const handleChartSwitch = (chartName) => {
  // Logic to switch to the selected chart
};
```

#### Deleting a Chart

The "Delete this chart" button allows users to remove the currently selected chart.

```
const handleDeleteChart = () => {
  // Logic to delete the currently selected chart
};
```

### 4. Customizing Button Labels

You can customize the button labels dynamically based on your application's needs:

```
const customButtons = ["Custom Chart 1", "Custom Chart 2", "+"];
```

### 5. Integration with APIs

The `History` component is set up to interact with APIs for fetching, adding, and deleting charts. This integration ensures that the user's chart configuration is persistent.

#### Example API Call to Fetch All Charts

```
const getAllCharts = async (userId) => {
  const response = await fetch(\`\${apiUrl}/api/historyChart/all/\${userId}\`);
  const data = await response.json();
  return data.charts.map((chart) => chart.chartName);
};
```

## Example Use Case

Suppose you want to allow users to visualize sales data over time. You can use the `History` component to manage different sales charts, like "Sales in Q1," "Sales in Q2," etc. Users can dynamically add new charts to compare different periods.

```
const SalesHistory = () => {
  return (
    <div>
      <h1>Sales Data History</h1>
      <History />
    </div>
  );
};
```

## Conclusion

The `History.jsx` component is a powerful tool for managing and visualizing data through charts. By following the examples and customization options provided, you can integrate this component seamlessly into your application, providing users with a dynamic and interactive experience.

# `Main.jsx` Component Documentation

## Overview

The `Main.jsx` component serves as a central dashboard for displaying sensor data and a calendar of events. It utilizes multiple libraries, such as `react-chartjs-2` for charts, `FullCalendar` for scheduling, and `axios` for API requests. The component fetches data from APIs, processes it, and renders charts and calendar events dynamically.

## Prerequisites

Ensure that the following packages are installed:

```
npm install react-chartjs-2 chart.js axios js-cookie @fullcalendar/react @fullcalendar/daygrid @fullcalendar/timegrid @fullcalendar/interaction chartjs-adapter-moment
```

## Basic Usage

To use the `Main` component, follow these steps:

### 1. Import the Component

First, import the `Main` component into your application.

```
import React from "react";
import Main from "./components/Main"; // Adjust the path as necessary
```

### 2. Integrating the Component

Integrate the `Main` component into your application where you want to display sensor data and a calendar.

```
import React from "react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Main from "./components/Main";

const App = () => {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Main />} />
      </Routes>
    </Router>
  );
};

export default App;
```

### 3. API Integration

The `Main` component fetches data from various API endpoints using `axios`. Ensure that your backend API is set up and the `REACT_APP_API_URL` environment variable is correctly configured.

#### Example: Setting up the Environment Variable

In your `.env` file:

```
REACT_APP_API_URL=http://your-api-url.com
```

### 4. Data Fetching and State Management

The component manages its state with React's `useState` and `useEffect` hooks. It fetches data for sensor readings, notifications, and subscription details.

#### Example: Data Fetching with Axios

```
useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await axios.get(\`\${apiUrl}/api/data/latestdata/all?groupId=\${groupId}\`);
      setList(response.data);
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };

  fetchData();
}, []);
```

### 5. Rendering Sensor Data

Sensor data is visualized using doughnut charts from the `react-chartjs-2` library. Each sensor topic has its own chart configuration, including colors, units, and thresholds.

#### Example: Rendering a Doughnut Chart

```
function renderChart(item, config) {
  const { heading, unit } = config;
  const { data_chart: dataChart, options_chart: optionsChart } = createChart(
    sortedList,
    item.topic,
    config
  );
  return (
    <div className={styles.sensordata} key={item.topic}>
      <div className={styles.elementssensordata}>
        <p className={styles.sensorheading}>{heading}</p>
        <h2 className={styles.sensorvalue}>
          {item.value} {unit}
        </h2>
        <div className={styles.doughnut}>
          <Doughnut data={dataChart} options={optionsChart} />
        </div>
      </div>
    </div>
  );
}
```

### 6. Calendar Integration

The component uses `FullCalendar` to display a calendar with events fetched from the API. The calendar supports various views like day grid and time grid.

#### Example: Rendering the Calendar

```
<FullCalendar
  plugins={[dayGridPlugin, timeGridPlugin, interactionPlugin]}
  events={events}
  initialView="timeGridWeek"
  height={1300}
  slotLabelFormat={{
    hour: "2-digit",
    minute: "2-digit",
    hour12: false,
  }}
/>
```

### 7. Handling Subscriptions

The component checks the user's subscription level (e.g., "medium" or "big") and adjusts the UI accordingly. This is useful for offering premium features to subscribed users.

#### Example: Conditional Rendering Based on Subscription

```
const isMediumOrBig = rightabo === "medium" || rightabo === "big";

{isMediumOrBig ? (
  <div>
    <h1 className={styles.heading}>Advanced Calendar</h1>
    <FullCalendar
      plugins={[dayGridPlugin, timeGridPlugin, interactionPlugin]}
      events={events}
      initialView="timeGridWeek"
      height={500}
    />
  </div>
) : (
  <h1 className={styles.heading}>Basic Calendar</h1>
)}
```

### 8. Styling the Component

The `Main` component uses a CSS module (`styles.module.css`) for styling. You can customize the appearance by editing the CSS classes or by overriding the styles with your own CSS.

## Example Use Case

Suppose you want to create a dashboard that displays real-time sensor data and events for a smart home system. Here's how you might set it up:

```
const Dashboard = () => {
  return (
    <div>
      <h1>Smart Home Dashboard</h1>
      <Main />
    </div>
  );
};

const App = () => {
  return (
    <Router>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/" element={<Navigate to="/dashboard" />} />
      </Routes>
    </Router>
  );
};

export default App;
```

In this example, the `Main` component is rendered within a `Dashboard` component, which could be the central hub for managing a smart home.

## Conclusion

The `Main.jsx` component is a powerful tool for creating dynamic, data-driven dashboards in React. It integrates seamlessly with various libraries and APIs, offering a customizable and interactive user experience.

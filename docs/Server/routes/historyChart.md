# History Charts Routes Documentation

This documentation outlines the routes for managing history charts, including retrieving, updating, and deleting charts for users.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /all/:userId](#get-alluserId)
  - [GET /settings/:userId/:chartName](#get-settingsuserIdchartName)
  - [POST /update/:userId](#post-updateuserId)
  - [PUT /change_datasets/:userId/:chartName/:dataTyp](#put-changedatasetsuserIdchartNamedataTyp)
  - [PUT /change_type/:userId/:chartName](#put-changetypeuserIdchartName)
  - [PUT /change_data_interval/:userId/:chartName](#put-changedataintervaluserIdchartName)
  - [PUT /change_update_interval/:userId/:chartName](#put-changeupdateintervaluserIdchartName)
  - [PUT /change_max_count/:userId/:chartName](#put-changemaxcountuserIdchartName)
  - [DELETE /delete/:userId/:chartName](#delete-deleteuserIdchartName)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are used in this module:

```
const express = require("express");
const router = express.Router();
const { HistoryChart } = require("../models/HistoryChart"); // replace with the actual path to your HistoryChart model
```

## Routes Overview

### GET /all/:userId

Fetches all charts for a specified user.

- **Parameters:**

  - `userId` (path parameter): The ID of the user.

- **Responses:**
  - **200 OK:** Returns the history chart for the user.
  - **404 Not Found:** If no history chart is found for the user.
  - **500 Internal Server Error:** For unexpected errors.

```
router.get("/all/:userId", async (req, res) => {
  try {
    const historyChart = await HistoryChart.findOne({
      userId: req.params.userId,
    });
    if (!historyChart) {
      return res.status(404).json({ error: "No history chart found." });
    }
    res.json(historyChart);
  } catch (err) {
    console.error(`Error getting charts for user: ${req.params.userId}`, err);
    res.status(500).json({ error: "An error occurred while getting charts." });
  }
});
```

### GET /settings/:userId/:chartName

Fetches the settings for a specific chart of a user.

- **Parameters:**

  - `userId` (path parameter): The ID of the user.
  - `chartName` (path parameter): The name of the chart.

- **Responses:**
  - **200 OK:** Returns the chart data for the specified chart.
  - **404 Not Found:** If no history chart or specific chart is found.
  - **500 Internal Server Error:** For unexpected errors.

```
router.get("/settings/:userId/:chartName", async (req, res) => {
  try {
    const historyChart = await HistoryChart.findOne({
      userId: req.params.userId,
    });
    const chart1 = historyChart.charts.find(
      (chart) => chart.chartName === req.params.chartName
    );
    if (!historyChart) {
      return res.status(404).json({ error: "No history chart found." });
    }
    res.json(chart1.chartData);
  } catch (err) {
    console.error(`Error getting charts for user: ${req.params.userId}`, err);
    res.status(500).json({ error: "An error occurred while getting charts." });
  }
});
```

### POST /update/:userId

Adds a new chart or updates an existing chart for a user.

- **Parameters:**

  - `userId` (path parameter): The ID of the user.

- **Request Body:**

  - `chartId`: The ID of the chart.
  - `chartName`: The name of the chart.
  - `chartData`: The data for the chart.

- **Responses:**
  - **200 OK:** Returns the updated history chart.
  - **500 Internal Server Error:** For unexpected errors.

```
router.post("/update/:userId", async (req, res) => {
  const {
    chartId = "defaultId",
    chartName = "Default Name",
    chartData = {},
  } = req.body;

  // Set the update_interval to 15000
  chartData.update_interval = 15000;

  const newChart = {
    chartId,
    chartName,
    chartData,
    createdAt: new Date(),
    updatedAt: new Date(),
  };

  try {
    const historyChart = await HistoryChart.findOneAndUpdate(
      { userId: req.params.userId },
      { $push: { charts: newChart } },
      { new: true, upsert: true }
    );
    res.json(historyChart);
  } catch (err) {
    console.error(`Error adding new chart for user: ${req.params.userId}`, err);
    res
      .status(500)
      .json({ error: "An error occurred while adding a new chart." });
  }
});
```

### PUT /change_datasets/:userId/:chartName/:dataTyp

Updates the dataset checked status for a specific chart.

- **Parameters:**

  - `userId` (path parameter): The ID of the user.
  - `chartName` (path parameter): The name of the chart.
  - `dataTyp` (path parameter): The data type to update.

- **Request Body:**

  - `checked`: Boolean value indicating if the dataset is checked.

- **Responses:**
  - **200 OK:** Returns the updated history chart.
  - **404 Not Found:** If no history chart or specific chart is found.
  - **500 Internal Server Error:** For unexpected errors.

```
router.put("/change_datasets/:userId/:chartName/:dataTyp", async (req, res) => {
  try {
    const { checked } = req.body;
    const { userId, chartName, dataTyp } = req.params;
    const checkedField = `charts.$.chartData.datasets.0.checked_${dataTyp}`;

    const historyChart = await HistoryChart.findOneAndUpdate(
      { userId: userId, "charts.chartName": chartName },
      { $set: { [checkedField]: checked } },
      { new: true }
    );

    if (!historyChart) {
      return res.status(404).json({ error: "No history chart found." });
    }
    res.json(historyChart);
  } catch (err) {
    console.error(`Error updating chart for user: ${userId}`, err);
    res.status(500).json({ error: "An error occurred while updating charts." });
  }
});
```

### PUT /change_type/:userId/:chartName

Updates the type of a specific chart.

- **Parameters:**

  - `userId` (path parameter): The ID of the user.
  - `chartName` (path parameter): The name of the chart.

- **Request Body:**

  - `type`: The new type for the chart.

- **Responses:**
  - **200 OK:** Returns the updated history chart.
  - **404 Not Found:** If no history chart or specific chart is found.
  - **500 Internal Server Error:** For unexpected errors.

```
router.put("/change_type/:userId/:chartName", async (req, res) => {
  try {
    const { type } = req.body;
    const { userId, chartName } = req.params;

    const historyChart = await HistoryChart.findOneAndUpdate(
      { userId: userId, "charts.chartName": chartName },
      { $set: { "charts.$.chartData.type": type } },
      { new: true }
    );

    if (!historyChart) {
      return res.status(404).json({ error: "No history chart found." });
    }
    res.json(historyChart);
  } catch (err) {
    console.error(`Error updating chart type for user: ${userId}`, err);
    res
      .status(500)
      .json({ error: "An error occurred while updating chart type." });
  }
});
```

### PUT /change_data_interval/:userId/:chartName

Updates the data interval for a specific chart.

- **Parameters:**

  - `userId` (path parameter): The ID of the user.
  - `chartName` (path parameter): The name of the chart.

- **Request Body:**

  - `data_interval`: The new data interval.

- **Responses:**
  - **200 OK:** Returns the updated history chart.
  - **404 Not Found:** If no history chart or specific chart is found.
  - **500 Internal Server Error:** For unexpected errors.

```
router.put("/change_data_interval/:userId/:chartName", async (req, res) => {
  try {
    const { data_interval } = req.body;
    const { userId, chartName } = req.params;

    const historyChart = await HistoryChart.findOneAndUpdate(
      { userId: userId, "charts.chartName": chartName },
      { $set: { "charts.$.chartData.data_interval": data_interval } },
      { new: true }
    );

    if (!historyChart) {
  return res.status(404).json({ error: "No history chart found." });
}
res.json(historyChart);
} catch (err) {
console.error(Error updating data interval for user: ${userId}, err);
res
.status(500)
.json({ error: “An error occurred while updating data interval.” });
}
});
```

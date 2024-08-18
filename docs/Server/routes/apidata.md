# Data Routes Documentation

This documentation provides a concise overview of the routes handling various data retrieval requests in a Node.js application using Express. These routes interact with the `Data` model and perform operations such as querying, sorting, and filtering based on request parameters.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /latestdata](#get-latestdata)
  - [GET /latestdata/Temperature](#get-latestdata-temperature)
  - [GET /latestdata/Humidity](#get-latestdata-humidity)
  - [GET /latestdata/Moisture/1](#get-latestdata-moisture1)
  - [GET /latestdata/Lux](#get-latestdata-lux)
  - [GET /latestdata/All](#get-latestdata-all)
  - [GET /All/temperature](#get-all-temperature)
  - [GET /tem](#get-tem)
  - [GET /All/humidity](#get-all-humidity)
  - [GET /All/Lux](#get-all-lux)
  - [GET /All/moisture/1](#get-all-moisture1)
  - [GET /All/moisture/2](#get-all-moisture2)
  - [GET /All/moisture/3](#get-all-moisture3)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are required for these routes:

```
const router = require("express").Router();
const { Data, validate } = require("../models/apidata");
```

- **express**: Framework for building web applications.
- **Data**: Mongoose model for interacting with the data collection.
- **validate**: Function to validate request parameters.

## Routes Overview

### GET /latestdata

Returns a simple status message to indicate the server is running.

```
router.get("/latestdata", (req, res) => {
  res.send("Running...");
});
```

### GET /latestdata/Temperature

Retrieves the latest temperature data for a specified group.

```
router.get("/latestdata/Temperature", async (req, res) => {
  // Retrieves the latest temperature data
});
```

### GET /latestdata/Humidity

Retrieves the latest humidity data for a specified group.

```
router.get("/latestdata/Humidity", async (req, res) => {
  // Retrieves the latest humidity data
});
```

### GET /latestdata/Moisture/1

Retrieves the latest moisture data for sensor 1 in a specified group.

```
router.get("/latestdata/Moisture/1", async (req, res) => {
  // Retrieves the latest moisture data for sensor 1
});
```

### GET /latestdata/Lux

Retrieves the latest lux (light intensity) data for a specified group.

```
router.get("/latestdata/Lux", async (req, res) => {
  // Retrieves the latest lux data
});
```

### GET /latestdata/All

Retrieves the latest data from multiple sensors for a specified group.

```
router.get("/latestdata/All", async (req, res) => {
  // Retrieves the latest data from various sensors
});
```

### GET /All/temperature

Retrieves temperature data with filtering options such as date range and value range.

```
router.get("/All/temperature", async (req, res) => {
  // Retrieves temperature data with filtering options
});
```

### GET /tem

Retrieves temperature data for the current day with optional filtering.

```
router.get("/tem", async (req, res) => {
  // Retrieves daily temperature data with optional filtering
});
```

### GET /All/humidity

Retrieves humidity data with filtering options such as date range and value range.

```
router.get("/All/humidity", async (req, res) => {
  // Retrieves humidity data with filtering options
});
```

### GET /All/Lux

Retrieves lux (light intensity) data with filtering options such as date range and value range.

```
router.get("/All/Lux", async (req, res) => {
  // Retrieves lux data with filtering options
});
```

### GET /All/moisture/1

Retrieves moisture data for sensor 1 with filtering options such as date range and value range.

```
router.get("/All/moisture/1", async (req, res) => {
  // Retrieves moisture data for sensor 1 with filtering options
});
```

### GET /All/moisture/2

Retrieves moisture data for sensor 2 with filtering options such as date range and value range.

```
router.get("/All/moisture/2", async (req, res) => {
  // Retrieves moisture data for sensor 2 with filtering options
});
```

### GET /All/moisture/3

Retrieves moisture data for sensor 3 with filtering options such as date range and value range.

```
router.get("/All/moisture/3", async (req, res) => {
  // Retrieves moisture data for sensor 3 with filtering options
});
```

## Error Handling

- **400 Bad Request:** Returned when validation of the request fails.
- **500 Internal Server Error:** Returned when an unexpected error occurs.

## Exported Modules

The router is exported as a module for use in the main application:

```
module.exports = router;
```

This documentation provides an overview of how to interact with the data endpoints in the application.

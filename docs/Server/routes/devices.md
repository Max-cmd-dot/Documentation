# Device Routes Documentation

This documentation describes the routes for managing devices, including retrieving a list of devices and creating a new device.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /](#get--)
  - [GET /create](#get-create)
- [Validation](#validation)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are used in this module:

```
const express = require("express");
const router = express.Router();
const {
  validate,
  Device,
  validate_new_device_request,
} = require("../models/devices");
```

- **express**: Framework for building web applications.
- **validate**: Function to validate query parameters for listing devices.
- **Device**: Mongoose model for device data.
- **validate_new_device_request**: Function to validate query parameters for creating a new device.

## Routes Overview

### GET /

Retrieves a list of devices based on the provided query parameters.

- **Query Parameters:**

  - `group`: The group ID to filter devices by.

- **Responses:**
  - **200 OK:** Returns the list of devices if found.
  - **400 Bad Request:** Returns an error message if the query parameters are invalid or if no data is found.
  - **500 Internal Server Error:** Returns a generic error message if an unexpected error occurs.

```
router.get("/", async (req, res) => {
  try {
    const { group } = req.query;
    console.log(group);
    const { error } = validate(req.query);
    if (error)
      return res.status(400).send({ message: error.details[0].message });
    if (!error) {
      const data = await Device.find({ group: group });
      if (data) {
        return res.status(200).send({ message: data });
      } else {
        return res.status(400).send({ message: "no data found" });
      }
    }
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log(error);
  }
});
```

### GET /create

Creates a new device with the specified parameters.

- **Query Parameters:**

  - `deviceId`: The ID of the new device.
  - `group`: The group ID to which the device belongs.
  - `type`: The type of the device.

- **Responses:**
  - **201 Created:** Returns a success message indicating the device was created.
  - **400 Bad Request:** Returns an error message if the query parameters are invalid.
  - **500 Internal Server Error:** Returns a generic error message if an unexpected error occurs.

```
router.get("/create", async (req, res) => {
  try {
    const { deviceId, group, type } = req.query;
    const { error } = validate_new_device_request(req.query);
    if (error)
      return res.status(400).send({ message: error.details[0].message });
    const device = new Device({ deviceId, group, type });
    if (!error) {
      await device.save();
      res.status(201).send({ message: "Device created successfully" });
    }
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log(error);
  }
});
```

## Validation

The route uses validation functions to ensure that the request parameters meet the required criteria.

- **`validate`**: Checks the query parameters for the `GET /`

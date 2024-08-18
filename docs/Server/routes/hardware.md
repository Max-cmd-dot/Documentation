# Device and Firmware Routes Documentation

This documentation describes the routes for managing device-related operations and firmware updates, including retrieving device details, providing firmware update information, and handling firmware downloads.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /](#get-root)
  - [GET /update](#get-update)
  - [GET /download](#get-download)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are used in this module:

```
const fs = require("fs");
const express = require("express");
const router = express.Router();
const path = require("path");
const { Device } = require("../models/devices");
```

- **fs**: Node.js file system module for interacting with the file system.
- **express**: Framework for building web applications.
- **path**: Node.js module for handling and transforming file paths.
- **Device**: Mongoose model for device data.

## Routes Overview

### GET /

Retrieves device details based on the `deviceId` provided in the request headers.

- **Headers:**

  - `deviceid`: The ID of the device to find.

- **Responses:**
  - **200 OK:** Returns device information including group, host, URL, and API key.
  - **400 Bad Request:** Returned if the device ID is not found or invalid.
  - **500 Internal Server Error:** Returned for unexpected server errors.

```
router.get("/", async (req, res) => {
  const deviceId = req.headers.deviceid;
  try {
    const device = await Device.findOne({ deviceId });
    console.log("Device ID: ", deviceId);
    if (!device) {
      console.error("Device not found");
      return res
        .status(400)
        .json({ message: "Invalid device ID or device not found" });
    }
    console.log("Device found");
    res.json({
      group: device.group,
      host: "eu-central-1.aws.data.mongodb-api.com",
      url: "/app/data-vycfd/endpoint/data/v1",
      apiKey:
        "wvNMOVt8Ad5sd3surfPXFePxxPIJLYe29bSnETeQqwIH7smcbzUM2Lt2t9fbOiDb",
    });
  } catch (err) {
    console.log(err);
    res.status(500).json({ message: err.message });
  }
});
```

### GET /update

Provides firmware update information, including the type, version, and URL for downloading the firmware.

- **Responses:**
  - **200 OK:** Returns an object containing firmware update type, version, and download URL.

```
router.get("/update", async (req, res) => {
  console.log(`Update firmware.bin from ${__dirname}`);
  res.json({
    type: "esp32-fota-http",
    version: "1.0.4",
    url: "http://192.168.178.121:8080/api/hardware/download",
  });
});
```

### GET /download

Handles firmware download requests by sending the firmware file.

- **Responses:**
  - **200 OK:** Initiates a download of the firmware file located at the specified path.
  - **500 Internal Server Error:** Returned if there's an error locating or sending the file.

```
router.get("/download", async (req, res) => {
  console.log(`Download firmware.bin from ${__dirname}`);
  const utilsDir = path.join(
    __dirname,
    "..",
    "..",
    "..",
    "..",
    "Scripts",
    "Hardware",
    "OTA_Esp32",
    ".pio",
    "build",
    "az-delivery-devkit-v4"
  );
  const alternative_dir = path.join(__dirname, "..", "utils");
  const file = `${utilsDir}/firmware.bin`;
  res.download(file); // Set disposition and send it.
});
```

## Error Handling

- **400 Bad Request:** Returned when a device ID is not found or the request is invalid.
- **500 Internal Server Error:** Returned for unexpected errors, including issues with file handling or database operations.

## Exported Modules

The router is exported for use in the main application:

```
module.exports = router;
```

This documentation provides an overview of device management and firmware update routes, including details on request handling, responses, and error scenarios.

# Group Routes Documentation

This documentation describes the routes for managing groups, including checking if a group exists, updating group settings, and fetching group settings.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /checkgroup](#get-checkgroup)
  - [GET /abo](#get-abo)
  - [PUT /update](#put-update)
  - [GET /settings](#get-settings)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are used in this module:

```
const express = require("express");
const router = express.Router();
const { Group } = require("../models/group"); // Replace with the actual model for your work groups
```

- **express**: Framework for building web applications.
- **Group**: Mongoose model for group data.

## Routes Overview

### GET /checkgroup

Checks if a group with the specified name exists.

- **Query Parameters:**

  - `group`: The name of the group to check.

- **Responses:**
  - **200 OK:** Returns a JSON object with a boolean `exists` indicating if the group is found.
  - **500 Internal Server Error:** Returns an error message if an unexpected error occurs.

```
router.get("/checkgroup", async (req, res) => {
  const { group } = req.query;

  try {
    console.log("Group name: ", group);
    const existingGroup = await Group.findOne({ name: group });
    const exists = !!existingGroup;
    console.log("Group exists: ", exists);
    res.json({ exists });
  } catch (error) {
    res.status(500).json({ message: "Internal server error " });
  }
});
```

### GET /abo

Fetches a group based on the specified name.

- **Query Parameters:**

  - `group`: The name of the group to fetch.

- **Responses:**
  - **200 OK:** Returns the group data if found.
  - **500 Internal Server Error:** Returns an error message if an unexpected error occurs.

```
router.get("/abo", async (req, res) => {
  const { group } = req.query;

  try {
    const Abo = await Group.findOne({ name: group });
    if (Abo) return res.json(Abo);
  } catch (error) {
    res.status(500).json({ message: "Internal server error " });
  }
});
```

### PUT /update

Updates the settings of a group based on the provided group ID.

- **Query Parameters:**

  - `groupId`: The name of the group to update.

- **Request Body:**

  - `settings`: The new settings to apply to the group.

- **Responses:**
  - **200 OK:** Returns the updated group data if the update is successful.
  - **404 Not Found:** Returns an error message if the group is not found.
  - **500 Internal Server Error:** Returns an error message if an unexpected error occurs.

```
router.put("/update", async (req, res) => {
  const { groupId } = req.query;
  const { settings } = req.body;
  console.log("Group ID: ", groupId);
  try {
    const group = await Group.findOneAndUpdate(
      { name: groupId },
      { $set: { settings: settings } },
      { new: true }
    );
    if (!group) return res.status(404).json({ message: "Group not found" });

    res.json(group);
  } catch (error) {
    console.log(error);
    res.status(500).json({ message: "Internal server error" });
  }
});
```

### GET /settings

Fetches the settings for a group based on the specified group ID.

- **Query Parameters:**

  - `groupId`: The name of the group whose settings to fetch.

- **Responses:**
  - **200 OK:** Returns the group settings if the group is found.
  - **404 Not Found:** Returns an error message if the group is not found.
  - **500 Internal Server Error:** Returns an error message if an unexpected error occurs.

```
router.get("/settings", async (req, res) => {
  const { groupId } = req.query;

  try {
    const group = await Group.findOne({ name: groupId });
    if (!group) return res.status(404).json({ message: "Group not found" });

    res.json(group.settings);
  } catch (error) {
    console.log(error);
    res.status(500).json({ message: "Internal server error" });
  }
});
```

## Error Handling

- **400 Bad Request:** Not explicitly handled in this implementation; ensure queries and body data are correctly formatted.
- **404 Not Found:** Returned if a group with the specified criteria is not found.
- **500 Internal Server Error:** Returned for unexpected server errors or exceptions.

## Exported Modules

The router is exported for use in the main application:

```
module.exports = router;
```

This documentation provides a detailed overview of the group routes, including request details, error handling, and expected responses.

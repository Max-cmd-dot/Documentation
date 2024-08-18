# Actions Routes Documentation

This documentation provides an in-depth overview of the routes defined for handling actions in a Node.js application using Express. These routes are responsible for managing actions, retrieving the current state, updating actions, and fetching automations. Each route interacts with the `Action` model defined in Mongoose and includes appropriate validation using the `Joi` library.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /](#get-)
  - [GET /current_state](#get-current_state)
  - [POST /update](#post-update)
  - [GET /get_automations](#get-get_automations)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)
- [Code Examples](#code-examples)

## Dependencies

The following dependencies are required for these routes:

```
const express = require("express");
const router = express.Router();
const {
  validate,
  validate_current_state,
  Action,
} = require("../models/actions");
```

- **express**: A minimal and flexible Node.js web application framework.
- **validate** and **validate_current_state**: Functions from the `actions` model to validate request data.
- **Action**: The Mongoose model for actions, imported from the `actions` model.

## Routes Overview

### GET /

This route handles updating the value of a specific action based on query parameters.

```
router.get("/", async (req, res) => {
  try {
    const { group, object, value } = req.query;
    const { error } = validate(req.query);
    if (error)
      return res.status(400).send({ message: error.details[0].message });

    const data = await Action.findOne({ group: group, object: object });
    if (data) {
      data.value = value; // Set the value property
      await data.save();  // Save the updated data object to the database
      return res.status(200).send({ message: "actions success" });
    } else {
      return res.status(400).send({ message: "no data found" });
    }
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log("error");
  }
});
```

**Functionality:**

- Validates the query parameters using `validate`.
- Retrieves an action from the database based on the `group` and `object` values.
- If found, updates the `value` of the action and saves it to the database.
- Returns appropriate success or error messages.

### GET /current_state

This route retrieves the current state of actions for a specific group.

```
router.get("/current_state", async (req, res) => {
  try {
    const { group } = req.query;
    const { error } = validate_current_state(req.query);
    if (error) {
      console.log("error validating");
      return res.status(400).send({ message: error.details[0].message });
    }

    const data = await Action.find({ group: group });
    if (data.length > 0) {
      return res.status(200).send({ message: data });
    } else {
      console.log("no data found");
    }
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log("internal server error" + error);
  }
});
```

**Functionality:**

- Validates the query parameters using `validate_current_state`.
- Retrieves all actions belonging to a specific group.
- Returns the current state of the actions or an error if no data is found.

### POST /update

This route updates the automations of a specific action based on the provided data in the request body.

```
router.post("/update", async (req, res) => {
  try {
    const { object, group, automations } = req.body;

    const { error } = validate(req.body);
    if (error)
      return res.status(400).send({ message: error.details[0].message });

    const updatedAction = await Action.findOneAndUpdate(
      { object, group },
      { $set: { automations } },
      { new: true }
    );

    if (!updatedAction)
      return res.status(404).send({ message: "Action not found" });

    res.status(200).send({ message: "Data updated successfully" });
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log("error", error);
  }
});
```

**Functionality:**

- Validates the request body using `validate`.
- Updates the automations of an existing action.
- Returns success or error messages based on the outcome of the update operation.

### GET /get_automations

This route fetches the automations of a specific action based on query parameters.

```
router.get("/get_automations", async (req, res) => {
  try {
    const { groupId, deviceName } = req.query;

    const action = await Action.findOne({ group: groupId, object: deviceName });

    if (!action) return res.status(404).send({ message: "Action not found" });

    res.status(200).send(action);
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log("error", error);
  }
});
```

**Functionality:**

- Retrieves the automations of a specific action based on `groupId` and `deviceName`.
- Returns the automations data or an error message if the action is not found.

## Error Handling

Each route is equipped with error handling to manage unexpected issues:

- **400 Bad Request**: When validation fails, a 400 status code is returned along with an error message.
- **404 Not Found**: When a requested resource is not found in the database.
- **500 Internal Server Error**: If an unexpected error occurs, a 500 status code is returned with an appropriate error message.

## Exported Modules

The router is exported as a module for use in the main application:

```
module.exports = router;
```

## Code Examples

### Example: Fetching the Current State

To retrieve the current state of actions for a specific group:

```
fetch('/current_state?group=Lights')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Example: Updating an Action

To update the automations of a specific action:

```
fetch('/update', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    object: 'Light',
    group: 'Living Room',
    automations: [...newAutomations],
  }),
})
  .then(response => response.json())
  .then(data => console.log('Success:', data))
  .catch(error => console.error('Error:', error));
```

This documentation outlines the purpose and functionality of the actions routes, providing a clear understanding of how to interact with these endpoints in a Node.js application.

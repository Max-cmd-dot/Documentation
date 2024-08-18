# Actions Model Documentation

This documentation provides an overview of the `actions` model in a Node.js application using Mongoose for MongoDB. The `actions` model is designed to store and validate actions related to objects and groups, with optional automation settings. The validation is handled using the `Joi` library.

## Table of Contents

- [Model Overview](#model-overview)
- [Schema Definition](#schema-definition)
- [Model Creation](#model-creation)
- [Validation](#validation)
- [Exported Modules](#exported-modules)
- [Code Examples](#code-examples)

## Model Overview

The `actions` model defines the structure of documents related to actions within an application. Each action is associated with an object and a group, and can optionally include automation settings for time-based or sensor-based triggers.

## Dependencies

The model file imports the following dependencies:

```
const mongoose = require("mongoose");
const Joi = require("joi");
```

- **mongoose**: A MongoDB object modeling tool for Node.js.
- **Joi**: A powerful schema description language and data validator for JavaScript.

## Schema Definition

The schema for the `actions` model is defined using Mongoose's `Schema` class. The schema includes the following fields:

```
const actionsSchema = new mongoose.Schema({
  object: { type: String, required: true },
  group: { type: String, required: true },
  value: { type: String, required: false },
  automations: [
    {
      timeAutomations: [
        {
          action: { type: String, required: true },
          time: { type: String, required: true },
          frequency: { type: String, required: true },
        },
      ],
      sensorAutomations: [
        {
          action: { type: String, required: true },
          sensor: { type: String, required: true },
          condition: { type: String, required: true },
          value: { type: String, required: true },
        },
      ],
    },
  ],
});
```

### Schema Fields

- **object**: A required string that identifies the object associated with the action.
- **group**: A required string that identifies the group associated with the action.
- **value**: An optional string field that can store additional information about the action.
- **automations**: An optional array that can include automation settings, such as:
  - **timeAutomations**: An array of time-based automations.
  - **sensorAutomations**: An array of sensor-based automations.

## Model Creation

The model is created by compiling the schema into a Mongoose model:

```
const Action = mongoose.model("actions", actionsSchema);
```

This line of code creates a model named `actions` based on the `actionsSchema`.

## Validation

The model includes two validation functions using `Joi` to ensure that the data being saved adheres to the required structure.

### Action Validation

This function validates the main action object:

```
const validate = (action) => {
  const schema = Joi.object({
    object: Joi.string().required().label("Object"),
    group: Joi.string().required(),
    value: Joi.string(),
    automations: Joi.array()
      .items(
        Joi.object({
          timeAutomations: Joi.array()
            .items(
              Joi.object({
                _id: Joi.string().optional(),
                action: Joi.string().optional(),
                time: Joi.string().optional(),
                frequency: Joi.string().optional(),
              }).optional()
            )
            .optional(),
          sensorAutomations: Joi.array()
            .items(
              Joi.object({
                _id: Joi.string().optional(),
                action: Joi.string().optional(),
                sensor: Joi.string().optional(),
                condition: Joi.string().optional(),
                value: Joi.string().optional(),
              }).optional()
            )
            .optional(),
        })
      )
      .optional(),
  });
  return schema.validate(action);
};
```

### Current State Validation

This function validates the current state of an action, specifically ensuring the group field is present:

```
const validate_current_state = (action) => {
  const schema = Joi.object({
    group: Joi.string().required().label("On/Offs"),
  });
  return schema.validate(action);
};
```

## Exported Modules

The file exports the following modules for use in other parts of the application:

```
module.exports = { Action, validate, validate_current_state };
```

- **Action**: The Mongoose model for actions.
- **validate**: A function to validate an action object.
- **validate_current_state**: A function to validate the current state of an action.

## Code Examples

### Example: Creating a New Action

To create a new action, you can use the `Action` model like this:

```
const newAction = new Action({
  object: "Light",
  group: "Living Room",
  value: "On",
  automations: [
    {
      timeAutomations: [
        {
          action: "Turn On",
          time: "18:00",
          frequency: "Daily",
        },
      ],
      sensorAutomations: [
        {
          action: "Turn Off",
          sensor: "Motion",
          condition: "No Movement",
          value: "10 minutes",
        },
      ],
    },
  ],
});

newAction.save()
  .then(() => console.log("Action saved successfully"))
  .catch((err) => console.error("Error saving action:", err));
```

### Example: Validating an Action

Before saving an action, you can validate it using the `validate` function:

```
const { error } = validate(newAction);
if (error) {
  console.error("Validation failed:", error.details);
} else {
  newAction.save()
    .then(() => console.log("Action saved successfully"))
    .catch((err) => console.error("Error saving action:", err));
}
```

This documentation covers the structure, creation, validation, and examples of using the `actions` model in your Node.js application.

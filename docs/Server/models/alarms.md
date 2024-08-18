# Alarms Model Documentation

This documentation provides a comprehensive overview of the `Alarms` model in a Node.js application that uses Mongoose to interact with MongoDB. The `Alarms` model is designed to store and validate alarm notifications, with optional fields for time, topic, value, and other properties. The validation of input data is handled using the `Joi` library.

## Table of Contents

- [Model Overview](#model-overview)
- [Schema Definition](#schema-definition)
- [Model Creation](#model-creation)
- [Validation](#validation)
- [Exported Modules](#exported-modules)
- [Code Examples](#code-examples)

## Model Overview

The `Alarms` model defines the structure of alarm notifications within the application. Each alarm may include information such as the time, topic, value, and group associated with the alarm. All fields are optional, allowing for flexible alarm configurations.

## Dependencies

The model file imports the following dependencies:

```
const mongoose = require("mongoose");
const Joi = require("joi");
```

- **mongoose**: A MongoDB object modeling tool for Node.js.
- **Joi**: A JavaScript library used for data validation.

## Schema Definition

The schema for the `Alarms` model is defined using Mongoose's `Schema` class. The schema includes the following fields:

```
const alarmsSchema = new mongoose.Schema({
  time: { type: Date, required: false },
  topic: { type: String, required: false },
  value: { type: String, required: false },
  ignore: { type: String, required: false },
  group: { type: String, required: false },
});
```

### Schema Fields

- **time**: An optional `Date` field that stores the time of the alarm.
- **topic**: An optional `String` field that represents the topic of the alarm.
- **value**: An optional `String` field that holds the value associated with the alarm.
- **ignore**: An optional `String` field that indicates if the alarm should be ignored.
- **group**: An optional `String` field that identifies the group associated with the alarm.

## Model Creation

The model is created by compiling the schema into a Mongoose model:

```
const Alarms = mongoose.model("Alarms", alarmsSchema);
```

This line of code creates a model named `Alarms` based on the `alarmsSchema`.

## Validation

The model includes a validation function using `Joi` to ensure that the data being saved adheres to the required structure.

### Notification Validation

This function validates the structure of an alarm notification:

```
const validate = (notification) => {
  const schema = Joi.object({
    time: Joi.string(),
    topic: Joi.string(),
    value: Joi.string(),
    ignore: Joi.string(),
    group: Joi.string(),
  });

  return schema.validate(notification);
};
```

### Validation Details

- **time**: Optional string, expected to represent a date or time.
- **topic**: Optional string, representing the topic of the alarm.
- **value**: Optional string, holding a value related to the alarm.
- **ignore**: Optional string, indicating whether to ignore the alarm.
- **group**: Optional string, specifying the group associated with the alarm.

## Exported Modules

The file exports the following modules for use in other parts of the application:

```
module.exports = { Alarms, validate };
```

- **Alarms**: The Mongoose model for alarms.
- **validate**: A function to validate an alarm notification object.

## Code Examples

### Example: Creating a New Alarm

To create a new alarm, you can use the `Alarms` model like this:

```
const newAlarm = new Alarms({
  time: new Date(),
  topic: "High Temperature",
  value: "75°C",
  ignore: "false",
  group: "Sensors",
});

newAlarm.save()
  .then(() => console.log("Alarm saved successfully"))
  .catch((err) => console.error("Error saving alarm:", err));
```

### Example: Validating an Alarm

Before saving an alarm, you can validate it using the `validate` function:

```
const { error } = validate(newAlarm);
if (error) {
  console.error("Validation failed:", error.details);
} else {
  newAlarm.save()
    .then(() => console.log("Alarm saved successfully"))
    .catch((err) => console.error("Error saving alarm:", err));
}
```

This documentation provides a complete overview of how to use the `Alarms` model in your Node.js application, covering schema definition, validation, and practical examples.

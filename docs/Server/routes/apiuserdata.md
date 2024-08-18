# User Routes Documentation

This documentation provides an overview of the route handling user data retrieval in a Node.js application using Express. This route interacts with the `User` model and performs operations such as querying user information by ID.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /:id](#get-id)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are required for this route:

```
const router = require("express").Router();
const { User, validate } = require("../models/apiuserdata");
const bcrypt = require("bcrypt");
const Joi = require("joi");
```

- **express**: Framework for building web applications.
- **User**: Mongoose model for interacting with the user collection.
- **validate**: Function to validate request parameters.
- **bcrypt**: Library for hashing passwords (imported but not used in this specific route).
- **Joi**: Library for schema validation (imported but not used in this specific route).

## Routes Overview

### GET /:id

Retrieves a user by their unique ID.

- **URL Parameter:**

  - `id`: The unique identifier of the user to be retrieved.

- **Request Validation:**

  - Validates the `id` parameter using the `validate` function from the `apiuserdata` model.

- **Responses:**
  - **200 OK:** Returns the user data if found.
  - **400 Bad Request:** Returns an error message if validation fails.
  - **401 Unauthorized:** Returns a message indicating invalid request if the user is not found.
  - **500 Internal Server Error:** Returns a general error message for unexpected issues.

```
router.get("/:id", async (req, res) => {
  try {
    const { error } = validate(req.params);
    if (error)
      return res.status(400).send({ message: error.details[0].message });

    const user = await User.findOne({ _id: req.params.id });

    if (!user) return res.status(401).send({ message: "Invalid request" });

    res.status(200).send(user);
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log("error");
  }
});
```

## Error Handling

- **400 Bad Request:** Returned when validation of the request fails.
- **401 Unauthorized:** Returned when the user is not found.
- **500 Internal Server Error:** Returned when an unexpected error occurs.

## Exported Modules

The router is exported as a module for use in the main application:

```
module.exports = router;
```

This documentation provides an overview of how to interact with the user retrieval endpoint in the application.

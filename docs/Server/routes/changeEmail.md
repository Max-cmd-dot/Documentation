# Email Update Route Documentation

This documentation details the route used to update a user's email address in a Node.js application using Express. This route allows users to change their email by providing a new email address.

## Table of Contents

- [Dependencies](#dependencies)
- [Route Overview](#route-overview)
  - [POST /:userId](#post--userid)
- [Validation](#validation)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are used in this route:

```
const express = require("express");
const router = express.Router();
const { User } = require("../models/user"); // Mongoose model for user
const Joi = require("joi"); // Schema validation library
```

- **express**: Framework for building web applications.
- **User**: Mongoose model for user data.
- **Joi**: Library for validating input data.

## Route Overview

### POST /:userId

Updates the email address for a specific user identified by `userId`.

- **Request Parameters:**

  - `userId`: The ID of the user whose email address is to be updated (URL parameter).

- **Request Body:**

  - `newEmail`: The new email address for the user (required, must be a valid email format).

- **Responses:**
  - **200 OK:** Returns a success message indicating the email was updated.
  - **400 Bad Request:** Returns an error message if the `newEmail` is not valid.
  - **404 Not Found:** Returns an error message if the user with the provided `userId` does not exist.
  - **500 Internal Server Error:** Returns a generic error message if an unexpected error occurs.

```
router.post("/:userId", async (req, res) => {
  // validate the request body first
  const newEmail = req.body.newEmail;
  console.log(newEmail);
  const userId = req.params.userId;
  console.log(userId);

  const schema = Joi.object({
    newEmail: Joi.string().email().required(),
  });

  const { error } = schema.validate({ newEmail });
  if (error) return res.status(400).send(error.details[0].message);

  //find user by id
  let user = await User.findById(userId);
  if (!user) return res.status(404).send("User not found.");

  // update email
  user.email = newEmail;
  user = await user.save();

  res.send({ message: "Email changed successfully" });
});
```

## Validation

The route uses Joi to validate the request body:

```
const schema = Joi.object({
  newEmail: Joi.string().email().required(),
});
```

- **newEmail**: Must be a valid email address and is required.

## Error Handling

- **400 Bad Request:** Returned when the `newEmail` does not meet the validation criteria.
- **404 Not Found:** Returned if the user with the specified `userId` does not exist.
- **500 Internal Server Error:** Returned for unexpected server errors.

**Error Message Example for Internal Server Error:**
“Must be an internal server error. Please log in again and try again. If the error persists, please contact support.”

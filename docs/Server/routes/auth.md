# User Login Route Documentation

This documentation describes the route used for user login in a Node.js application using Express. This route handles user authentication by validating credentials and generating an authentication token.

## Table of Contents

- [Dependencies](#dependencies)
- [Route Overview](#route-overview)
  - [POST /](#post-)
- [Validation](#validation)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)

## Dependencies

The following dependencies are used in this route:

```
const router = require("express").Router();
const { User } = require("../models/user");
const bcrypt = require("bcrypt");
const Joi = require("joi");
```

- **express**: Framework for building web applications.
- **User**: Mongoose model for interacting with the user collection.
- **bcrypt**: Library for comparing hashed passwords.
- **Joi**: Library for schema validation.

## Route Overview

### POST /

Handles user login by validating credentials and generating an authentication token.

- **Request Body:**

  - `email`: User's email address (required).
  - `password`: User's password (required).

- **Responses:**
  - **200 OK:** Returns a success message with the authentication token, user ID, and user group.
  - **400 Bad Request:** Returns an error message if validation fails.
  - **401 Unauthorized:** Returns an error message if the email or password is incorrect.
  - **500 Internal Server Error:** Returns a general error message for unexpected issues.

```
router.post("/", async (req, res) => {
  try {
    const { error } = validate(req.body);
    if (error)
      return res.status(400).send({ message: error.details[0].message });

    const user = await User.findOne({ email: req.body.email });
    if (!user)
      return res.status(401).send({ message: "Invalid Email or Password" });

    const validPassword = await bcrypt.compare(
      req.body.password,
      user.password
    );
    if (!validPassword)
      return res.status(401).send({ message: "Invalid Email or Password" });

    const token = user.generateAuthToken();
    const userid = user._id;
    const group = user.group;

    res.status(200).send({
      data: {
        token: token,
        userId: userid,
        group,
      },
      message: "Logged in successfully",
    });
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
  }
});
```

## Validation

The `validate` function ensures that the request body contains valid data:

```
const validate = (data) => {
  const schema = Joi.object({
    email: Joi.string().email().required().label("Email"),
    password: Joi.string().required().label("Password"),
  });
  return schema.validate(data);
};
```

- **email**: Must be a valid email address and is required.
- **password**: Must be a non-empty string and is required.

## Error Handling

- **400 Bad Request:** Returned when validation of the request body fails.
- **401 Unauthorized:** Returned when the provided email or password is incorrect.
- **500 Internal Server Error:** Returned for unexpected server issues.

## Exported Modules

The router is exported for use in the main application:

```
module.exports = router;
```

This documentation provides an overview of the user login route, including request details, responses, and validation.

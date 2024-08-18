# Alarms Routes Documentation

This documentation provides a detailed overview of the routes related to alarms in a Node.js application using Express. These routes are responsible for retrieving alarm notifications based on specified criteria. The routes interact with the `Notification` model, validating requests using the `Joi` library.

## Table of Contents

- [Dependencies](#dependencies)
- [Routes Overview](#routes-overview)
  - [GET /alarms](#get-alarms)
- [Error Handling](#error-handling)
- [Exported Modules](#exported-modules)
- [Code Examples](#code-examples)

## Dependencies

The following dependencies are required for these routes:

```
const router = require("express").Router();
```

- **express**: A minimal and flexible Node.js web application framework.

## Routes Overview

### GET /alarms

This route handles the retrieval of alarm notifications for a specific group.

```
router.get("/alarms", async (req, res) => {
  try {
    const groupId = req.query.groupId;
    const { error } = validate(req.body);
    if (error)
      return res.status(400).send({ message: error.details[0].message });

    const notifications = await Notification.find({
      ignore: "false",
      group: groupId,
    }).sort({
      _id: -1,
    });
    if (notifications) return res.json(notifications);
  } catch (error) {
    console.log(error);
    res.status(500).send({ message: "Internal Server Error" });
  }
});
```

**Functionality:**

- **Request Validation:** The route expects a `groupId` query parameter and validates the incoming request using the `validate` function (though the request body is not utilized in this specific route).
- **Retrieve Notifications:** Retrieves alarm notifications from the `Notification` model where the `ignore` field is set to `"false"` and the `group` matches the provided `groupId`.
- **Sorting:** The retrieved notifications are sorted in descending order by `_id`, ensuring the latest notifications are returned first.
- **Response:** If notifications are found, they are returned in JSON format. If an error occurs during processing, an appropriate error message is returned.

## Error Handling

The route includes error handling mechanisms to manage various scenarios:

- **400 Bad Request:** If the validation of the incoming request fails, a 400 status code is returned along with a detailed error message.
- **500 Internal Server Error:** If an unexpected error occurs, a 500 status code is returned with an appropriate error message.

## Exported Modules

The router is exported as a module for use in the main application:

```
module.exports = router;
```

## Code Examples

### Example: Fetching Alarm Notifications

To retrieve alarm notifications for a specific group:

```
fetch('/alarms?groupId=12345')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

This documentation outlines the purpose and functionality of the alarms route, providing a clear understanding of how to interact with this endpoint in a Node.js application.

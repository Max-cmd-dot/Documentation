# User Creation Route Documentation

This documentation describes the route for creating a new user and adding them to a specified group.

## Dependencies

- **User**: Mongoose model for managing users.
- **Group**: Mongoose model for managing groups.
- **bcrypt**: Library for hashing passwords.
- **express**: Web application framework.

## Route Overview

### POST / (Create User)

Creates a new user and adds the user to a specified group.

- **Body Params:**

  - `email` (required): The email address of the new user.
  - `password` (required): The password for the new user.
  - `firstName` (required): The first name of the new user.
  - `lastName` (required): The last name of the new user.
  - `group` (required): The name of the group to which the user should be added.

- **Responses:**
  - **201 Created:** Returns a success message indicating the user was created successfully.
  - **400 Bad Request:** If the request body is invalid, if the group name is invalid, or if there are issues with user creation.
  - **409 Conflict:** If a user with the given email already exists.
  - **500 Internal Server Error:** For unexpected server errors.

```
router.post("/", async (req, res) => {
  try {
    const { error } = validate(req.body);
    if (error)
      return res.status(400).send({ message: error.details[0].message });

    const user = await User.findOne({ email: req.body.email });
    if (user)
      return res
        .status(409)
        .send({ message: "User with given email already exists!" });

    const salt = await bcrypt.genSalt(Number(process.env.SALT));
    const hashPassword = await bcrypt.hash(req.body.password, salt);
    const newUser = Object.assign({}, req.body, { password: hashPassword });
    await new User(newUser).save();

    // Add the user's name to the group specified in the request body

    const name = req.body.firstName + req.body.lastName;
    const filter = { name: req.body.group };
    const update = { $push: { employee: name } };
    const options = { new: true, upsert: true };

    const group = await Group.findOneAndUpdate(filter, update, options);
    if (!group) {
      return res.status(400).send({ message: "Invalid group name" });
    }

    // Parse the employee field into an array
    const employeeArray = group.employee ? group.employee.split(",") : [];

    // Add the new employee to the array
    employeeArray.push(name);

    // Convert the array back into a string and update the employee field
    group.employee = employeeArray.join(",");
    await group.save();

    res.status(201).send({ message: "User created successfully" });
  } catch (error) {
    res.status(500).send({ message: "Internal Server Error" });
    console.log("Error in creating user: ", error);
  }
});
```

## Error Handling

- **400 Bad Request**: If the request body fails validation or if the group name is invalid.
- **409 Conflict**: If a user with the given email already exists.
- **500 Internal Server Error**: For unexpected server errors, ensure to log detailed error information for debugging purposes.

This route handles the complete process of user creation and group assignment, including error handling and logging to help troubleshoot issues.

# Password Reset Functionality Documentation

This documentation describes the routes for handling password reset requests.

## Dependencies

- **User**: Mongoose model for managing users.
- **Token**: Mongoose model for managing password reset tokens.
- **sendEmail**: Utility function to send emails.
- **crypto**: Module to generate cryptographic tokens.
- **Joi**: Validation library.
- **joi-password-complexity**: Password complexity validation.
- **bcrypt**: Library for hashing passwords.
- **express**: Web application framework.

## Routes Overview

### POST / (Request Password Reset Link)

Requests a password reset link and sends it to the user's email.

- **Body Params:**

  - `email` (required): The email address of the user requesting a password reset.

- **Responses:**
  - **200 OK:** Returns a success message indicating that the password reset link has been sent.
  - **400 Bad Request:** If the email is invalid or the user does not exist.
  - **500 Internal Server Error:** For unexpected server errors.

```
router.post("/", async (req, res) => {
  try {
    const schema = Joi.object({ email: Joi.string().email().required() });
    const { error } = schema.validate(req.body);
    if (error) return res.status(400).send(error.details[0].message);

    const user = await User.findOne({ email: req.body.email });
    if (!user)
      return res.status(400).send("user with given email doesn't exist");

    let token = await Token.findOne({ userId: user._id });
    if (!token) {
      token = await new Token({
        userId: user._id,
        token: crypto.randomBytes(32).toString("hex"),
      }).save();
    }

    const link = `${apiUrl}/reset_password/${user._id}/${token.token}`;
    const text =
      `In the following you will find the link to reset your password!
      ` + link;
    await sendEmail(user.email, "Password reset", text);

    res.send("password reset link sent to your email account");
  } catch (error) {
    res.send("An error occured");
    console.log(error);
  }
});
```

### POST /resetvalidate (Reset Password)

Validates the reset token and updates the user's password.

- **Body Params:**

  - `password` (required): The new password.
  - `token` (required): The reset token.
  - `_id` (required): The user ID.

- **Responses:**
  - **200 OK:** Returns a success message indicating the password has been reset.
  - **400 Bad Request:** If the token is invalid or expired, or if the user is not found.
  - **500 Internal Server Error:** For unexpected server errors.

```
router.post("/resetvalidate", async (req, res) => {
  try {
    const schema = Joi.object({
      password: passwordComplexity().required().label("Password"),
      token: Joi.string().required(),
      _id: Joi.string().required(),
    });
    const { error } = schema.validate(req.body);
    if (error) return res.status(400).send(error.details[0].message);

    const salt = await bcrypt.genSalt(Number(process.env.SALT));
    const hashPassword = await bcrypt.hash(req.body.password, salt);
    const user = await User.findById(req.body._id);
    if (!user) return res.status(400).send("invalid link or expired");

    const token = await Token.findOne({
      userId: user._id,
      token: req.body.token,
    });
    if (!token) return res.status(400).send("Invalid link or expired");

    user.password = hashPassword;
    await user.save();
    await token.delete();

    res.send("password reset sucessfully.");
  } catch (error) {
    res.send("An error occured");
    console.log(error);
  }
});
```

## Error Handling

- Ensure that errors are logged and appropriate HTTP status codes and messages are returned to the client.

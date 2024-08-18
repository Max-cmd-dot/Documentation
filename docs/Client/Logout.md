# `Logout.jsx` Component Documentation

## Overview

The `Logout.jsx` component is designed to handle the user logout process in a React application. It manages cookie removal, updates the login state, and redirects the user to specific pages after logging out. This component also provides a user-friendly interface that asks for logout confirmation and allows users to either proceed with logout or go back to the main page.

## Prerequisites

Before using the `Logout` component, ensure that the following packages are installed:

```
npm install js-cookie
```

Also, make sure you have integrated `reduxStore` for state management and routing in your application.

## Basic Usage

To use the `Logout` component, follow these steps:

### 1. Import the Component

First, import the `Logout` component into the necessary part of your application.

```
import React, { useState } from "react";
import Logout from "./components/Logout"; // Adjust the path as necessary
```

### 2. Integrating the Component

Integrate the `Logout` component into your application where you want to provide a logout option to users, typically in a menu or settings page.

```
import React, { useState } from "react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Logout from "./components/Logout";
import Home from "./components/Home";

const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(true);

  return (
    <Router>
      <Routes>
        <Route path="/logout" element={<Logout setIsLoggedIn={setIsLoggedIn} />} />
        <Route path="/" element={isLoggedIn ? <Home /> : <Navigate to="/login" />} />
      </Routes>
    </Router>
  );
};

export default App;
```

### 3. Handling Logout State

The `Logout` component accepts a prop, `setIsLoggedIn`, which is a function that updates the authentication state. This allows the rest of your application to react to the user logging out by redirecting them to a login or landing page.

#### Example: Handling Logout and Redirection

```
const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(true);

  return (
    <Router>
      <Routes>
        <Route path="/logout" element={<Logout setIsLoggedIn={setIsLoggedIn} />} />
        <Route path="/" element={isLoggedIn ? <Dashboard /> : <Navigate to="/login" />} />
      </Routes>
    </Router>
  );
};
```

### 4. Customizing the Component

#### Redirect After Logout

By default, the `Logout` component redirects the user to the `/Landing` page after logout. You can easily customize this redirection by modifying the `window.location.href` within the `handleLogout` function.

#### Example: Redirecting to a Custom Page

If you want to redirect the user to a custom page after they log out, simply change the URL in the `handleLogout` function.

```
const handleLogout = (e) => {
  e.preventDefault();
  Cookies.remove("token");
  Cookies.remove("userId");
  Cookies.remove("groupId");
  setIsLoggedIn(false);
  setTimeout(() => {
    window.location.href = "/customPage"; // Redirect to your custom page
  }, 100); // Delay of 100 milliseconds
};
```

### 5. Going Back to the Main Page

The `Logout` component also includes a "Go Back" button that allows users to return to the main page without logging out. This is particularly useful if the user clicks the logout button by mistake.

#### Example: Customizing the "Go Back" Functionality

You can customize the redirection target for the "Go Back" button by modifying the `handlegoback` function.

```
const handlegoback = () => {
  changeRoute("/dashboard"); // Change route to dashboard or any other page
  setTimeout(() => {
    window.location.href = "/dashboard"; // Redirect to the desired page
  }, 100); // Delay of 100 milliseconds
};
```

### 6. Styling the Component

The `Logout` component uses a CSS module (`styles.module.css`) for styling. You can customize the appearance by editing the CSS classes or by overriding the styles with your own CSS.

## Example Use Case

Suppose you want to add a logout option to a user profile page. Here’s how you might set it up:

```
const UserProfile = () => {
  return (
    <div>
      <h1>User Profile</h1>
      <Logout setIsLoggedIn={setIsLoggedIn} />
    </div>
  );
};

const App = () => {
  return (
    <Router>
      <Routes>
        <Route path="/profile" element={<UserProfile />} />
        <Route path="/" element={<Dashboard />} />
      </Routes>
    </Router>
  );
};
```

In this example, the `Logout` component is rendered on the user profile page, allowing users to log out directly from there.

## Conclusion

The `Logout.jsx` component is a simple yet essential component for handling user logout processes in a React application. It is designed to be flexible, allowing for easy customization to fit the specific needs of your project.

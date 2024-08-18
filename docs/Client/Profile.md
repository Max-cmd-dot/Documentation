# Profile Component Documentation

This documentation provides an overview of the `Profile` component in a React application. The `Profile` component is responsible for displaying and managing user profile information, including the user's name, email, group code, and subscription level. It also includes functionality for changing the user's email and, conditionally, managing group employees.

## Table of Contents

- [Component Overview](#component-overview)
- [Dependencies](#dependencies)
- [State Management](#state-management)
- [Effects](#effects)
- [Event Handlers](#event-handlers)
- [Conditional Rendering](#conditional-rendering)
- [Code Examples](#code-examples)

## Component Overview

The `Profile` component is a functional React component that:

- Fetches and displays user data from an API.
- Allows users to change their email.
- Conditionally displays and manages group employees based on the user's subscription level.

## Dependencies

The component imports several libraries and modules:

```
import React, { useState, useEffect } from "react";
import axios from "axios";
import ClipLoader from "react-spinners/ClipLoader";
import Cookies from "js-cookie";
import styles from "./styles.module.css";
```

- **React**: For building the user interface and managing state.
- **axios**: For making HTTP requests to the API.
- **ClipLoader**: A loading spinner component from `react-spinners`.
- **js-cookie**: For handling cookies, which are used to store user-specific data.
- **styles**: A CSS module for styling the component.

## State Management

The component uses several `useState` hooks to manage its internal state:

```
const [userData, setUserData] = useState({});
const [userGroup, setUserGroup] = useState("");
const [groupAbo, setgroupAbo] = useState("");
const [loading, setLoading] = useState(true);
const [groupEmployees, setGroupEmployees] = useState([]);
const [selectedEmployee, setSelectedEmployee] = useState(null);
const [isDeletePopupOpen, setIsDeletePopupOpen] = useState(false);
const [rightabo, setRightabo] = useState(false);
const [isEmailPopupOpen_1, setIsEmailPopupOpen_1] = useState(false);
const [error, setError] = useState("");
const [message, setMessage] = useState("");
const [email, setEmail] = useState("");
const [confirmEmail, setConfirmEmail] = useState("");
const [isMatching, setIsMatching] = useState(false);
const [isValid, setIsValid] = useState(false);
```

These states manage everything from user data, loading status, popup visibility, email validation, and more.

## Effects

The component uses `useEffect` hooks to fetch data and manage side effects:

### Fetch User Data

This `useEffect` fetches user-specific data when the component is first mounted:

```
useEffect(() => {
  const fetchData = async () => {
    try {
      const url = `${apiUrl}/api/apiuserdata/${userId}`;
      const response = await axios.get(url, {
        headers: {
          Authorization: \`Bearer \${token}\`,
        },
      });
      const { firstName, lastName, email, group } = response.data;
      setUserData({ firstName, lastName, email });
      setUserGroup(group);
      const abo = await Abo(group);
      setgroupAbo(abo);
      setLoading(false);
    } catch (error) {
      setLoading(false);
    }
  };

  fetchData();
}, []);
```

### Fetch Group Employees Data

This `useEffect` fetches employee data if the user's subscription level allows it:

```
useEffect(() => {
  if (rightabo === "medium") {
    const fetchGroupEmployeesData = async () => {
      try {
        const response = await axios.get(\`\${apiUrl}/api/group/abo?group=\${groupId}\`);
        setGroupEmployees(response.data.employee);
      } catch (error) {
        setLoading(false);
      }
    };

    fetchGroupEmployeesData();
    const interval = setInterval(fetchGroupEmployeesData, 5000);
    return () => clearInterval(interval);
  }
}, [groupId, rightabo]);
```

## Event Handlers

### Email Change

Handles the logic for changing the user's email:

```
const handleEmailChange = async (event) => {
  event.preventDefault();
  try {
    const response = await axios.post(\`\${apiUrl}/api/changeEmail/\${userId}\`, {
      newEmail: email,
    });
    setMessage(response.data.message);
    setTimeout(() => {
      window.location.href = "/Profile";
    }, 1000);
  } catch (error) {
    setError(error);
  }
};
```

### Popup Controls

Handles the logic for opening and closing the email popup:

```
const openEmailPopup_1 = () => {
  setIsEmailPopupOpen_1(true);
};

const closeEmailPopup_1 = () => {
  setIsEmailPopupOpen_1(false);
};
```

## Conditional Rendering

The component conditionally renders parts of the UI based on the state:

- **Loading Spinner**: Displayed while data is being fetched.
- **Email Popup**: Displayed when the user opts to change their email.
- **Group Employees**: Displayed based on the user's subscription level.

### Example: Loading Spinner

```
{loading ? (
  <div className={styles.loading}>
    <ClipLoader size={150} className={styles.heading} />
  </div>
) : (
  <div className={styles.data}>
    {/* Profile Data Here */}
  </div>
)}
```

## Code Examples

### Fetching User Data

Here's an example of how the user data is fetched and used:

```
useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await axios.get(\`\${apiUrl}/api/apiuserdata/\${userId}\`, {
        headers: { Authorization: \`Bearer \${token}\` },
      });
      setUserData(response.data);
      setLoading(false);
    } catch (error) {
      setLoading(false);
    }
  };
  fetchData();
}, []);
```

### Handling Email Change

This is how the component handles email changes:

```
const handleEmailChange = async (event) => {
  event.preventDefault();
  try {
    const response = await axios.post(\`\${apiUrl}/api/changeEmail/\${userId}\`, {
      newEmail: email,
    });
    setMessage(response.data.message);
    setTimeout(() => {
      window.location.href = "/Profile";
    }, 1000);
  } catch (error) {
    setError(error);
  }
};
```

This documentation covers the main aspects of the `Profile` component, including its structure, state management, effects, event handling, and conditional rendering.

# `Navbar.jsx` Component Documentation

## Overview

The `Navbar.jsx` component is a responsive navigation bar designed for use with a React application. It dynamically renders sidebar content based on the user's subscription level and includes icons for navigation, user profile management, and notifications. The component also fetches subscription data from an API to tailor the sidebar's content.

## Prerequisites

Ensure that the following packages are installed:

```
npm install react-icons react-router-dom axios js-cookie react-redux
```

## Basic Usage

To use the `Navbar` component, follow these steps:

### 1. Import the Component

First, import the `Navbar` component into your application.

```
import React from "react";
import Navbar from "./components/Navbar"; // Adjust the path as necessary
```

### 2. Integrating the Component

Integrate the `Navbar` component into your application where you want the navigation bar to appear, typically at the top level of your app.

```
import React from "react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Navbar from "./components/Navbar";

const App = () => {
  return (
    <Router>
      <Navbar />
      <Routes>
        {/* Define your routes here */}
      </Routes>
    </Router>
  );
};

export default App;
```

### 3. API Integration

The `Navbar` component fetches the user's subscription level from an API. Make sure your backend API is set up correctly and that the `REACT_APP_API_URL` environment variable is configured.

#### Example: Setting up the Environment Variable

In your `.env` file:

```
REACT_APP_API_URL=http://your-api-url.com
```

### 4. Sidebar Data Structure

The sidebar data is structured based on the user's subscription level:

- `StandardSidebarData`: Available to all users.
- `SidebarData`: Available to users with a "small" subscription.
- `SidebarData2`: Available to users with a "medium" subscription.
- `SidebarData3`: Available to users with a "big" subscription.

#### Example: Structure of Sidebar Data

Each `SidebarData` file should export an array of objects, where each object represents a sidebar item.

```
export const SidebarData = [
  {
    title: "Dashboard",
    path: "/dashboard",
    icon: <FaIcons.FaHome />,
    cName: "nav-text",
  },
  // Add more items as needed
];
```

### 5. Data Fetching and State Management

The component uses React's `useState` and `useEffect` hooks to manage the sidebar's visibility and to fetch the user's subscription data.

#### Example: Fetching Subscription Data with Axios

```
useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await axios.get(\`\${apiUrl}/api/group/abo?group=\${group}\`);
      setRightabo(response.data.package);
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };

  fetchData();
  const interval = setInterval(fetchData, 10000);

  return () => {
    clearInterval(interval);
  };
}, []);
```

### 6. Rendering the Sidebar

The sidebar content is rendered dynamically based on the user's subscription level. The `renderSidebarData` function handles this logic, displaying the appropriate sidebar items.

#### Example: Conditional Sidebar Rendering

```
const renderSidebarData = () => {
  let additionalSidebar;
  if (rightabo === "small") {
    additionalSidebar = SidebarData.map((item, index) => (
      <li key={index} className={item.cName}>
        <Link to={item.path} onClick={() => dispatch(changeRoute(item.path))}>
          {item.icon}
          <span>{item.title}</span>
        </Link>
      </li>
    ));
  } else if (rightabo === "medium") {
    additionalSidebar = SidebarData2.map((item, index) => (
      <li key={index} className={item.cName}>
        <Link to={item.path} onClick={() => dispatch(changeRoute(item.path))}>
          {item.icon}
          <span>{item.title}</span>
        </Link>
      </li>
    ));
  } else if (rightabo === "big") {
    additionalSidebar = SidebarData3.map((item, index) => (
      <li key={index} className={item.cName}>
        <Link to={item.path} onClick={() => dispatch(changeRoute(item.path))}>
          {item.icon}
          <span>{item.title}</span>
        </Link>
      </li>
    ));
  }

  const standardSidebar = StandardSidebarData.map((item, index) => (
    <li key={index} className={item.cName}>
      <Link to={item.path} onClick={() => dispatch(changeRoute(item.path))}>
        {item.icon}
        <span>{item.title}</span>
      </Link>
    </li>
  ));

  if (rightabo === "small" || rightabo === "medium" || rightabo === "big") {
    return (
      <>
        {additionalSidebar}
        <hr className="line" />
        {standardSidebar}
      </>
    );
  } else {
    return (
      <>
        <h1>Error. Please try to relogin. If the error persists, please contact support.</h1>
      </>
    );
  }
};
```

### 7. Styling the Component

The `Navbar` component uses a CSS file (`Navbar.css`) for styling. You can customize the appearance by editing this CSS file or by adding your own styles.

#### Example: Basic Styling in `Navbar.css`

```
.navbar {
  background-color: #333;
  height: 80px;
  display: flex;
  justify-content: start;
  align-items: center;
}

.nav-menu {
  background-color: #333;
  width: 250px;
  height: 100vh;
  display: flex;
  justify-content: center;
  position: fixed;
  top: 0;
  left: -100%;
  transition: 850ms;
}

.nav-menu.active {
  left: 0;
  transition: 350ms;
}

.nav-text {
  display: flex;
  justify-content: start;
  align-items: center;
  padding: 8px 16px;
  list-style: none;
  height: 60px;
}
```

### 8. Handling Navigation and Profile Icons

The component includes icons for opening the sidebar, viewing notifications, accessing the user profile, and logging out. These are rendered using the `react-icons` library.

#### Example: Rendering Icons

```
<IconContext.Provider value={{ color: "#fff" }}>
  <div className="navbar">
    <div className="logo">
      <Link to="#" className="menu-bars">
        <FaIcons.FaBars onClick={showSidebar} />
      </Link>
    </div>
    <div className="profile_picture">
      <Link to="/notifications" className="icons">
        <bifrom.BiBell />
      </Link>
      <Link to="/profile" className="icons">
        <feathericons.FiUser />
      </Link>
      <Link to="/logout" className="icons">
        <feathericons.FiLogOut />
      </Link>
    </div>
  </div>
</IconContext.Provider>
```

### 9. Responsive Behavior

The sidebar toggles between visible and hidden states when the user clicks the menu icon. The visibility is controlled by the `sidebar` state and the `showSidebar` function.

#### Example: Toggling Sidebar Visibility

```
const showSidebar = () => setSidebar(!sidebar);

return (
  <nav className={sidebar ? "nav-menu active" : "nav-menu"}>
    <ul className="nav-menu-items" onClick={showSidebar}>
      {/* Sidebar content */}
    </ul>
  </nav>
);
```

## Example Use Case

Suppose you are building a dashboard application where different user roles have access to different features. Here's how you might integrate the `Navbar` component:

```
const Dashboard = () => {
  return (
    <div>
      <Navbar />
      <h1>Welcome to the Dashboard</h1>
      {/* Additional components and routes */}
    </div>
  );
};

const App = () => {
  return (
    <Router>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/" element={<Navigate to="/dashboard" />} />
      </Routes>
    </Router>
  );
};

export default App;
```

In this example, the `Navbar` is part of a `Dashboard` component, offering a tailored navigation experience based on the user's subscription level.

## Conclusion

The `Navbar.jsx` component is a flexible, dynamic navigation bar that adapts to different user roles or subscription levels. It integrates easily into any React application and offers customizable styling and functionality.

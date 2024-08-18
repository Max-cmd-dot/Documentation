# Actions Component Documentation

This documentation provides an overview of the `Actions` component and the associated `Device` component in the project. It is intended for developers working on the codebase to understand the functionality and structure of these components.

## Table of Contents

- [Introduction](#introduction)
- [Dependencies](#dependencies)
- [Component Structure](#component-structure)
  - [Device Component](#device-component)
  - [Actions Component](#actions-component)
- [Helper Functions](#helper-functions)
- [API Interaction](#api-interaction)
- [Redux Integration](#redux-integration)
- [Styling](#styling)

## Introduction

The `Actions` component is responsible for managing and displaying the state of various devices within a group. It interacts with a backend API to fetch, update, and display device states. The component includes functionality for automations, allowing users to set up time-based or sensor-based triggers to control devices.

## Dependencies

The component relies on the following dependencies:

- **React**: Core library for building UI components.
- **React-Redux**: For state management and dispatching actions.
- **Axios**: For making HTTP requests to the backend API.
- **js-cookie**: For handling cookies to manage session or group information.
- **react-icons**: For displaying the settings icon.

## Component Structure

### Device Component

The `Device` component is a memoized functional component that handles the display and management of individual devices. It includes the following key features:

- **Props**:

  - `type`: The type of the device (e.g., "light", "thermostat").
  - `number`: The device number, used to uniquely identify devices of the same type.
  - `list2`: A list of devices from the current group.
  - `toggleDevice`: Callback function to toggle the device state between "on" and "off".
  - `deviceCounts`: Object containing counts of devices by type.
  - `groupId`: The current group ID used for API requests.

- **State**:

  - `isSettingsPopupVisible`: Controls the visibility of the settings popup.
  - `iconBg`: Manages the background color of the settings icon.
  - `timeAutomations` and `sensorAutomations`: Arrays to store automation rules for time-based and sensor-based triggers, respectively.

- **Lifecycle**:

  - Fetches automation data on mount and when `groupId` or `deviceName` changes.
  - Saves automation data to the backend when the "Save" button is clicked.

- **UI Elements**:
  - Displays the device name and state.
  - Provides buttons to start/stop the device.
  - Includes a settings popup for managing automations.

### Actions Component

The `Actions` component is the main container that handles the state of all devices within a group. It includes the following key features:

- **State**:

  - `list2`: List of devices in the current group.
  - `deviceCounts`: Object that tracks the count of devices by type.

- **Lifecycle**:

  - Sets the route to `/actions` on mount.
  - Fetches the current state of devices when the component is mounted and at regular intervals.
  - Updates the device list when the `currentPage` or `groupId` changes.

- **UI Elements**:
  - Displays the current state of all devices.
  - Uses the `Device` component to render each device.
  - Updates device states based on user interaction.

## Helper Functions

### `capitalizeFirstLetter`

Capitalizes the first letter of a given string. Used primarily for formatting device types.

### `countDevices`

Counts the number of devices by type in the `list2` array and returns an object with the counts.

### `fetchData_current_state`

Fetches the current state of all devices in the group from the API and updates the `list2` state.

## API Interaction

The component interacts with the backend API through Axios. The following endpoints are used:

- **GET `/api/actions/get_automations`**: Fetches automation rules for a specific device.
- **POST `/api/actions/update`**: Updates the automation rules for a device.
- **GET `/api/actions/current_state`**: Retrieves the current state of all devices in the group.
- **GET `/api/actions/`**: Toggles the state of a device (on/off).

An Axios instance is created with a base URL configured from the environment variable `REACT_APP_API_URL`.

## Redux Integration

The `Actions` component integrates with Redux by:

- Selecting the `currentPage` state from the Redux store.
- Dispatching the `changeRoute` action to update the route when the component is mounted.

## Styling

The components use CSS modules for styling. Styles are imported from `./styles.module.css` and applied to various elements using the `className` attribute.

## Conclusion

The `Actions` and `Device` components work together to provide a user interface for managing device states and automations within a group. The code is structured to be modular, with reusable components and helper functions, ensuring ease of maintenance and extensibility.

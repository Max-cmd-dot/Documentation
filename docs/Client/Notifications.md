# Notifications Component Documentation

The `Notifications` component is a React component designed to handle and display notifications for a system. It includes functionality for fetching, filtering, and managing notifications, as well as managing user settings related to notifications.

## Table of Contents

- [Dependencies](#dependencies)
- [State Variables](#state-variables)
- [Effects](#effects)
- [Memoized Values](#memoized-values)
- [Functions](#functions)
- [Event Handlers](#event-handlers)
- [Return JSX](#return-jsx)

## Dependencies

This component relies on several external dependencies:

- `react`: Core React library for building the component.
- `axios`: For making HTTP requests to the backend API.
- `react-redux`: For managing global state with Redux.
- `react-icons`: For using icon components, specifically `FiSettings`.
- `react-spinners`: For showing a loading spinner.
- `js-cookie`: For handling cookies.
- `ButtonGroup`: A custom button group component.
- `styles.module.css`: A CSS module for styling the component.

## State Variables

The component uses several `useState` hooks to manage state:

- `list`: An array that stores the fetched notifications.
- `groupId`: A string that stores the group ID fetched from cookies.
- `notificationtype`: A string that stores the current notification type (e.g., "alarms", "log", "release notes").
- `notificationTypeFilter`: A string that stores the current filter for notifications (e.g., "all", "system", "water").
- `releaseNotes`: An array that stores the fetched release notes.
- `isSettingsPopupVisible`: A boolean that determines the visibility of the settings popup.
- `saveStatus`: A string that stores the status of the settings save operation.
- `isLoading`: A boolean that indicates whether data is being loaded.
- **Settings State**:
  - `tempMin`, `tempMax`: Numbers representing the minimum and maximum temperature settings.
  - `moistureMin`, `moistureMax`: Numbers representing the minimum and maximum soil moisture settings.
  - `emailNotification`: A boolean that indicates if email notifications are enabled.
  - `email`: A string that stores the user's email address.
  - `isValid`: A boolean that indicates if the email address is valid.

## Effects

- **Route Change Effect**:

  - This effect dispatches the `changeRoute` action when the component mounts.

- **Fetch Settings Effect**:

  - This effect fetches the user's notification settings from the backend when the component mounts.

- **Fetch Data Effect**:
  - This effect fetches notifications and release notes from the backend. It re-fetches data every 5 seconds.

## Memoized Values

- `systemNotifications`: Filters the `list` state to include only system notifications.
- `waterNotifications`: Filters the `list` state to include only water-related notifications.

## Functions

### `renderNotifications`

This function returns the appropriate notifications to display based on the `notificationTypeFilter` state.

### `renderNotification`

This function returns a JSX element for an individual notification.

### `handleSettingsClick`

This function toggles the visibility of the settings popup.

### `handleSettingsClose`

This function closes the settings popup.

### `handleEmailChange`

This function updates the email state and checks if the email is valid.

### `handleSave`

This function saves the settings by sending a `PUT` request to the backend.

### `handleIgnore`

This function allows the user to ignore specific notifications or all notifications.

### `printButtonLabel`

This function updates the `notificationtype` state based on the button clicked.

### `handleFilter`

This function updates the `notificationTypeFilter` state based on the filter selected.

## Event Handlers

- `handleSettingsClick`: Opens the settings popup.
- `handleSettingsClose`: Closes the settings popup.
- `handleEmailChange`: Handles email input change and validation.
- `handleSave`: Saves the current settings.
- `handleIgnore`: Ignores selected notifications.
- `printButtonLabel`: Handles the selection of notification types (e.g., "alarms", "log", "release notes").
- `handleFilter`: Handles the selection of notification filters (e.g., "all", "system", "water").

## Return JSX

The component renders a set of elements based on the current state:

- **Heading**: The main heading of the component.
- **Settings Icon**: An icon that opens the settings popup when clicked.
- **Settings Popup**: A modal that allows users to adjust their notification settings.
- **Button Groups**: For selecting between "alarms", "log", and "release notes", and for filtering notifications.
- **Notifications List**: Renders the list of notifications based on the selected filter.
- **Release Notes**: Displays release notes if the "release notes" type is selected.
- **No Data Messages**: Displays appropriate messages when there are no notifications or alarms.

## Conclusion

The `Notifications` component is a versatile tool for managing and displaying notifications within a system. It integrates with various external libraries and services to fetch data, manage state, and allow user interaction through a clean and responsive interface.

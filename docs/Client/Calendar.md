# Calendar Component Documentation

This documentation provides an overview of the `Calendar` component, its purpose, and examples of how to use it within your React application. The `Calendar` component is a fully functional calendar built using the `FullCalendar` library, and it is integrated with Redux for state management and Axios for data fetching.

## Table of Contents

- [Introduction](#introduction)
- [Props](#props)
- [Examples](#examples)
  - [Basic Usage](#basic-usage)
  - [Adding Events](#adding-events)
  - [Handling Event Clicks](#handling-event-clicks)
  - [Customizing Event Rendering](#customizing-event-rendering)
- [API Integration](#api-integration)
- [State Management](#state-management)
- [Styling](#styling)

## Introduction

The `Calendar` component displays a fully-featured calendar with day, week, and month views. It is integrated with an API to fetch and display events dynamically. Users can also interact with the calendar by adding or deleting events.

## Props

While the `Calendar` component does not directly accept props, it relies on internal state and API calls to manage its data. It uses Redux to track the current page and Axios to fetch event data from a remote API.

## Examples

### Basic Usage

To use the `Calendar` component in your application, simply import and render it within your JSX.

```
import Calendar from './Calendar';

function App() {
  return (
    <div>
      <Calendar />
    </div>
  );
}
```

### Adding Events

Users can add events to the calendar by selecting a date or date range. A prompt will appear asking for the event title, and the event will be added to the calendar.

```
const handleDateSelect = (selectInfo) => {
  let title = prompt("Please enter a new title for your event");
  let calendarApi = selectInfo.view.calendar;

  calendarApi.unselect();

  if (title) {
    calendarApi.addEvent({
      id: createEventId(),
      title,
      start: selectInfo.startStr,
      end: selectInfo.endStr,
      allDay: selectInfo.allDay,
    });
  }
};
```

### Handling Event Clicks

Users can delete events by clicking on them. A confirmation prompt will appear before the event is removed.

```
const handleEventClick = (clickInfo) => {
  if (
    confirm(
      \`Are you sure you want to delete the event '\${clickInfo.event.title}'\`
    )
  ) {
    clickInfo.event.remove();
  }
};
```

### Customizing Event Rendering

You can customize how events are displayed in the calendar using the `renderEventContent` function. This function allows you to modify the HTML content of each event.

```
function renderEventContent(eventInfo) {
  return (
    <>
      <b>{eventInfo.timeText}</b>
      <i>{eventInfo.event.title}</i>
    </>
  );
}
```

## API Integration

The `Calendar` component integrates with an external API to fetch event data. It uses Axios for making HTTP requests. The component automatically fetches the latest data every 5 seconds when the current page is set to "/calendar".

```
useEffect(() => {
  if (currentPage === "/calendar") {
    const fetchData = async () => {
      try {
        const response = await axios.get(
          \`\${apiUrl}/api/notification/latestdata/notifications?groupId=\${groupId}\`
        );
        const valuesArr = response.data.map((item) => ({
          message: item.message,
          time: new Date(item.time).toISOString(),
          group: item.group,
          timenow: new Date().getTime(),
        }));
        setList2(valuesArr);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };
    fetchData();
    const interval = setInterval(fetchData, 5000);
    return () => {
      clearInterval(interval);
    };
  }
}, [groupId, currentPage]);
```

## State Management

The `Calendar` component uses Redux to manage the application's state, particularly tracking the current page. The `changeRoute` action is dispatched when the component mounts, ensuring that the application's routing state is up-to-date.

```
useEffect(() => {
  dispatch(changeRoute("/forecast"));
}, [dispatch]);
```

## Styling

The `Calendar` component is styled using CSS modules. The relevant styles are imported from the `styles.module.css` file.

```
import styles from "./styles.module.css";

<div className={styles.container}>
  <h1 className={styles.h1}>Calendar</h1>
  <div className={styles.box}>
    <div className={styles.calendar}>
      <FullCalendar
        ...
      />
    </div>
  </div>
</div>
```

## Conclusion

The `Calendar` component is a versatile tool for displaying and managing events in your React application. With features like event addition, deletion, and API integration, it is suitable for a wide range of use cases.

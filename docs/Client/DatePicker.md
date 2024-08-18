# DatePicker Component Documentation

## Overview

The `DatePicker` component is a custom date selection component built using React. It provides a user-friendly interface for selecting dates and includes features such as localization, date formatting, and default date ranges. The component is designed to be flexible, allowing for various customizations based on your application's requirements.

## Key Features

### 1. Custom Date Selection

The `DatePicker` component allows users to select a date from a visual calendar popup. The component can be configured to display either a single date or a range of dates.

- **Single Date Selection**: Selects one specific date.
- **Range Selection**: Allows users to pick a start and end date, perfect for applications requiring date ranges, such as booking systems or reporting tools.

### 2. Localization and Date Formatting

The component supports multiple languages and custom date formats, making it adaptable to different locales and user preferences.

- **Localization**: Supports localization to display the date picker in various languages. This ensures that the component can be used in international applications.
- **Date Formatting**: Users can specify how dates should be displayed, for example, in `MM/DD/YYYY` or `DD/MM/YYYY` format.

### 3. Default Date Ranges

The component can be initialized with default date ranges, such as the current week, month, or custom date ranges, providing users with a convenient starting point.

### 4. Integration with Forms

The `DatePicker` can be easily integrated with form components, allowing selected dates to be submitted as part of form data. It works seamlessly with form libraries like `Formik` or `react-hook-form`.

### 5. Customizable UI

The appearance of the `DatePicker` component can be customized through CSS classes or inline styles, allowing it to fit into various UI themes and designs.

## Usage Examples

### Example 1: Basic Date Picker

This example shows how to use the `DatePicker` to select a single date.

```
import React from "react";
import DatePicker from "./DatePicker";

const App = () => {
  const handleDateChange = (date) => {
    console.log("Selected date:", date);
  };

  return (
    <div>
      <h1>Select a Date</h1>
      <DatePicker onDateChange={handleDateChange} />
    </div>
  );
};

export default App;
```

### Example 2: Date Range Picker

In this example, the `DatePicker` is configured to allow the selection of a date range.

```
import React from "react";
import DatePicker from "./DatePicker";

const App = () => {
  const handleDateRangeChange = (startDate, endDate) => {
    console.log("Selected date range:", startDate, "to", endDate);
  };

  return (
    <div>
      <h1>Select a Date Range</h1>
      <DatePicker range onDateChange={handleDateRangeChange} />
    </div>
  );
};

export default App;
```

### Example 3: Localization and Custom Date Format

This example demonstrates how to localize the `DatePicker` and format the selected date.

```
import React from "react";
import DatePicker from "./DatePicker";

const App = () => {
  const handleDateChange = (date) => {
    console.log("Selected date:", date);
  };

  return (
    <div>
      <h1>Select a Date</h1>
      <DatePicker
        locale="fr"
        dateFormat="DD/MM/YYYY"
        onDateChange={handleDateChange}
      />
    </div>
  );
};

export default App;
```

### Example 4: Setting a Default Date Range

This example sets the default date range to the current week.

```
import React from "react";
import DatePicker from "./DatePicker";

const App = () => {
  const handleDateRangeChange = (startDate, endDate) => {
    console.log("Selected date range:", startDate, "to", endDate);
  };

  return (
    <div>
      <h1>Select a Date Range</h1>
      <DatePicker
        range
        defaultStartDate={new Date()}
        defaultEndDate={new Date(new Date().setDate(new Date().getDate() + 7))}
        onDateChange={handleDateRangeChange}
      />
    </div>
  );
};

export default App;
```

### Example 5: Integrating with Form Libraries

This example shows how to integrate `DatePicker` with `react-hook-form` to manage form state.

```
import React from "react";
import { useForm, Controller } from "react-hook-form";
import DatePicker from "./DatePicker";

const App = () => {
  const { control, handleSubmit } = useForm();

  const onSubmit = (data) => {
    console.log("Form data:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <h1>Submit a Date</h1>
      <Controller
        name="selectedDate"
        control={control}
        render={({ field }) => <DatePicker {...field} />}
      />
      <button type="submit">Submit</button>
    </form>
  );
};

export default App;
```

## Prop Details

- **`onDateChange`**: A callback function that is triggered whenever a date is selected. It receives the selected date (or dates, if in range mode) as arguments.
- **`range`**: A boolean prop that, when set to `true`, enables range selection mode, allowing users to pick a start and end date.
- **`locale`**: A string prop that sets the language and localization for the date picker (e.g., `"en"`, `"fr"`).
- **`dateFormat`**: A string prop that defines how the date should be displayed (e.g., `"MM/DD/YYYY"`).
- **`defaultStartDate`**: A Date object that sets the default start date for the date picker.
- **`defaultEndDate`**: A Date object that sets the default end date for the date picker.

## Conclusion

The `DatePicker` component is a flexible and powerful tool for managing date selection in React applications. Whether you need a simple date picker or a more complex range selection with localization and custom formatting, this component can be adapted to meet your needs.

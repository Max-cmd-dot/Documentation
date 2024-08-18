# ButtonGroup Component Documentation

This documentation provides an overview of the `ButtonGroup` component. It is intended for developers working on the project to understand the functionality and customization options available for this component.

## Table of Contents

- [Introduction](#introduction)
- [Props](#props)
- [Component Structure](#component-structure)
- [Event Handling](#event-handling)
- [Styling](#styling)
- [Customization](#customization)

## Introduction

The `ButtonGroup` component is a reusable UI element that renders a group of buttons. Each button can be customized in terms of size, color, and behavior. The component also supports active and disabled states for the buttons.

## Props

The `ButtonGroup` component accepts the following props:

- `buttons` (array): An array of button labels to display.
- `doSomethingAfterClick` (function): Callback function to execute after a button is clicked.
- `defaultActiveButton` (string or number): The default active button, either by label (string) or index (number).
- `overrideBoxColor` (boolean): If `true`, applies custom styles to override the default box color.
- `overrideButtonColor` (boolean): If `true`, applies custom styles to override the default button color.
- `buttonSize` (string): Sets the font size of the button text.
- `buttonWidth` (string): Sets the width of each button.
- `buttonHeight` (string): Sets the height of each button.
- `activeButton` (string or number): Specifies the currently active button, either by label (string) or index (number).
- `disabled` (boolean): If `true`, disables all buttons in the group.

## Event Handling

The `ButtonGroup` component handles button clicks through the `handleClick` function. This function is called when a button is clicked and triggers the `doSomethingAfterClick` callback passed in as a prop.

```
const handleClick = (event, id) => {
  doSomethingAfterClick(event);
};
```

- **Parameters**:
  - `event`: The click event object.
  - `id`: The index of the clicked button.

## Styling

The component uses CSS classes defined in the `button-group.css` file for styling. Key classes include:

- `box-container`: The container that wraps all buttons.
- `customButton`: The base class for each button.
- `activeButton`: Applied to the currently active button.
- `override-box-color`: Applied to override the default box color when `overrideBoxColor` is `true`.
- `override_button_color`: Applied to override the default button color when `overrideButtonColor` is `true`.
- `disabled`: Applied to buttons when the `disabled` prop is `true`.

## Customization

### Button Appearance

You can customize the appearance of the buttons using the following props:

- `buttonSize`: Adjusts the font size of the button text.
- `buttonWidth`: Sets the width of the buttons.
- `buttonHeight`: Sets the height of the buttons.

These styles are applied inline to each button:

```
style={{
  fontSize: buttonSize,
  width: buttonWidth,
  height: buttonHeight,
}}
```

### Active and Disabled States

The active button is determined based on the `activeButton` prop, which can be either a string (matching the button label) or a number (matching the button index). The `disabled` prop, when `true`, adds a `disabled` class to all buttons, preventing them from being clicked.

```
className={`${
  (typeof activeButton === "string" ? buttonLabel : i) === activeButton
    ? "customButton activeButton"
    : "customButton"
} ${overrideButtonColor ? "override_button_color" : ""}${
  disabled ? "disabled" : "" // Add this line
}`}
```

### Color Overrides

The `overrideBoxColor` and `overrideButtonColor` props allow you to override the default colors of the button group container and buttons, respectively. When these props are set to `true`, additional CSS classes (`override-box-color` and `override_button_color`) are applied.

## Conclusion

The `ButtonGroup` component is a versatile and customizable UI element that can be used to create interactive button groups in your application. It supports various customization options, including size, color, and state management, making it adaptable to different use cases.

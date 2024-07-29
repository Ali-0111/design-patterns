# Design patterns

# 1: Compound-pattern

```
import React, { createContext, useState, useContext } from "react";

const DropdownContext = createContext();

function Dropdown({ children }) {
  const [open, setOpen] = useState(false);
  return (
    <DropdownContext.Provider value={{ open, setOpen }}>
      {children}
    </DropdownContext.Provider>
  );
}

function Toggle() {
  const { open, setOpen } = useContext(DropdownContext);
  return (
    <button onClick={() => setOpen(!open)}>
      {open ? "Close Menu" : "Open Menu"}
    </button>
  );
}

function Menu({ children }) {
  const { open } = useContext(DropdownContext);
  return open && <ul>{children}</ul>;
}

function MenuItem({ children }) {
  return <li>{children}</li>;
}

Dropdown.Toggle = Toggle;
Dropdown.Menu = Menu;
Dropdown.MenuItem = MenuItem;

export default function DropdownMenu() {
  return (
    <Dropdown>
      <Dropdown.Toggle />
      <Dropdown.Menu>
        <Dropdown.MenuItem>Edit</Dropdown.MenuItem>
        <Dropdown.MenuItem>Delete</Dropdown.MenuItem>
      </Dropdown.Menu>
    </Dropdown>
  );
}
```

# 2.

```
import React, { createContext, useState, useContext } from "react";

const ModalContext = createContext();

function Modal({ children }) {
  const [open, setOpen] = useState(false);
  return (
    <ModalContext.Provider value={{ open, setOpen }}>
      {children}
    </ModalContext.Provider>
  );
}

function OpenButton() {
  const { setOpen } = useContext(ModalContext);
  return <button onClick={() => setOpen(true)}>Open Modal</button>;
}

function CloseButton() {
  const { setOpen } = useContext(ModalContext);
  return <button onClick={() => setOpen(false)}>Close</button>;
}

function Content({ children }) {
  const { open } = useContext(ModalContext);
  return open ? <div className="modal">{children}</div> : null;
}

Modal.OpenButton = OpenButton;
Modal.CloseButton = CloseButton;
Modal.Content = Content;

export default function ModalExample() {
  return (
    <Modal>
      <Modal.OpenButton />
      <Modal.Content>
        <h1>Modal Title</h1>
        <p>Some content for the modal.</p>
        <Modal.CloseButton />
      </Modal.Content>
    </Modal>
  );
}

```
# 3.

```
import React, { createContext, useState, useContext } from "react";

const TabsContext = createContext();

function Tabs({ children }) {
  const [activeTab, setActiveTab] = useState(0);
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      {children}
    </TabsContext.Provider>
  );
}

function TabList({ children }) {
  return <div className="tab-list">{children}</div>;
}

function Tab({ index, children }) {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  return (
    <button
      className={activeTab === index ? "active" : ""}
      onClick={() => setActiveTab(index)}
    >
      {children}
    </button>
  );
}

function TabPanels({ children }) {
  const { activeTab } = useContext(TabsContext);
  return <div className="tab-panels">{children[activeTab]}</div>;
}

Tabs.TabList = TabList;
Tabs.Tab = Tab;
Tabs.TabPanels = TabPanels;

export default function TabsExample() {
  return (
    <Tabs>
      <Tabs.TabList>
        <Tabs.Tab index={0}>Tab 1</Tabs.Tab>
        <Tabs.Tab index={1}>Tab 2</Tabs.Tab>
        <Tabs.Tab index={2}>Tab 3</Tabs.Tab>
      </Tabs.TabList>
      <Tabs.TabPanels>
        <div>Content for Tab 1</div>
        <div>Content for Tab 2</div>
        <div>Content for Tab 3</div>
      </Tabs.TabPanels>
    </Tabs>
  );
}
```

# 4.

```
import React, { createContext, useState, useContext } from "react";

const TabsContext = createContext();

function Tabs({ children }) {
  const [activeTab, setActiveTab] = useState(0);
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      {children}
    </TabsContext.Provider>
  );
}

function TabList({ children }) {
  return <div className="tab-list">{children}</div>;
}

function Tab({ index, children }) {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  return (
    <button
      className={activeTab === index ? "active" : ""}
      onClick={() => setActiveTab(index)}
    >
      {children}
    </button>
  );
}

function TabPanels({ children }) {
  const { activeTab } = useContext(TabsContext);
  return <div className="tab-panels">{children[activeTab]}</div>;
}

Tabs.TabList = TabList;
Tabs.Tab = Tab;
Tabs.TabPanels = TabPanels;

export default function TabsExample() {
  return (
    <Tabs>
      <Tabs.TabList>
        <Tabs.Tab index={0}>Tab 1</Tabs.Tab>
        <Tabs.Tab index={1}>Tab 2</Tabs.Tab>
        <Tabs.Tab index={2}>Tab 3</Tabs.Tab>
      </Tabs.TabList>
      <Tabs.TabPanels>
        <div>Content for Tab 1</div>
        <div>Content for Tab 2</div>
        <div>Content for Tab 3</div>
      </Tabs.TabPanels>
    </Tabs>
  );
}
```

#
# 2. HOC pattern
General structure
```

withCounter.js
import React from "react";
import { useState } from "react";
const UpdatedComponent = (OriginalComponent, increaseCount) => {
  function NewComponent(props) {
    const [counter, setCounter] = useState(10);
    return (
      <OriginalComponent
        name="LogRocket"
        counter={counter}
        incrementCounter={() => setCounter((size) => size + increaseCount)}
      />
    );
  }
  return NewComponent;
};
export default UpdatedComponent;
```

# . use of hoc for atoms:
```
import withCounter from "./withCounter";
function HoverIncrease(props) {
  const [fontSize, setFontSize] = useState(10);
  const { counter, incrementCounter } = props;
  return (
    <div>
      <button onMouseOver={() => setFontSize((size) => size + 1)}>
        Increase on hover
      </button>
      <p style={{ fontSize }}>
        Size of font in onMouseOver function: {fontSize}
      </p>
      <p> Value of 'name' in HoverIncrease: {props.name}</p>
      <button onClick={() => incrementCounter()}>Increment counter</button>
      <p> Value of 'counter' in HoverIncrease: {counter}</p>
    </div>
  );
}
export default withCounter(HoverIncrease, 10);

```

```
import { useEffect, useState } from "react";
import withCounter from "./withCounter";
function ClickIncrease(props) {
  const { counter, incrementCounter } = props;
  const [fontSize, setFontSize] = useState(10);
  return (
    <div>
      <button onClick={() => setFontSize((size) => size + 1)}>
        Increase with click
      </button>
      <p style={{ fontSize }}>Size of font in onClick function: {fontSize}</p>
      <p>Value of 'name' in ClickIncrease: {props.name}</p>
      <button onClick={() => incrementCounter()}>Increment counter</button>
      <p> Value of 'counter' in ClickIncrease: {counter}</p>
    </div>
  );
}
export default withCounter(ClickIncrease, 3);

```

# .General case
```
<withAuth>
  <withLayout>
    <withLogging>
      <Component />
    </withLogging>
  </withLayout>
</withAuth>
```

# conainter/presentaional
```
The presentational/container pattern is a common design pattern in React (and other component-based frameworks) that separates concerns by splitting components into two categories:

Presentational Components: These components are primarily concerned with how things look. They receive data and callbacks exclusively via props and have little to no internal state.
Container Components: These components are concerned with how things work. They fetch data, manage state, and handle business logic. They pass data and callbacks down to presentational components.
This pattern helps in creating a clear separation of concerns, improving maintainability and testability. Here are 10 common use cases where the presentational/container pattern is used:

Form Handling:
Presentational Component: Renders the form fields and buttons, and receives form data and submit/cancel callbacks via props.
Container Component: Manages form state, validation, and submission logic.
#

Data Fetching:
Presentational Component: Displays data in a list or table format.
Container Component: Fetches data from an API and passes it down to the presentational component.
#
Authentication:
Presentational Component: Shows login form and error messages.
Container Component: Manages authentication state, handles login/logout actions, and communicates with authentication services.
#
Routing:
Presentational Component: Displays the UI based on the current route.
Container Component: Manages the routing logic, listens for route changes, and provides the necessary data to the presentational component.
#
State Management with Redux:
Presentational Component: Renders UI based on props.
Container Component: Connects to Redux store, maps state and dispatch to props, and passes them to the presentational component.
#
Complex UI Components:
Presentational Component: Renders the complex UI (e.g., modals, dropdowns) based on props.
Container Component: Manages the state and logic for showing/hiding the UI component and handles interactions.
#
Pagination:
Presentational Component: Displays pagination controls and the current page's data.
Container Component: Manages pagination state, fetches data for the current page, and handles page changes.
#
Search and Filter:
Presentational Component: Renders search input, filter options, and results.
Container Component: Manages search and filter state, performs search queries, and filters data.
#
Dashboard:
Presentational Component: Displays various widgets and dashboard elements based on props.
Container Component: Fetches data for the dashboard, manages state for each widget, and handles user interactions.
#
Error Handling:
Presentational Component: Displays error messages or fallback UI.
Container Component: Catches errors, manages error state, and decides when to show the error message.
``

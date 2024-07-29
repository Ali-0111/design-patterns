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

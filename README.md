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


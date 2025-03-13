# SRE TRAINING (DAY 22) - REACTJS


---

## **Introduction to React**  
### **What is React?**  
React is a **JavaScript library** for building user interfaces. It is developed and maintained by **Meta (Facebook)**. React allows developers to create reusable UI components that update efficiently when data changes.

### 🚀 **Key Features of React:**  
- **Component-Based Architecture** → UI is divided into independent, reusable pieces.
- **Virtual DOM** → Enhances performance by updating only the changed parts of the UI.
- **Declarative UI** → Code is more predictable and easier to debug.
- **Hooks (useState, useEffect, etc.)** → Manage state and side effects in functional components.
- **JSX (JavaScript XML)** → Allows writing UI in a syntax similar to HTML inside JavaScript.
- **One-Way Data Binding** → Improves control over the application’s data flow.
- **React Router** → Enables navigation and routing in single-page applications (SPA).
- **React Native** → Extends React for mobile app development using native components.

###  **Understanding the DOM and Virtual DOM**  
The **DOM (Document Object Model)** is a structured representation of a webpage, allowing scripts and programs to manipulate the document's structure, style, and content dynamically. When an update occurs, the traditional DOM updates the entire structure, which can be slow.  
  
The **Virtual DOM**, used by React, is a lightweight copy of the actual DOM. React updates this Virtual DOM first and then efficiently determines what needs to change in the actual DOM. This makes updates much faster and reduces performance bottlenecks.  

### 🔧 **Setting up React in WSL**
```sh
# Step 1: Navigate to your project folder
cd ~/hello-world

# Step 2: Initialize a new React app (using Vite for speed)
npm create vite@latest . --template react

# Step 3: Install dependencies
npm install

# Step 4: Start the development server
npm run dev
```

This will start the React app at **http://localhost:5173/**.  

---

## **Installing Node.js**
To ensure the correct version of Node.js is installed, follow these steps:

```sh
# Remove existing Node.js completely
sudo apt remove nodejs npm -y
sudo apt autoremove -y

# Clean up any remaining Node.js files
sudo rm -rf /usr/local/bin/npm /usr/local/share/man/man1/node* /usr/local/lib/dtrace/node.d ~/.npm ~/.node-gyp /opt/local/bin/node /opt/local/include/node /opt/local/lib/node_modules

# Get the NodeSource setup script for Node.js 20.x
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js and npm from NodeSource repository
sudo apt install -y nodejs

# Verify installation
node -v  # Should show v20.x.x
npm -v   # Should show a compatible npm version

# Install build essentials (needed for some npm packages)
sudo apt install -y build-essential

# Now you can create your React app
npx create-react-app hello-world
```

✅ **Now, Node.js 20.x is installed, and React can be set up properly.**  

---

## **React Mini Project Codes**

### **Counter Component (`Counter.js`)**
```jsx
import React, { useState } from 'react';

function Counter() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
            <button onClick={() => setCount(count - 1)}>Decrement</button>
            <button onClick={() => setCount(0)}>Reset</button>
        </div>
    );
}

export default Counter;
```

### **Todo Component (`Todo.js`)**
```jsx
import React, { useState } from 'react';

function Todo() {
    const [todos, setTodos] = useState([]);
    const [input, setInput] = useState('');

    const addTodo = () => {
        if (input.trim() !== '') {
            setTodos([...todos, input]);
            setInput('');
        }
    };

    const removeTodo = (index) => {
        setTodos(todos.filter((_, i) => i !== index));
    };

    return (
        <div>
            <h1>Todo List</h1>
            <input
                type="text"
                value={input}
                onChange={(e) => setInput(e.target.value)}
                placeholder="Add a todo"
            />
            <button onClick={addTodo}>Add</button>
            <ul>
                {todos.map((todo, index) => (
                    <li key={index}>
                        {todo}
                        <button onClick={() => removeTodo(index)}>Delete</button>
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default Todo;
```
---

## **Theme Switcher Component (`ThemeSwitcher.js`)**
```jsx
import React, { useState } from 'react';

function ThemeSwitcher() {
    const [theme, setTheme] = useState('light');

    const toggleTheme = () => {
        setTheme(theme === 'light' ? 'dark' : 'light');
    };

    return (
        <button 
            onClick={toggleTheme} 
            style={{
                backgroundColor: theme === 'light' ? 'white' : 'black',
                color: theme === 'light' ? 'black' : 'white',
                padding: '10px 20px',
                border: '1px solid gray',
                cursor: 'pointer',
                margin: '10px'
            }}
        >
            {theme === 'light' ? 'Dark Mode' : 'Light Mode'}
        </button>
    );
}

export default ThemeSwitcher;
```

---

## **Clock Component (`Clock.js`)**
```jsx
import React, { useState, useEffect } from 'react';

function Clock() {
    const [time, setTime] = useState(new Date().toLocaleTimeString());

    useEffect(() => {
        const interval = setInterval(() => {
            setTime(new Date().toLocaleTimeString());
        }, 1000);

        return () => clearInterval(interval);
    }, []);

    return (
        <div style={{ margin: '10px', fontSize: '20px' }}>
            <p>Current Time: {time}</p>
        </div>
    );
}

export default Clock;
```

---

## **Quote Generator Component (`QuoteGenerator.js`)**
```jsx
import React, { useState } from 'react';

const quotes = [
    "The only way to do great work is to love what you do. – Steve Jobs",
    "Success is not final, failure is not fatal: It is the courage to continue that counts. – Winston Churchill",
    "Believe you can and you're halfway there. – Theodore Roosevelt",
    "Act as if what you do makes a difference. It does. – William James",
    "Dream big and dare to fail. – Norman Vaughan"
];

function QuoteGenerator() {
    const [quote, setQuote] = useState("Click the button for a quote!");

    const generateQuote = () => {
        const randomIndex = Math.floor(Math.random() * quotes.length);
        setQuote(quotes[randomIndex]);
    };

    return (
        <div style={{ margin: '10px', padding: '10px', border: '1px solid gray', borderRadius: '5px' }}>
            <p>{quote}</p>
            <button onClick={generateQuote}>Get New Quote</button>
        </div>
    );
}

export default QuoteGenerator;
```

---

## **Updated `App.jsx` File**
```jsx
import React from 'react';
import Counter from './Counter';
import Todo from './Todo';
import ThemeSwitcher from './ThemeSwitcher';
import Clock from './Clock';
import QuoteGenerator from './QuoteGenerator';
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';
import './App.css';

function App() {
  return (
    <Router>
      <div className="App">
        <nav>
          <ul>
            <li><Link to="/">Home</Link></li>
            <li><Link to="/counter">Counter</Link></li>
            <li><Link to="/todo">Todo</Link></li>
            <li><Link to="/theme">Theme Switcher</Link></li>
            <li><Link to="/clock">Clock</Link></li>
            <li><Link to="/quotes">Quote Generator</Link></li>
          </ul>
        </nav>
        <Routes>
          <Route path="/" element={<h1>Welcome to My React App</h1>} />
          <Route path="/counter" element={<Counter />} />
          <Route path="/todo" element={<Todo />} />
          <Route path="/theme" element={<ThemeSwitcher />} />
          <Route path="/clock" element={<Clock />} />
          <Route path="/quotes" element={<QuoteGenerator />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```



---

## **4️⃣ Version Compatibility Script (`version_compatibility.sh`)** 
A script i designed to check python compatibility issues. It takes user input as Python version and the testing code for validation. 
```sh
#!/bin/bash

# Function to check if the specified Python version is installed
check_python_version() {
    py_version=$1
    if command -v python$py_version &>/dev/null; then
        echo " Python $py_version found."
        return 0
    else
        echo " Python $py_version is not installed."
        return 1
    fi
}

# Function to execute the user-provided code
run_python_code() {
    py_version=$1
    user_code="$2"

    check_python_version "$py_version" || { echo "⚠️ Install Python $py_version first."; exit 1; }

    echo " Running code in Python $py_version..."
    output=$(python$py_version -c "$user_code" 2>&1)
    exit_code=$?

    if [ $exit_code -eq 0 ]; then
        echo " Works fine!"
    else
        echo " Error: $output"
    fi
}

# Main script execution
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <python_version> '<python_code>'"
    exit 1
fi

PYTHON_VERSION="$1"
USER_CODE="$2"

run_python_code "$PYTHON_VERSION" "$USER_CODE"
```

✅ **This script checks Python version, runs user code, and detects compatibility issues.**  

---


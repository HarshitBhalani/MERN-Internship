# Day 11 Report - Advanced React concepts

**Date:** 15th July 2025  
**Intern:** Harshit Bhalani (231133116003)  
**Company:** CreArt Solutions Pvt. Ltd., Ahmedabad  
**Reporting Manager:** Mr. Alkesh Kaba

## Task Overview
Advanced React concepts including component types, routing, props, and JSX styling techniques

## Learning Objectives Achieved
* ✅ Understanding Functional Components and their implementation
* ✅ Mastering Class Components structure and lifecycle
* ✅ Implementing React Router for navigation
* ✅ Working with Props for data passing
* ✅ Exploring JSX and various styling approaches

## Key Concepts Learned

### 1. Functional Components
```javascript
// Basic Functional Component
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Arrow Function Component with Hooks
const UserProfile = ({ name, age }) => {
  const [isActive, setIsActive] = useState(false);
  
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <button onClick={() => setIsActive(!isActive)}>
        {isActive ? 'Active' : 'Inactive'}
      </button>
    </div>
  );
};
```

### 2. Class Components
```javascript
// Class Component with State and Lifecycle
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    console.log('Component mounted');
  }

  handleIncrement = () => {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <h3>Count: {this.state.count}</h3>
        <button onClick={this.handleIncrement}>
          Increment
        </button>
      </div>
    );
  }
}
```

### 3. React Router Implementation
```javascript
// Browser Router Setup
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/contact">Contact</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </Router>
  );
}
```

### 4. Props Usage
```javascript
// Parent Component
function App() {
  const user = { name: 'Harshit', role: 'Intern' };
  
  return (
    <div>
      <UserCard user={user} isActive={true} />
      <ProductList products={['Laptop', 'Phone', 'Tablet']} />
    </div>
  );
}

// Child Component receiving props
function UserCard({ user, isActive }) {
  return (
    <div>
      <h3>{user.name}</h3>
      <p>Role: {user.role}</p>
      <span>Status: {isActive ? 'Online' : 'Offline'}</span>
    </div>
  );
}
```

### 5. JSX Styling Approaches

**Inline Styling:**
```javascript
const InlineExample = () => {
  const buttonStyle = {
    backgroundColor: '#007bff',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '5px',
    cursor: 'pointer'
  };

  return (
    <button style={buttonStyle}>
      Click Me
    </button>
  );
};
```

**Internal Styling:**
```javascript
const InternalExample = () => {
  return (
    <div>
      <style jsx>{`
        .container {
          display: flex;
          justify-content: center;
          align-items: center;
          height: 100vh;
        }
        
        .card {
          background-color: #f8f9fa;
          padding: 20px;
          border-radius: 8px;
          box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
      `}</style>
      
      <div className="container">
        <div className="card">
          <h2>Internal Styling Example</h2>
        </div>
      </div>
    </div>
  );
};
```

**External Styling:**
```css
/* styles.css */
.header {
  background-color: #282c34;
  color: white;
  padding: 20px;
  text-align: center;
}

.nav-menu {
  display: flex;
  justify-content: space-around;
  background-color: #f1f1f1;
  padding: 10px;
}

.nav-menu a {
  text-decoration: none;
  color: #333;
  font-weight: bold;
}

.nav-menu a:hover {
  color: #007bff;
}
```

```javascript
// Component using external CSS
import './styles.css';

const Header = () => {
  return (
    <header className="header">
      <h1>My React App</h1>
      <nav className="nav-menu">
        <a href="/">Home</a>
        <a href="/about">About</a>
        <a href="/services">Services</a>
      </nav>
    </header>
  );
};
```

## Skills Developed
* Component architecture design and implementation
* State management in both functional and class components
* Client-side routing and navigation
* Props passing and component communication
* Multiple styling approaches in React applications
* JSX syntax and best practices

## Practical Applications
* Built a multi-page application with routing
* Created reusable components with props
* Implemented different styling strategies
* Practiced component lifecycle methods
* Developed navigation systems

## Next Steps
* Explore React Hooks in depth (useState, useEffect, useContext)
* Learn about Component Lifecycle methods
* Practice with more complex routing scenarios
* Study CSS-in-JS libraries (styled-components)
* Begin working with forms and user input handling

## Reflection
Today's session provided comprehensive coverage of React's core concepts. The comparison between functional and class components was enlightening, and understanding different styling approaches gives flexibility in development. The routing implementation opens up possibilities for creating complex single-page applications.

**Status:** Completed ✅  
**Difficulty Level:** Intermediate

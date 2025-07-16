Day 12 Report - React CRUD Operations for EMS Project

Date: 16th July 2025
Intern: Harshit Bhalani (231133116003)
Company: CreArt Solutions Pvt. Ltd., Ahmedabad
Reporting Manager: Mr. Alkesh Kaba

Task Overview
Building Employee Management System (EMS) with React CRUD operations - Add, Read, Update pages with form validation and data management

Learning Objectives Achieved
✅ Creating Add Employee page with form validation
✅ Implementing Read functionality for employee listing
✅ Building Update employee information feature
✅ Working with React forms and state management
✅ Implementing navigation between CRUD pages
✅ Using localStorage for data persistence

Key Concepts Learned

1. Add Employee Page
```javascript
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';

const AddEmployee = () => {
  const navigate = useNavigate();
  const [employee, setEmployee] = useState({
    firstName: '',
    lastName: '',
    email: '',
    contact: '',
    designation: ''
  });

  const [errors, setErrors] = useState({});

  const handleChange = (e) => {
    setEmployee({
      ...employee,
      [e.target.name]: e.target.value
    });
  };

  const validateForm = () => {
    const newErrors = {};
    
    if (!employee.firstName.trim()) {
      newErrors.firstName = 'First name is required';
    }
    
    if (!employee.lastName.trim()) {
      newErrors.lastName = 'Last name is required';
    }
    
    if (!employee.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(employee.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!employee.contact.trim()) {
      newErrors.contact = 'Contact number is required';
    } else if (!/^\d{10}$/.test(employee.contact)) {
      newErrors.contact = 'Contact must be 10 digits';
    }
    
    if (!employee.designation.trim()) {
      newErrors.designation = 'Designation is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (validateForm()) {
      const existingEmployees = JSON.parse(localStorage.getItem('employees')) || [];
      const newEmployee = {
        ...employee,
        id: Date.now()
      };
      
      existingEmployees.push(newEmployee);
      localStorage.setItem('employees', JSON.stringify(existingEmployees));
      
      alert('Employee added successfully!');
      navigate('/employees');
    }
  };

  return (
    <div className="add-employee">
      <h2>Add New Employee</h2>
      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label>First Name:</label>
          <input
            type="text"
            name="firstName"
            value={employee.firstName}
            onChange={handleChange}
            placeholder="Enter first name"
          />
          {errors.firstName && <span className="error">{errors.firstName}</span>}
        </div>

        <div className="form-group">
          <label>Last Name:</label>
          <input
            type="text"
            name="lastName"
            value={employee.lastName}
            onChange={handleChange}
            placeholder="Enter last name"
          />
          {errors.lastName && <span className="error">{errors.lastName}</span>}
        </div>

        <div className="form-group">
          <label>Email:</label>
          <input
            type="email"
            name="email"
            value={employee.email}
            onChange={handleChange}
            placeholder="Enter email address"
          />
          {errors.email && <span className="error">{errors.email}</span>}
        </div>

        <div className="form-group">
          <label>Contact:</label>
          <input
            type="text"
            name="contact"
            value={employee.contact}
            onChange={handleChange}
            placeholder="Enter contact number"
          />
          {errors.contact && <span className="error">{errors.contact}</span>}
        </div>

        <div className="form-group">
          <label>Designation:</label>
          <input
            type="text"
            name="designation"
            value={employee.designation}
            onChange={handleChange}
            placeholder="Enter designation"
          />
          {errors.designation && <span className="error">{errors.designation}</span>}
        </div>

        <div className="form-actions">
          <button type="submit">Add Employee</button>
          <button type="button" onClick={() => navigate('/employees')}>
            Cancel
          </button>
        </div>
      </form>
    </div>
  );
};

export default AddEmployee;
```

2. Read Employees Page
```javascript
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';

const EmployeeList = () => {
  const [employees, setEmployees] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');

  useEffect(() => {
    const storedEmployees = JSON.parse(localStorage.getItem('employees')) || [];
    setEmployees(storedEmployees);
  }, []);

  const handleDelete = (id) => {
    if (window.confirm('Are you sure you want to delete this employee?')) {
      const updatedEmployees = employees.filter(emp => emp.id !== id);
      setEmployees(updatedEmployees);
      localStorage.setItem('employees', JSON.stringify(updatedEmployees));
    }
  };

  const filteredEmployees = employees.filter(employee =>
    employee.firstName.toLowerCase().includes(searchTerm.toLowerCase()) ||
    employee.lastName.toLowerCase().includes(searchTerm.toLowerCase()) ||
    employee.email.toLowerCase().includes(searchTerm.toLowerCase()) ||
    employee.designation.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div className="employee-list">
      <div className="list-header">
        <h2>Employee Management System</h2>
        <Link to="/add-employee" className="add-btn">
          Add New Employee
        </Link>
      </div>

      <div className="search-bar">
        <input
          type="text"
          placeholder="Search employees..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
        />
      </div>

      <table className="employee-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Email</th>
            <th>Contact</th>
            <th>Designation</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {filteredEmployees.length > 0 ? (
            filteredEmployees.map(employee => (
              <tr key={employee.id}>
                <td>{employee.id}</td>
                <td>{employee.firstName}</td>
                <td>{employee.lastName}</td>
                <td>{employee.email}</td>
                <td>{employee.contact}</td>
                <td>{employee.designation}</td>
                <td>
                  <Link to={`/edit-employee/${employee.id}`} className="edit-btn">
                    Edit
                  </Link>
                  <button 
                    onClick={() => handleDelete(employee.id)}
                    className="delete-btn"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="7">No employees found</td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  );
};

export default EmployeeList;
```

3. Update Employee Page
```javascript
import React, { useState, useEffect } from 'react';
import { useNavigate, useParams } from 'react-router-dom';

const EditEmployee = () => {
  const navigate = useNavigate();
  const { id } = useParams();
  
  const [employee, setEmployee] = useState({
    firstName: '',
    lastName: '',
    email: '',
    contact: '',
    designation: ''
  });

  const [errors, setErrors] = useState({});

  useEffect(() => {
    const employees = JSON.parse(localStorage.getItem('employees')) || [];
    const employeeToEdit = employees.find(emp => emp.id === parseInt(id));
    
    if (employeeToEdit) {
      setEmployee(employeeToEdit);
    } else {
      alert('Employee not found!');
      navigate('/employees');
    }
  }, [id, navigate]);

  const handleChange = (e) => {
    setEmployee({
      ...employee,
      [e.target.name]: e.target.value
    });
  };

  const validateForm = () => {
    const newErrors = {};
    
    if (!employee.firstName.trim()) {
      newErrors.firstName = 'First name is required';
    }
    
    if (!employee.lastName.trim()) {
      newErrors.lastName = 'Last name is required';
    }
    
    if (!employee.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(employee.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!employee.contact.trim()) {
      newErrors.contact = 'Contact number is required';
    } else if (!/^\d{10}$/.test(employee.contact)) {
      newErrors.contact = 'Contact must be 10 digits';
    }
    
    if (!employee.designation.trim()) {
      newErrors.designation = 'Designation is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (validateForm()) {
      const employees = JSON.parse(localStorage.getItem('employees')) || [];
      const updatedEmployees = employees.map(emp => 
        emp.id === parseInt(id) ? employee : emp
      );
      
      localStorage.setItem('employees', JSON.stringify(updatedEmployees));
      
      alert('Employee updated successfully!');
      navigate('/employees');
    }
  };

  return (
    <div className="edit-employee">
      <h2>Edit Employee</h2>
      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label>First Name:</label>
          <input
            type="text"
            name="firstName"
            value={employee.firstName}
            onChange={handleChange}
            placeholder="Enter first name"
          />
          {errors.firstName && <span className="error">{errors.firstName}</span>}
        </div>

        <div className="form-group">
          <label>Last Name:</label>
          <input
            type="text"
            name="lastName"
            value={employee.lastName}
            onChange={handleChange}
            placeholder="Enter last name"
          />
          {errors.lastName && <span className="error">{errors.lastName}</span>}
        </div>

        <div className="form-group">
          <label>Email:</label>
          <input
            type="email"
            name="email"
            value={employee.email}
            onChange={handleChange}
            placeholder="Enter email address"
          />
          {errors.email && <span className="error">{errors.email}</span>}
        </div>

        <div className="form-group">
          <label>Contact:</label>
          <input
            type="text"
            name="contact"
            value={employee.contact}
            onChange={handleChange}
            placeholder="Enter contact number"
          />
          {errors.contact && <span className="error">{errors.contact}</span>}
        </div>

        <div className="form-group">
          <label>Designation:</label>
          <input
            type="text"
            name="designation"
            value={employee.designation}
            onChange={handleChange}
            placeholder="Enter designation"
          />
          {errors.designation && <span className="error">{errors.designation}</span>}
        </div>

        <div className="form-actions">
          <button type="submit">Update Employee</button>
          <button type="button" onClick={() => navigate('/employees')}>
            Cancel
          </button>
        </div>
      </form>
    </div>
  );
};

export default EditEmployee;
```

4. Main App Component with Routing
```javascript
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';
import EmployeeList from './components/EmployeeList';
import AddEmployee from './components/AddEmployee';
import EditEmployee from './components/EditEmployee';
import './App.css';

function App() {
  return (
    <Router>
      <div className="App">
        <Routes>
          <Route path="/" element={<Navigate to="/employees" />} />
          <Route path="/employees" element={<EmployeeList />} />
          <Route path="/add-employee" element={<AddEmployee />} />
          <Route path="/edit-employee/:id" element={<EditEmployee />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

5. EMS Project Styling
**CSS Styling:**
```css
.App {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

.add-employee, .edit-employee {
  max-width: 600px;
  margin: 0 auto;
  padding: 30px;
  background: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
  color: #333;
}

.form-group input {
  width: 100%;
  padding: 12px;
  border: 2px solid #ddd;
  border-radius: 6px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.form-group input:focus {
  outline: none;
  border-color: #007bff;
}

.error {
  color: #dc3545;
  font-size: 14px;
  margin-top: 5px;
  display: block;
}

.form-actions {
  display: flex;
  gap: 15px;
  justify-content: flex-end;
  margin-top: 30px;
}

.form-actions button {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.form-actions button[type="submit"] {
  background-color: #28a745;
  color: white;
}

.form-actions button[type="submit"]:hover {
  background-color: #218838;
}

.form-actions button[type="button"] {
  background-color: #6c757d;
  color: white;
}

.form-actions button[type="button"]:hover {
  background-color: #5a6268;
}

.employee-list {
  padding: 20px;
}

.list-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  padding-bottom: 20px;
  border-bottom: 2px solid #eee;
}

.list-header h2 {
  color: #333;
  margin: 0;
}

.add-btn {
  background-color: #007bff;
  color: white;
  padding: 12px 24px;
  text-decoration: none;
  border-radius: 6px;
  font-weight: bold;
  transition: background-color 0.3s;
}

.add-btn:hover {
  background-color: #0056b3;
}

.search-bar {
  margin-bottom: 25px;
}

.search-bar input {
  width: 100%;
  padding: 15px;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  background-color: #f8f9fa;
}

.employee-table {
  width: 100%;
  border-collapse: collapse;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.employee-table th,
.employee-table td {
  padding: 15px;
  text-align: left;
  border-bottom: 1px solid #eee;
}

.employee-table th {
  background-color: #343a40;
  color: white;
  font-weight: bold;
  text-transform: uppercase;
  font-size: 14px;
}

.employee-table tr:hover {
  background-color: #f8f9fa;
}

.edit-btn {
  background-color: #ffc107;
  color: #212529;
  padding: 8px 16px;
  text-decoration: none;
  border-radius: 4px;
  margin-right: 8px;
  font-size: 14px;
  font-weight: bold;
  transition: background-color 0.3s;
}

.edit-btn:hover {
  background-color: #e0a800;
}

.delete-btn {
  background-color: #dc3545;
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  transition: background-color 0.3s;
}

.delete-btn:hover {
  background-color: #c82333;
}
```

Skills Developed
- CRUD operations implementation in React applications
- Form handling with controlled components and validation
- State management using useState and useEffect hooks
- React Router implementation for multi-page navigation
- Local storage integration for data persistence
- Component-based architecture and modular design
- User experience enhancement with search functionality
- Error handling and client-side form validation

Practical Applications
- Built complete Employee Management System with CRUD functionality
- Implemented responsive forms with real-time validation
- Created dynamic employee listing with search capabilities
- Developed seamless navigation between different pages
- Integrated local storage for persistent data management
- Enhanced user interface with modern styling techniques

Next Steps
- Implement advanced search filters and sorting options
- Add pagination for handling large employee datasets
- Create employee profile view with detailed information
- Integrate with backend API for server-side data management
- Add authentication and role-based access control
- Implement data export functionality (PDF, Excel)

Reflection
Today's session was highly productive as I successfully developed a complete Employee Management System with full CRUD functionality. The implementation of Add, Read, and Update operations provided comprehensive understanding of React state management, form handling, and component architecture. Working with localStorage demonstrated practical data persistence techniques, while the routing system created a seamless user experience. The form validation implementation enhanced my skills in creating robust, user-friendly applications. This project serves as an excellent foundation for building more complex React applications in real-world scenarios.

Status: Completed ✅
Difficulty Level: Intermediate

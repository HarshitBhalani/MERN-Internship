#Day 12 Report - React CRUD Operations for EMS Project

Date: 16th July 2025
Intern: Harshit Bhalani (231133116003)
Company: CreArt Solutions Pvt. Ltd., Ahmedabad
Reporting Manager: Mr. Alkesh Kaba

Task Overview
Building Employee Management System (EMS) with React CRUD operations including Add, Read, and Update functionality for employee records

Learning Objectives Achieved
✅ Creating React pages for CRUD operations
✅ Building Add Employee form with validation
✅ Implementing Read functionality with employee listing
✅ Developing Update employee information feature
✅ Managing form state and data handling
✅ Implementing navigation between CRUD pages

Key Concepts Learned

1. Employee Data Structure
```javascript
// Employee object structure
const employee = {
  id: 1,
  firstName: 'Harshit',
  lastName: 'Bhalani',
  email: 'harshit@example.com',
  contact: '9876543210',
  designation: 'Software Developer Intern'
};
```

2. Add Employee Page
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
      // Get existing employees from localStorage
      const existingEmployees = JSON.parse(localStorage.getItem('employees')) || [];
      
      // Add new employee with unique ID
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

3. Read Employees Page (Employee List)
```javascript
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';

const EmployeeList = () => {
  const [employees, setEmployees] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');

  useEffect(() => {
    // Load employees from localStorage
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

      <div className="employee-table">
        <table>
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
    </div>
  );
};

export default EmployeeList;
```

4. Update Employee Page
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
    // Load employee data for editing
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
      // Update employee in localStorage
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

5. Main App Component with Routing
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

6. CSS Styling for EMS
```css
/* App.css */
.App {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.add-employee, .edit-employee {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.error {
  color: red;
  font-size: 12px;
  display: block;
  margin-top: 5px;
}

.form-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  margin-top: 20px;
}

.form-actions button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.form-actions button[type="submit"] {
  background-color: #007bff;
  color: white;
}

.form-actions button[type="button"] {
  background-color: #6c757d;
  color: white;
}

.employee-list {
  padding: 20px;
}

.list-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.add-btn {
  background-color: #28a745;
  color: white;
  padding: 10px 20px;
  text-decoration: none;
  border-radius: 4px;
}

.search-bar {
  margin-bottom: 20px;
}

.search-bar input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.employee-table table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.employee-table th,
.employee-table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.employee-table th {
  background-color: #f8f9fa;
  font-weight: bold;
}

.edit-btn {
  background-color: #ffc107;
  color: black;
  padding: 5px 10px;
  text-decoration: none;
  border-radius: 3px;
  margin-right: 5px;
}

.delete-btn {
  background-color: #dc3545;
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}
```

Skills Developed
- CRUD operations implementation in React
- Form handling and validation
- State management with useState and useEffect hooks
- Local storage integration for data persistence
- React Router for navigation between pages
- Component-based architecture for scalable applications
- User experience enhancement with search functionality
- Error handling and form validation

Practical Applications
- Built complete Employee Management System
- Implemented Add, Read, Update, and Delete operations
- Created responsive and user-friendly interfaces
- Added search functionality for better user experience
- Integrated form validation for data integrity
- Implemented local storage for data persistence

Next Steps
- Add more advanced validation (duplicate email check)
- Implement sorting and filtering options
- Add pagination for large datasets
- Create employee profile view page
- Integrate with backend API instead of local storage
- Add authentication and authorization features

Reflection
Today's session was highly productive as I successfully implemented a complete CRUD application for Employee Management System. The experience of building Add, Read, and Update functionality provided deep insights into React state management, form handling, and component architecture. Working with local storage gave me understanding of data persistence, and the routing implementation made the application feel like a real-world system.

The validation implementation taught me about user experience considerations, and the search functionality enhanced the practical usability of the application. This project serves as a solid foundation for building more complex React applications.

Status: Completed ✅
Difficulty Level: Intermediate-Advanced

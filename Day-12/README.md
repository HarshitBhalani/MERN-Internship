# Day 12 Report - Employee Management System CRUD Operations

**Date:** 16th July 2025  
**Intern:** Harshit Bhalani (231133116003)  
**Company:** CreArt Solutions Pvt. Ltd., Ahmedabad  
**Reporting Manager:** Mr. Alkesh Kaba

## Task Overview
Building Employee Management System (EMS) with CRUD operations including Add, Read, and Update employee pages with form handling and data management.

## Learning Objectives Achieved
✅ Creating Add Employee form with validation  
✅ Implementing Read/Display employee list functionality  
✅ Developing Update Employee form with pre-filled data  
✅ Managing employee data with firstName, lastName, email, contact, designation  
✅ Implementing form handling and state management  

## Key Concepts Learned

### 1. Add Employee Component
```jsx
// AddEmployee.js
import React, { useState } from 'react';

const AddEmployee = () => {
  const [employee, setEmployee] = useState({
    firstName: '',
    lastName: '',
    email: '',
    contact: '',
    designation: ''
  });

  const [errors, setErrors] = useState({});

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setEmployee(prev => ({
      ...prev,
      [name]: value
    }));
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
      // Add employee logic
      console.log('Employee added:', employee);
      alert('Employee added successfully!');
      // Reset form
      setEmployee({
        firstName: '',
        lastName: '',
        email: '',
        contact: '',
        designation: ''
      });
    }
  };

  return (
    <div className="add-employee-container">
      <h2>Add New Employee</h2>
      <form onSubmit={handleSubmit} className="employee-form">
        <div className="form-group">
          <label htmlFor="firstName">First Name:</label>
          <input
            type="text"
            id="firstName"
            name="firstName"
            value={employee.firstName}
            onChange={handleInputChange}
            className={errors.firstName ? 'error' : ''}
          />
          {errors.firstName && <span className="error-message">{errors.firstName}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="lastName">Last Name:</label>
          <input
            type="text"
            id="lastName"
            name="lastName"
            value={employee.lastName}
            onChange={handleInputChange}
            className={errors.lastName ? 'error' : ''}
          />
          {errors.lastName && <span className="error-message">{errors.lastName}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="email">Email:</label>
          <input
            type="email"
            id="email"
            name="email"
            value={employee.email}
            onChange={handleInputChange}
            className={errors.email ? 'error' : ''}
          />
          {errors.email && <span className="error-message">{errors.email}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="contact">Contact:</label>
          <input
            type="tel"
            id="contact"
            name="contact"
            value={employee.contact}
            onChange={handleInputChange}
            className={errors.contact ? 'error' : ''}
          />
          {errors.contact && <span className="error-message">{errors.contact}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="designation">Designation:</label>
          <select
            id="designation"
            name="designation"
            value={employee.designation}
            onChange={handleInputChange}
            className={errors.designation ? 'error' : ''}
          >
            <option value="">Select Designation</option>
            <option value="Software Developer">Software Developer</option>
            <option value="UI/UX Designer">UI/UX Designer</option>
            <option value="Project Manager">Project Manager</option>
            <option value="Business Analyst">Business Analyst</option>
            <option value="QA Engineer">QA Engineer</option>
            <option value="DevOps Engineer">DevOps Engineer</option>
          </select>
          {errors.designation && <span className="error-message">{errors.designation}</span>}
        </div>

        <button type="submit" className="submit-btn">Add Employee</button>
      </form>
    </div>
  );
};

export default AddEmployee;
```

### 2. Read Employee Component
```jsx
// ReadEmployees.js
import React, { useState, useEffect } from 'react';

const ReadEmployees = () => {
  const [employees, setEmployees] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [filteredEmployees, setFilteredEmployees] = useState([]);

  // Sample employee data
  useEffect(() => {
    const sampleEmployees = [
      {
        id: 1,
        firstName: 'John',
        lastName: 'Doe',
        email: 'john.doe@example.com',
        contact: '9876543210',
        designation: 'Software Developer'
      },
      {
        id: 2,
        firstName: 'Jane',
        lastName: 'Smith',
        email: 'jane.smith@example.com',
        contact: '9876543211',
        designation: 'UI/UX Designer'
      },
      {
        id: 3,
        firstName: 'Mike',
        lastName: 'Johnson',
        email: 'mike.johnson@example.com',
        contact: '9876543212',
        designation: 'Project Manager'
      }
    ];
    setEmployees(sampleEmployees);
    setFilteredEmployees(sampleEmployees);
  }, []);

  useEffect(() => {
    const filtered = employees.filter(employee =>
      employee.firstName.toLowerCase().includes(searchTerm.toLowerCase()) ||
      employee.lastName.toLowerCase().includes(searchTerm.toLowerCase()) ||
      employee.email.toLowerCase().includes(searchTerm.toLowerCase()) ||
      employee.designation.toLowerCase().includes(searchTerm.toLowerCase())
    );
    setFilteredEmployees(filtered);
  }, [searchTerm, employees]);

  const handleDelete = (id) => {
    if (window.confirm('Are you sure you want to delete this employee?')) {
      setEmployees(employees.filter(emp => emp.id !== id));
    }
  };

  return (
    <div className="read-employees-container">
      <h2>Employee List</h2>
      
      <div className="search-container">
        <input
          type="text"
          placeholder="Search employees..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
          className="search-input"
        />
      </div>

      <div className="employee-table-container">
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
            {filteredEmployees.map(employee => (
              <tr key={employee.id}>
                <td>{employee.id}</td>
                <td>{employee.firstName}</td>
                <td>{employee.lastName}</td>
                <td>{employee.email}</td>
                <td>{employee.contact}</td>
                <td>{employee.designation}</td>
                <td>
                  <button 
                    className="edit-btn"
                    onClick={() => console.log('Edit:', employee.id)}
                  >
                    Edit
                  </button>
                  <button 
                    className="delete-btn"
                    onClick={() => handleDelete(employee.id)}
                  >
                    Delete
                  </button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>

      {filteredEmployees.length === 0 && (
        <div className="no-employees">
          <p>No employees found.</p>
        </div>
      )}
    </div>
  );
};

export default ReadEmployees;
```

### 3. Update Employee Component
```jsx
// UpdateEmployee.js
import React, { useState, useEffect } from 'react';
import { useParams, useNavigate } from 'react-router-dom';

const UpdateEmployee = () => {
  const { id } = useParams();
  const navigate = useNavigate();
  
  const [employee, setEmployee] = useState({
    firstName: '',
    lastName: '',
    email: '',
    contact: '',
    designation: ''
  });

  const [errors, setErrors] = useState({});
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Simulate fetching employee data
    const fetchEmployee = () => {
      // Sample data - replace with actual API call
      const sampleEmployee = {
        id: parseInt(id),
        firstName: 'John',
        lastName: 'Doe',
        email: 'john.doe@example.com',
        contact: '9876543210',
        designation: 'Software Developer'
      };
      
      setEmployee(sampleEmployee);
      setLoading(false);
    };

    fetchEmployee();
  }, [id]);

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setEmployee(prev => ({
      ...prev,
      [name]: value
    }));
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
      // Update employee logic
      console.log('Employee updated:', employee);
      alert('Employee updated successfully!');
      navigate('/employees');
    }
  };

  if (loading) {
    return <div className="loading">Loading employee data...</div>;
  }

  return (
    <div className="update-employee-container">
      <h2>Update Employee</h2>
      <form onSubmit={handleSubmit} className="employee-form">
        <div className="form-group">
          <label htmlFor="firstName">First Name:</label>
          <input
            type="text"
            id="firstName"
            name="firstName"
            value={employee.firstName}
            onChange={handleInputChange}
            className={errors.firstName ? 'error' : ''}
          />
          {errors.firstName && <span className="error-message">{errors.firstName}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="lastName">Last Name:</label>
          <input
            type="text"
            id="lastName"
            name="lastName"
            value={employee.lastName}
            onChange={handleInputChange}
            className={errors.lastName ? 'error' : ''}
          />
          {errors.lastName && <span className="error-message">{errors.lastName}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="email">Email:</label>
          <input
            type="email"
            id="email"
            name="email"
            value={employee.email}
            onChange={handleInputChange}
            className={errors.email ? 'error' : ''}
          />
          {errors.email && <span className="error-message">{errors.email}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="contact">Contact:</label>
          <input
            type="tel"
            id="contact"
            name="contact"
            value={employee.contact}
            onChange={handleInputChange}
            className={errors.contact ? 'error' : ''}
          />
          {errors.contact && <span className="error-message">{errors.contact}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="designation">Designation:</label>
          <select
            id="designation"
            name="designation"
            value={employee.designation}
            onChange={handleInputChange}
            className={errors.designation ? 'error' : ''}
          >
            <option value="">Select Designation</option>
            <option value="Software Developer">Software Developer</option>
            <option value="UI/UX Designer">UI/UX Designer</option>
            <option value="Project Manager">Project Manager</option>
            <option value="Business Analyst">Business Analyst</option>
            <option value="QA Engineer">QA Engineer</option>
            <option value="DevOps Engineer">DevOps Engineer</option>
          </select>
          {errors.designation && <span className="error-message">{errors.designation}</span>}
        </div>

        <div className="form-actions">
          <button type="submit" className="submit-btn">Update Employee</button>
          <button 
            type="button" 
            className="cancel-btn"
            onClick={() => navigate('/employees')}
          >
            Cancel
          </button>
        </div>
      </form>
    </div>
  );
};

export default UpdateEmployee;
```

### 4. EMS Styling
```css
/* EMS.css */
.add-employee-container,
.read-employees-container,
.update-employee-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.employee-form {
  background: #f8f9fa;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: #333;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.form-group input.error,
.form-group select.error {
  border-color: #dc3545;
}

.error-message {
  color: #dc3545;
  font-size: 14px;
  margin-top: 5px;
  display: block;
}

.submit-btn {
  background-color: #007bff;
  color: white;
  padding: 12px 30px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
}

.submit-btn:hover {
  background-color: #0056b3;
}

.search-container {
  margin-bottom: 20px;
}

.search-input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.employee-table-container {
  overflow-x: auto;
}

.employee-table {
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
  font-weight: 600;
}

.edit-btn,
.delete-btn {
  margin-right: 10px;
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.edit-btn {
  background-color: #28a745;
  color: white;
}

.edit-btn:hover {
  background-color: #218838;
}

.delete-btn {
  background-color: #dc3545;
  color: white;
}

.delete-btn:hover {
  background-color: #c82333;
}

.form-actions {
  display: flex;
  gap: 10px;
}

.cancel-btn {
  background-color: #6c757d;
  color: white;
  padding: 12px 30px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
}

.cancel-btn:hover {
  background-color: #5a6268;
}

.no-employees {
  text-align: center;
  padding: 40px;
  color: #666;
}

.loading {
  text-align: center;
  padding: 40px;
  font-size: 18px;
  color: #666;
}
```

### 5. Main App Component with Routing
```jsx
// App.js
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';
import AddEmployee from './components/AddEmployee';
import ReadEmployees from './components/ReadEmployees';
import UpdateEmployee from './components/UpdateEmployee';
import './EMS.css';

function App() {
  return (
    <Router>
      <div className="App">
        <nav className="navbar">
          <div className="nav-brand">
            <h1>Employee Management System</h1>
          </div>
          <ul className="nav-links">
            <li><Link to="/">Home</Link></li>
            <li><Link to="/add">Add Employee</Link></li>
            <li><Link to="/employees">View Employees</Link></li>
          </ul>
        </nav>

        <main className="main-content">
          <Routes>
            <Route path="/" element={<ReadEmployees />} />
            <Route path="/add" element={<AddEmployee />} />
            <Route path="/employees" element={<ReadEmployees />} />
            <Route path="/update/:id" element={<UpdateEmployee />} />
          </Routes>
        </main>
      </div>
    </Router>
  );
}

export default App;
```

## Skills Developed
- Form handling and validation in React
- State management for complex forms
- CRUD operations implementation
- React Router for navigation between pages
- Data filtering and search functionality
- Component communication and props handling
- Error handling and user feedback
- Responsive table design

## Practical Applications
- Built complete Add Employee form with validation
- Created employee listing page with search functionality
- Implemented Update Employee form with pre-filled data
- Added delete functionality with confirmation
- Designed responsive and user-friendly interface
- Implemented proper error handling and user feedback

## Next Steps
- Implement backend API integration
- Add authentication and authorization
- Create employee dashboard with analytics
- Implement pagination for large datasets
- Add export functionality (PDF, Excel)
- Integrate with database for data persistence
- Add employee profile image upload

## Reflection
Today's session focused on building a complete Employee Management System with CRUD operations. The implementation of form validation, state management, and user interface design provided practical experience in React development. The project structure and component organization follow best practices for scalable applications.

**Status:** Completed ✅  
**Difficulty Level:** Intermediate

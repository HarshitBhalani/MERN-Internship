# Day 4 Report - Node.js Modules & Web Server

**Date:** 5th July 2025  
**Intern:** Harshit Bhalani (231133116003)  
**Company:** CreArt Solutions Pvt. Ltd., Ahmedabad  
**Reporting Manager:** Mr. Alkesh Kaba  

## Task Overview
Exploration of Node.js built-in modules and basic web server creation

## Learning Objectives Achieved
- ✅ OS module implementation and system information access
- ✅ HTTP module understanding and server creation
- ✅ Client-server communication principles
- ✅ Route handling without frameworks
- ✅ Multiple response format handling

## Key Concepts Learned

### 1. OS Module Capabilities
- **System Information Access:**
  - Memory usage monitoring
  - CPU information retrieval
  - Platform type detection
  - File path management
- **Server-side System Interaction:**
  - Host machine details
  - Resource monitoring
  - Environment information

### 2. HTTP Module Fundamentals
- **Web Server Creation:** Building server from scratch
- **Port Listening:** Server configuration and startup
- **Request Processing:** Handling incoming client requests
- **Response Generation:** Sending data back to clients

### 3. Client-Server Communication
- **Request-Response Cycle:** Understanding HTTP protocol
- **Route Handling:** URL-based request routing
- **HTTP Methods:** GET, POST, PUT, DELETE handling
- **Status Codes:** Proper response status management

### 4. Response Formats
- **Plain Text:** Simple string responses
- **HTML:** Structured web content
- **JSON:** API-style data responses
- **Custom Headers:** Content-type management

## Practical Skills Developed
- System information retrieval programming
- Basic web server architecture
- HTTP protocol implementation
- Route handling logic
- Response formatting techniques

## Code Examples Practiced
```javascript
// OS Module Usage
const os = require('os');
console.log('Platform:', os.platform());
console.log('CPU:', os.cpus());
console.log('Memory:', os.totalmem());

// Basic HTTP Server
const http = require('http');
const server = http.createServer((req, res) => {
    const url = req.url;
    if (url === '/') {
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.end('<h1>Home Page</h1>');
    } else if (url === '/api') {
        res.writeHead(200, {'Content-Type': 'application/json'});
        res.end(JSON.stringify({message: 'API Response'}));
    }
});
server.listen(3000);
```

## Technical Achievements
- Successfully created functional web server
- Implemented multiple route handlers
- Handled different response formats
- Established client-server communication

## Next Steps
- Learn Express.js framework
- Implement advanced routing
- Add middleware functionality
- Create RESTful APIs

## Reflection
Building a web server from scratch using just the HTTP module gave me deep insights into how web applications work under the hood. Understanding the request-response cycle and route handling will be invaluable when working with Express.js and building APIs.

---
**Status:** Completed ✅  
**Difficulty Level:** Intermediate

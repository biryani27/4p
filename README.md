# 4p
Program 5: Deploy connectivity between react and node application for inventory management system

// server.js
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const mongoose = require('mongoose');

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(bodyParser.json());
app.use(cors());

// MongoDB connection (replace 'your_connection_string' with your MongoDB connection string)
mongoose.connect('mongodb://localhost:27017/inventory-management', { useNewUrlParser: true, useUnifiedTopology: true });
const connection = mongoose.connection;
connection.once('open', () => {
  console.log('MongoDB connection established successfully');
});

// Mongoose model definition
const Schema = mongoose.Schema;
const inventorySchema = new Schema({
  name: { type: String, required: true },
  quantity: { type: Number, required: true },
});
const Inventory = mongoose.model('Inventory', inventorySchema);

// Routes
app.get('/', (req, res) => {
  res.send('Welcome to the Inventory Management System API');
});

// Route to add inventory item
app.post('/inventory/add', (req, res) => {
  const { name, quantity } = req.body;

  const newItem = new Inventory({
    name: name,
    quantity: quantity,
  });

  newItem.save()
    .then(item => res.json('Inventory item added successfully'))
    .catch(err => res.status(400).json('Error: ' + err));
});

// Start server
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});



//App.js
import React from 'react';
import InventoryList from './components/InventoryList';

function App() {
  return (
    <div className="App">
      <InventoryList />
    </div>
  );
}

export default App;

//InventoryForm.js
import React, { useState } from 'react';
import axios from 'axios';

const InventoryForm = () => {
  const [itemName, setItemName] = useState('');
  const [itemQuantity, setItemQuantity] = useState('');

  const handleAddItem = async () => {
    try {
      // Send a POST request to the server to add the inventory item
      await axios.post('http://localhost:5000/inventory/add', {
        name: itemName,
        quantity: parseInt(itemQuantity, 10), // Convert quantity to an integer
      });

      // Clear input fields after successful addition
      setItemName('');
      setItemQuantity('');

      // Handle success, maybe show a success message or update the inventory list
    } catch (error) {
      // Handle error, show an error message or log to console
      console.error(error);
    }
  };

  return (
    <div>
      <h2>Add Inventory Item</h2>
      <label htmlFor="itemName">Item Name:</label>
      <input
        type="text"
        id="itemName"
        placeholder="Enter item name"
        value={itemName}
        onChange={(e) => setItemName(e.target.value)}
      />
      <label htmlFor="itemQuantity">Quantity:</label>
      <input
        type="number"
        id="itemQuantity"
        placeholder="Enter quantity"
        value={itemQuantity}
        onChange={(e) => setItemQuantity(e.target.value)}
      />
      <button onClick={handleAddItem}>Add Item</button>
    </div>
  );
};

export default InventoryForm;
















PROGRAM 4: Design an employee Management system using RESTFULL APIs
server.js
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = process.env.PORT || 3001;

app.use(cors());
app.use(bodyParser.json());

// MongoDB connection
mongoose.connect('mongodb://localhost:27017/employee-management', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Define Employee Schema
const employeeSchema = new mongoose.Schema({
  name: String,
  position: String,
  department: String,
});

const Employee = mongoose.model('Employee', employeeSchema);

// API routes
app.get('/api/employees', async (req, res) => {
  try {
    const employees = await Employee.find();
    res.json(employees);
  } catch (error) {
    res.status(500).json({ error: 'Internal Server Error' });
  }
});

app.post('/api/employees', async (req, res) => {
  const { name, position, department } = req.body;

  try {
    const newEmployee = new Employee({ name, position, department });
    await newEmployee.save();
    res.status(201).json(newEmployee);
  } catch (error) {
    res.status(500).json({ error: 'Internal Server Error' });
  }
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
AddEmployeeForm.js

import React from 'react';

const AddEmployeeForm = ({ newEmployee, onInputChange, onAddEmployee }) => {
  return (
    <div>
      <h2>Add Employee</h2>
      <label>Name:</label>
      <input type="text" name="name" value={newEmployee.name} onChange={onInputChange} />
      <label>Position:</label>
      <input type="text" name="position" value={newEmployee.position} onChange={onInputChange} />
      <label>Department:</label>
      <input type="text" name="department" value={newEmployee.department} onChange={onInputChange} />
      <button onClick={onAddEmployee}>Add Employee</button>
    </div>
  );
};

export default AddEmployeeForm;



EmployeeList.js
// components/EmployeeList.js

import React from 'react';

const EmployeeList = ({ employees }) => {
  return (
    <div>
      <h2>Employee List</h2>
      <ul>
        {employees.map(employee => (
          <li key={employee._id}>
            {employee.name} - {employee.position} - {employee.department}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default EmployeeList;




OUTPUT

  Local:            http://localhost:3000
  On Your Network:  http://192.168.1.8:3000




styles.css


/* styles.css */

body {
    font-family: 'Arial', sans-serif;
  }
  
  .app {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }
  
  h1, h2 {
    color: #333;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  li {
    margin-bottom: 10px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
  }
  
  input {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
  }
  
  button {
    background-color: #4caf50;
    color: white;
    padding: 10px;
    border: none;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #45a049;
  }
  





 

Using MongoDB Command-Line Interface:
1.	Open a new terminal.
2.	Start the MongoDB shell by typing the following command:

Output for use employee-management
 

Display all documents in the employees collection

 

Collection of Document in Employee-Management
 

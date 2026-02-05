# Express PostgreSQL Delete API 🚀

A beginner-friendly **Express.js REST API** that demonstrates how to **delete records from a PostgreSQL database** using multiple approaches:
- URL parameters
- Request body
- Request body validation

This project completes the **DELETE operation** of a CRUD backend system.

---
## 📂 Project Structure
```
express-postgresql-crud-api
│
├── db.js # PostgreSQL connection pool
├── delete.js # Express server & API routes
├── package.json
└── README.md
```
## ⚙️ Configure the PostgreSQL connection pool (`db.js`)
```
const { Pool } = require('pg');
const pool = new Pool({
   user: 'your_username',
   host: 'localhost',
   database: 'your_database_name',
   password: 'YOUR_PASSWORD',
   port: 5432,                     // Default PostgreSQL port

  });
  
  module.exports = pool;
```
## Create Your Express Server
Create a file: delete.js
```
// delete.js
const express = require('express');
const pool = require('./db');
const { body, validationResult } = require('express-validator');
const bodyParser = require('body-parser');
const app = express();
const PORT = 9000;

// Middleware to parse JSON
app.use(bodyParser.json());

// Test the connection
app.get('/',(req, res) => {
    res.send(`<h1>EXPRESS JS API</h1>`);
});
//----------------------------------Id pareams------------------------------
app.delete('/delemploye/:id', async (req, res) => {
  try {
      const { id } = req.params; // Get the ID from request parameters
      const result = await pool.query('DELETE FROM employe WHERE id = $1',[id]); // Execute the query

      if (result.rowCount === 0) {
          return res.status(404).send({ message: 'Employe item not found' });
      }
      
      res.status(200).send({ message: 'Employe item deleted successfully' });
  } catch (err) {
      console.error('Error executing query', err.stack);
      res.status(500).send('Internal Server Error');
  }
});
//------------------------------------bodyParser------------------------
// Define a route to query the database
app.delete('/delemploye', async (req, res) => {
    try {
      const { id } = req.body; 
      const rs = await pool.query('SELECT * FROM employe WHERE id = $1', [id]);
      console.log(rs.rows.length) 
      if(rs.rows.length>0){
         await pool.query('delete FROM employe WHERE id = $1', [id]); // Adjust query to your table
              
         res.send('{status:"200" ,message: "delete success "}');
      }else{
                res.send('{status:"400" ,message: "delete failed"}');
           }
    } catch (err) {
      console.error('Error executing query', err.stack);
      res.status(500).send('Internal Server Error');
    }
  });
          //--------------------------validation---------------------------
          app.delete('/delemployeById', [ 
            body('id').notEmpty().withMessage('id is required'),
            ]   ,async (req, res) => {
            try {
                const errors = validationResult(req);
             if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
             }else{
            const { id } = req.body; 
             const rs= await pool.query('Select * FROM employe WHERE id = $1', [id]);
            if (rs.rows.length > 0) {
                  await pool.query('delete FROM employe WHERE id = $1', [id]);
                   res.status(200).json({ status: 'success', message: 'delete successful' });
                } else {
                  res.status(400).json({ status: 'failed', message: 'delete failed' });
                }
            }
            }catch (err) {
              console.error('Error executing query', err.stack);
              res.status(500).send('Internal Server Error');
            }
          });
          // Start the server
  app.listen(PORT,() => {
    console.log(`Server running on http://localhost:${PORT}`);
  });  
 ``` 
 ## 🚀 API Endpoints
### 🔹 Test API
GET /

#### Response:
```
<h1>EXPRESS JS API</h1>
```
### 🔹 Delete Employee (URL Parameter)
DELETE /delemploye/:id

Example:
```
DELETE /delemploye/5
```
#### Success Response:
```
{
  "message": "Employe item deleted successfully"
}
```
### 🔹 Delete Employee (Request Body)
DELETE /delemploye


#### Request Body:
```
{
  "id": 5
}
```
#### Response:
```
{
  "status": "200",
  "message": "delete success"
}
```
### 🔹 Delete Employee (With Validation)
DELETE /delemployeById


#### Request Body:
```
{
  "id": 5
}
```
- Validation Rule
- id is required
```
 Success Response
{
  "status": "success",
  "message": "delete successful"
}
Error Response
{
  "errors": [
    {
      "type": "field",
      "msg": "id is required",
      "path": "id",
      "location": "body"
    }
  ]
}
 ```
## 🎯 Learning Outcomes
- Handling DELETE requests in Express.js
- Deleting records using URL parameters & request body
- Input validation using express-validator
- Checking record existence before delete
- Using correct HTTP status codes

## 👨‍💻 Author

- Kumlesh Kurre
- Backend Developer
- Skills: Express.js | Node.js | PostgreSQL | REST APIs
 
## ⭐ Support
If you like this project, please ⭐ star the repository to support my work!
  

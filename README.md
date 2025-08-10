# PRODIGY_FD_02
EMPLOYEE MANAGEMENT SYSTEM
. Frontend Development (React.js)
Set up React Project:

bash
Copy
Edit
npx create-react-app employee-management
cd employee-management
npm install react-router-dom tailwindcss axios
Configure Tailwind CSS:

bash
Copy
Edit
npx tailwindcss init
In tailwind.config.js, add:

js
Copy
Edit
module.exports = {
  content: ["./src/**/*.{html,js,jsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
In src/index.css, add:

css
Copy
Edit
@tailwind base;
@tailwind components;
@tailwind utilities;
Create Components:

Login.js: User login form.

Dashboard.js: Admin dashboard with navigation.

EmployeeList.js: Display list of employees.

EmployeeForm.js: Form to add or edit employee details.

2. Backend Development (Node.js & Express.js)
Set up Node.js Project:

bash
Copy
Edit
mkdir server
cd server
npm init -y
npm install express mongoose cors dotenv jsonwebtoken bcryptjs
Create Server:
In server.js:

js
Copy
Edit
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const dotenv = require('dotenv');

dotenv.config();
const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.log(err));

app.listen(5000, () => {
  console.log('Server running on http://localhost:5000');
});
Create Models:

Employee.js: Schema for employee data.

User.js: Schema for user authentication.

Create Routes:

authRoutes.js: Routes for login and registration.

employeeRoutes.js: Routes for CRUD operations on employees.

3. Authentication with JWT
User Registration:

js
Copy
Edit
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const User = require('../models/User');

const register = async (req, res) => {
  const { username, email, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  const newUser = new User({ username, email, password: hashedPassword });
  await newUser.save();
  res.status(201).send('User registered');
};
User Login:

js
Copy
Edit
const login = async (req, res) => {
  const { email, password } = req.body;
  const user = await User.findOne({ email });
  if (!user) return res.status(400).send('User not found');
  const isMatch = await bcrypt.compare(password, user.password);
  if (!isMatch) return res.status(400).send('Invalid credentials');
  const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET, { expiresIn: '1h' });
  res.json({ token });
};
Protect Routes:

js
Copy
Edit
const jwt = require('jsonwebtoken');

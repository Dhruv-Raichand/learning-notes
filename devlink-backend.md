# Backend Development - Learning Notes

## Project Setup

- Create a repository
- Initialize the repository
- Understanding node_modules, package.json, package-lock.json
- Install express
- Create a server
- Listen to port 7777
- Write request handlers for /test, /hello
- Install nodemon and update scripts inside package.json
- What are dependencies
- What is the use of "-g" while npm install
- Difference between caret and tilde (^ vs ~)

## Git & Version Control

- Initialize git
- Create .gitignore
- Create a remote repo on GitHub
- Push all code to remote origin

## Routing Basics

- Play with routes and route extensions ex. /hello, /, /hello/2, /xyz
- Order of the routes matter a lot
- Install Postman app and make a workspace/collection > test API calls
- Write logic to handle GET, POST, PATCH, DELETE API Calls and test them on Postman
- Explore routing and use of ?, +, (), * in the routes
- Use of regex in routes /a/, /.*fly$/
- Reading the query params in the routes
- Reading the dynamic routes

## Middleware & Request Handling

- Multiple Route Handlers - Play with the code
- Understanding next()
- next function and errors along with res.send()
- app.use("/route", rH, [rH2, rH3], rH4, rh5);
- What is a Middleware
- How Express.js basically handles requests behind the scenes
- Difference between app.use and app.all
- Write a dummy auth middleware for admin
- Write a dummy auth middleware for all user routes, except /user/login
- Error Handling using app.use("/", (err, req, res, next) => {});

## Database Integration

- Create a free cluster on MongoDB Atlas
- Install mongoose library
- Connect your application to the Database using connection URL
- Call the connectDB function and connect to the Database before starting application
- Create a userSchema & User Model
- Create a POST /signup API to add data to database
- Push some documents using API calls from Postman
- Error Handling using try, catch

## Data Handling

- JS object vs JSON (difference)
- Add the express.json middleware to your app
- Make your /signup API dynamic to receive data from the end user
- User.findOne with duplicate email ids, which object returned
- API - get user by email
- API - Feed API - GET /feed - get all the users from the database
- API - get user by id
- Create a delete user API
- Difference between PUT and PATCH
- API - Update a user
- Explore the Mongoose Documentation for Models methods
- What are the options in a Model.findOneAndUpdate method, explore more about it
- API - Update the user with email ID

## Schema Validation

- Explore schematype options from the documentation
- Add required, unique, lowercase, min, minLength, trim
- Add default values
- Create a custom validate function for gender
- Improve the DB schema - Add all appropriate validations on each field in Schema
- Add timestamps to the userSchema
- Add API level validation on PATCH request & signup post api
- Data sanitizing - Add API validation for each field
- Install validator
- Explore validator library function and use validator function for password, email, & url
- **NEVER TRUST req.body**

## Authentication & Security

- Validate data in signup API
- Install bcrypt package
- Create PasswordHash using bcrypt.hash & save the user with encrypted password
- Create login API
- Compare passwords and throw error if email or password is invalid
- Install cookie-parser
- Send a cookie to user
- Create GET /profile API and check if you get the cookie back
- Install jsonwebtoken
- In login API, after email and password validation, create a JWT token and send it to user in cookies
- Read the cookies inside your profile API and find the logged in user
- Create userAuth middleware
- Add the userAuth middleware in profile API and a sendConnectionRequest API
- Set the expiry of JWT token and cookies to 7 days
- Create userSchema method to getJWT()
- Create userSchema method to comparePasswords(passwordInputByUser)

## API Structure & Organization

- Explore similar platform APIs for inspiration
- Create a list of all APIs you can think of for your application
- Group multiple routes under respective routes
- Create authRoute, profileRouter, requestRouter
- Import these routers in app.js
- Create POST /logout
- Create PATCH /profile/edit
- Create PATCH /profile/password
- Validate all data in every POST, PATCH APIs

## Connection Request System

- Create Connection Request Schema
- Send Connection Request API
- Proper validation of Data
- **Think about ALL corner cases**
- $or query $and query in mongoose - https://www.mongodb.com/docs/manual/reference/operator/query-logical/
- schema.pre("save") function
- Read more about indexes in MongoDB
- Why do we need index in DB?
- What are the advantages and disadvantages of creating indexes?
- Read about compound indexes - https://www.mongodb.com/docs/manual/core/indexes/index-types/index-compound/
- **ALWAYS THINK ABOUT ALL CORNER CASES**

## Advanced Features

- Write code with proper validation for POST /request/review/:status/:requestId
- Thought process - POST vs GET
- Read about ref and populate - https://mongoosejs.com/docs/api/document.html#Document.prototype.populate()
- Create GET /user/requests/received with all the checks
- Create GET /user/connections

## Feed Logic & Pagination

- Logic for GET /feed API
- Explore the $nin, $and, $ne, and other comparison operators
- **Pagination implementation:**

```
/feed?page=1&limit=10 => 1-10 => .skip(0) & .limit(10)
/feed?page=2&limit=10 => 11-20 => .skip(10) & .limit(10)
/feed?page=3&limit=10 => 21-30 => .skip(20) & .limit(10)

Formula: skip = (page - 1) * limit
```

---

## Key Concepts to Remember

### Security Best Practices
- Never trust req.body - always validate
- Hash passwords with bcrypt
- Use JWT for authentication
- Set HTTP-only cookies for tokens
- Validate all user inputs
- Handle errors properly

### Database Best Practices
- Use indexes for frequently queried fields
- Think about all corner cases
- Use compound indexes when needed
- Implement proper error handling
- Use timestamps for tracking

### Code Organization
- Group related routes together
- Use middleware for common logic
- Separate concerns (routes, controllers, models)
- Keep code DRY (Don't Repeat Yourself)

### API Design
- Use proper HTTP methods (GET, POST, PATCH, DELETE)
- Implement pagination for large datasets
- Return appropriate status codes
- Provide clear error messages
- Document your APIs

---

## Useful Resources

- [MongoDB Query Operators](https://www.mongodb.com/docs/manual/reference/operator/query-logical/)
- [Mongoose Populate](https://mongoosejs.com/docs/api/document.html#Document.prototype.populate())
- [MongoDB Compound Indexes](https://www.mongodb.com/docs/manual/core/indexes/index-types/index-compound/)
- [Express.js Documentation](https://expressjs.com/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)

---

## Learning Checklist

- [ ] Understand Express.js routing
- [ ] Master middleware concepts
- [ ] Learn MongoDB and Mongoose
- [ ] Implement JWT authentication
- [ ] Practice data validation
- [ ] Understand database indexing
- [ ] Handle errors properly
- [ ] Implement pagination
- [ ] Secure your APIs
- [ ] Organize code properly

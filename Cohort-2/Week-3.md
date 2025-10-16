## fetch
- The `fetch()` method is used to make **HTTP requests** to servers.
- Returns a **Promise** that resolves to the `Response` object representing the response to the request. e.g.
  ![[Pasted image 20250709122914.png]]
- By default `fetch()` sends the HTTP GET request, if you want to send request with other HTTP method then you need to specify it
  ![[Pasted image 20250709123012.png]]
  e.g. 
  ![[Pasted image 20250709122703.png]]

## Authentication
Before get into Authentication, let's understand some jargons
1. Hashing
2. Encryption
3. Jason Web Tokens
4. Local Storage
##### 1. Hashing
- A one-way process to convert data (like passwords) into a fixed-length string.
- **Irreversible**: Original data cannot be retrieved from the hash.
- Commonly used for storing passwords securely.
- Changing the input a little bit changes the output by a lot.
- Examples: SHA-256, bcrypt.
##### 2. Encryption
- A two-way process where data is converted into a secure format.
- **Reversible** with the correct key.
- Used for protecting data during transmission (e.g., HTTPS).
- Types: Symmetric (same key for encryption & decryption) and Asymmetric (public/private keys).
##### 3. JSON Web Tokens (JWT)
- A compact, self-contained way to represent information securely between parties as a JSON object.
- Used for **stateless authentication**.
- It takes JSON as an input and give Token (long string) as an output. Also, whoever has the output can see the input i.e. it is not hashed or encrypted or protected. It is just converting JSON object to a long string. So anyone can revert-back that string to original object. You can get bunch of JWT decoder online.
- Token/string consists of three parts: **Header, Payload, Signature**.
- Anyone can revert-back the token string to original object but, only original servers can do the verification of that Token.
- Can be stored on the client side (e.g., in local storage).
- JSON Web Token is a specification like ECMAscript, its various implementations are present in various libraries. (e.g. jsonwebtoken on npm)
![[Pasted image 20250708115558.png]]
##### 4. Local Storage
- Web storage that allows storing key-value pairs in a user's browser.
- **Persists even after the browser is closed** (unlike sessionStorage).
- Useful for saving tokens or user preferences.
- Not secure for sensitive data (e.g., passwords or raw JWTs).
#### Assignment
![[Pasted image 20250708120350.png]]
###### Answer
`const express = require("express");`
`const jwt = require("jsonwebtoken");`

`const jwtPassword = "123456";`
`const app = express();`
`app.use(express.json());`

`const ALL_USERS = [`
  `{`
    `username: "harkirat@gmail.com",`
    `password: "123",`
    `name: "harkirat singh",`
  `},`
  `{`
    `username: "raman@gmail.com",`
    `password: "123321",`
    `name: "Raman singh",`
  `},`
  `{`
    `username: "priya@gmail.com",`
    `password: "123321",`
    `name: "Priya kumari",`
  `},`
`];`

`function userExists(username, password) {`
  `const userExists = false;`
  `for(let i=0 ; i<ALL_USERS.length ; i++){`
    `if(ALL_USERS[i].username == username && ALL_USERS[i].password == password){`
      `userExists = true;`
    `}`
  `}`
  `return userExists;`
`}`

`app.post("/signin", function (req, res) {`
  `const username = req.body.username;`
  `const password = req.body.password;`
  `if (!userExists(username, password)) {`
    `return res.status(403).json({`
      `msg: "User doesnt exist in our in memory db",`
    `});`
  `}`
  `var token = jwt.sign({ username: username }, "shhhhh");`
  `return res.json({`
    `token,`
  `});`
`});`

`app.get("/users", function (req, res) {`
  `const token = req.headers.authorization;`
  `try {`
    `const decoded = jwt.verify(token, jwtPassword);`
    `const username = decoded.username;`
    `res.json({`
      `users: ALL_USERS`
    `});`
  `} catch (err) {`
    `return res.status(403).json({`
      `msg: "Invalid token",`
    `});`
  `}`
`});`
`app.listen(3000)`
## Database (MongoDB)
- We use **Mongoose** to connect *MongoDB* database.
  `const mongoose = require("mongoose")`
  `mongoose.connect("url_to_database");`
![[Pasted image 20250719162507.png]]
- if database of given name is not present then it will get created and if its present then *mongoose* will simply gets conneted to the database.
- Before storing data to database we need to specify the model of the data, and then store it.
  ![[Pasted image 20250719154557.png]]
## Middlewares

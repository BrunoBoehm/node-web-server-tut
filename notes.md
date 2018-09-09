## S5L41 Basic server Setup
`npm init` creates the `package.json` and we can `npm install express --save` to get our copy of express.
We create a `server.js` file.
```js
const express = require('express');

var app = express();

app.get('/', (req, res) => {
    res.send('<h1>Hello Express!</h1>');
});

// app.get('/about', (req, res) => {
//     res.send('About Page');
// });

// app.get('/bad', (req, res) => {
//     res.send({
//         errorMessage: "Unable to handle"
//     });
// });

app.listen(3000);
```
We can start the server with terminal cmd `nodemon server.js`.

## S5L42 Static Web Server
Middleware is like a 3rd party add on: `app.use()`.
We can create a new html page in ou `/public` folder an then tell the middleware to check it out
```js
app.use(express.static(__dirname + "/public"));
```

We can now visit http://localhost:3000/help.html
Note we can add a callback to our `app.listen()` function.
```js
app.listen(3000, () => {
    console.log("Server is on, port 3000");
});
```
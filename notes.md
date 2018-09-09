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
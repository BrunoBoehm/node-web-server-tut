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

## S5L43 Rendering dynamic templates with data
A templating engine will help us to do what we do with ruby or php.
We will use `handlebar` with the handlebar view engine for express: https://npmjs.com/package/hbs `npm install hbs@4.0.0 --save`

Configuration is simple:
```js
const express = require('express');
const hbs = require('hbs');

var app = express();

app.set('view engine', 'hbs');
...
```
We can now make a directory called views and create our template base
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>About Page</title>
    </head>
    <body>
        <h1>About Page</h1>
        <p>This is the About page</p>

        <footer>
            <p>Copyright 2018</p>
        </footer>
    </body>
</html>
```

And we change our route to `render` our template
```js
app.get('/about', (req, res) => {
    res.render('about.hbs');
});
```

We can improve this by using variables.
We pass variables in the route like so
```js
app.get('/about', (req, res) => {
    res.render('about.hbs', {
        pageTitle: "About Page",
        currentYear: new Date().getFullYear()
    });
});
```

And use them with `{{  }}`
```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>{{pageTitle}}</title>
    </head>
    <body>
        <h1>About Page</h1>
        <p>This is the {{pageTitle}}</p>

        <footer>
            <p>Copyright {{currentYear}}</p>
        </footer>
    </body>
</html>
```

## S5L44 Handlebar Partials
We need to register our partials in our `server.js`.
```js
hbs.registerPartials(__dirname + '/views/partials');
```

We create a `/views/partials` folder and a `footer.hbs` template
The partial then can be injected with `{{> footer}}`.

Handlebar helpers can be registered 
```js
hbs.registerHelper('getCurrentYear', () => {
    return new Date().getFullYear()
})
```
and used like `{{getCurrentYear}}` in any template.

Note it might be useful to tell nodemon to watch handlebar files `nodemon server.js -e js, hbs`.
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


## S5L45 Express middleware
We can expand express, make changes to the response object, check if someone is logged in etc...

Middleware is simply
```js
app.use((req, res, next) => {
    // next();
});
```
If we comment out next() the page will never render since the middleware doesn't know what to do.

Let's create a log!
```js
app.use((req, res, next) => {
    var now = new Date().toString();
    var log = `${now}: ${req.method} ${req.url}`;
    console.log(log);
    fs.appendFile('server.log', log + '\n', (err) => {
        if (err) {
            console.log('Unable to append to server.log');
        }
    });
    next();
});
```

We can create a maintenance page
```js
app.use((req, res, next) => {
    res.render('maintenance.hbs');
});
```
We don't call `next()` so the `get()` handlers will not be touched.

Order is super important. If we try to access help.html we see it's still active.
Make sure that our public folder is called after...
```js
app.use(express.static(__dirname + "/public"));
```

## S5L48 Deploying to Heroku
Check the toolbelt is installed `heroku --help`.

We need to make a few changes to our server.
`env` in terminal tells your env variables.
We need a `const port = process.env.PORT || 3000;` if PORT is available (on heroku) it will be used, otherwise it will default to 3000.

We can use this const inside of our `listen` function.
```js
app.listen(port, () => {
    console.log("Server is on, port", port);
});
```

Last, we need to add a `start` script to our `package.json`. Heroku will run it.
```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
```

We commit and push to github.

`heroku create` will create a heroku remote.
We can now `git push heroku` and `heroku open`.
# How to use Tailwind with a Node/Express/EJS application

This repository will accompany an article and/or video on this topic.

First, some definitions:

**Tailwind**: An advanced, utility-first CSS framework for rapidly building custom user interfaces in web applications.

**Node**: A cross-platform, JavaScript runtime environment that executes JavaScript code outside a web browser, facilitating server-side scripting.

**Express**: A minimalistic, fast and unopinionated web application framework for Node.js, widely used for building APIs and web applications.

**EJS**: A simple templating language that lets you generate HTML markup with plain JavaScript, enhancing code readability and flexibility in web development.

---

## 1. Initialising the project

To get up and running with this project, run

```
npm install
```

If you were building this from scratch you'd initialise the project with:

```
npm init -y
```

and add the dependencies:

```
npm install express ejs tailwind
```

## 2. Setting up the folder structure

```
.
├── dist
│   └── styles
│       └── style.css // Where tailwind will build our css
├── package-lock.json
├── package.json
├── readme.md // This file
├── src
│   ├── index.js  // Where we'll create our server
│   ├── style.css // Where we will write our css
│   └── views
│       └── index.ejs // Our first page
```

We're gathering together all of the code we write in the `src/` directory and all of the code the Tailwind might write in the `dist/` directory. This separation of manually written and programatically generated code is a good practice.

## 3. Create the server

Here's our `index.js` file:

```js
const express = require('express');
const path = require('path');
const ejs = require("ejs")

const app = express();

// Here we're setting which directory the views are in:
app.set('views', path.join(__dirname, 'views')); 

// Here we're setting the view engine to be ejs
app.set('view engine', 'ejs');

// Here we're making the files stored in our dist directory available
app.use(express.static(path.join(__dirname, '..', 'dist')))

// Our home route
app.get('/', (req, res) => {
	// Which template will be rendered.
	res.render('index');
});

// And let our server listen for incoming connections
const server = app.listen(3000, () => {
	console.log(`The application started on port ${server.address().port}`);
});
```

The index.js file outlined here serves as the main entry point of an Express web application. The server is configured to use EJS as its view engine, enabling it to render dynamic HTML based on the specified EJS templates. The 'views' directory, where these templates are stored, is defined relative to the file's location. It also sets up the application to serve static files such as CSS, JavaScript, and images from a 'dist' directory. The application has a single route (/) that responds to GET requests by rendering the 'index' view. The server listens for incoming connections on port 3000, and logs a console message once it successfully starts up.

## 4. Write the template

```html
<!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" 
 />
    <link href="./styles/style.css" rel="stylesheet" />
    <title>Document</title>
  </head>
  <body>
    <h1 class="text-3xl font-bold underline bg-red-100">Hello world!</h1>
  </body>
 </html>
 ```

 The nice thing about templates in EJS is that we can write them in regular HTML. The benefit of course is that we can pass in variables from our routes and controllers into this view. This page doesn't do that.

 Notice that we are using Tailwind classes:
 - `text-3xl` is a pretty large font
 - `font-bold` is a bold font
 - `underline` is underling the text
 - `bg-red-100` is setting a light red background (something I use to check Tailwind is working)

 We can run our server now by running:

 ```
 node src/index.js
 ```

 or 

 ```
 npm run start
 ```

 but it will be unstyled. We haven't created the `style.css` or configured Tailwind yet.

Let's do that next.

## 5. Configure Tailwind

We'll install and initialise Tailwind:

```
npm install -D tailwindcss
npx tailwindcss init
```

and update our new `tailwind.config.js` file to know where to find our content:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/views/*.ejs",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Next, we'll create src/style.css and add the Tailwind directives:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

and now we start up the Tailwind CLI and allow it to build our CSS:

```
npx tailwindcss -i ./src/style.css -o ./dist/styles/style.css --watch
```

I've also added this as an npm script, so you can instead run

```
npm run style:watch
```

Now, as you add and update CSS classes in your templates, this tool will add the relevant CSS to your distribution file.

## Conclusion

At this point, you've a fully working Node.js application that uses Express, EJS and TailwindCSS. 

Feel free to use this project as a starter for any of your projects.
# How to use Tailwind with a Node/Express/EJS application

Welcome to this step-by-step guide, which also accompanies a related article and/or video. You will learn how to set up and use Tailwind, an advanced CSS framework, in a Node/Express/EJS web application. Here are brief descriptions of the main components:

- **Tailwind**: A utility-first CSS framework for rapidly creating custom user interfaces.
- **Node**: A cross-platform JavaScript runtime environment that allows server-side scripting.
- **Express**: A lightweight, fast, and flexible web application framework for Node.js.
- **EJS**: A templating language that helps generate HTML markup using plain JavaScript.

---

## 1. Initialising the project

To get started with this project, install the dependencies with the following command:

```
npm install
```

If you're creating a similar project from scratch, initialize the project using:

```
npm init -y
```

Then, add the necessary dependencies:

```
npm install express ejs tailwindcss
```

## 2. Setting up the folder structure

The project has the following folder structure:

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

The `src/` directory contains our source code, while the `dist/` directory hosts programmatically generated content like Tailwind CSS. Separating manual and generated code promotes cleaner and more organized codebase.

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

The `index.js` file is the main entry point of our Express application. It uses EJS as its view engine to render dynamic HTML content and serves static files from the 'dist' directory. The server listens on port 3000 and logs a confirmation message upon successful startup.

## 4. Write the template

We use EJS to create templates in HTML format, allowing us to inject variable data from routes and controllers dynamically. The `index.ejs` template in this project doesn't use any dynamic data:

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

 but note that the page will be unstyled as we have not configured Tailwind yet.

Let's do that next.

## 5. Configure Tailwind

Initialize Tailwind:

```
npx tailwindcss init
```

Then update the `tailwind.config.js` file to specify where Tailwind can find our content:

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

Create the `src/style.css` file and include Tailwind directives:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Finally, run the Tailwind CLI to watch for changes and build our CSS:

```
npx tailwindcss -i ./src/style.css -o ./dist/styles/style.css --watch
```

Or, use the predefined npm script:

```
npm run style:watch
```

As you modify CSS classes in your templates, Tailwind updates the `style.css` file in the distribution directory.

## Conclusion

Congratulations! You now have a working Node.js application that incorporates Express, EJS, and TailwindCSS. Feel free to use this project as a starting point for your

 web applications. Enjoy coding!
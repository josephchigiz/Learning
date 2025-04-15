# React

## Updating UI with JavaScript

Using JavaScript and DOM to add an H1  
Check index.html

The method used there is **imperative programming**, whereby we give step by step instructions on how the DOM should behave.  
This is very inefficient as we go into large projects.

A good alternative is **declarative programming**.  
In declarative, you give the order and let the compiler do the rest.

## Using React

You can use React in the project by loading two React scripts:

- react core library: <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
- react-dom: <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

## React used JSX

- **JSX** is a syntax extension for JS that allows the description of UI with a familar HTML-JS syntax.
- However, browsers do not understand JSX, so a **compiler** is needed. This is where **Babel** comes in: <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
- After including the Babel script, you also need to inform it of which code to compile by changing the script type to `type = text/jsx`

## Building UI With Components

### Components

User interfaces can be broken down into smaller chunks of code called **components**  
They allow you to build self-contained, reusable snippets of code.  
This is very convinien as you piece components together to come up with a page and when you need to update a section of the page, you just modify the relevant component.

As React components are written in JS, let's see how this is done from a JavaScript perspective.

#### Creating components

In React, components are functions.  
Move over to index3.html and create a function called `header` in your script tag

### Displaying Data with Props

So far, if we were to reuse the `<Header />` component, it would display the same content both times.  
_What if you want to pass different text or don't know the info ahead of time?_

Regular HTML elements have attributes such as `src` different pieces of info can be passed that change the behaviour of the element. In the same way, you can pass properties into React components. These are called `props`.  
Similar to a JavaScript function, we can design components that accept different arguments(props) that change the behaviour of the component or what is visible when rendered on the screen. Then you can pass these down from parent components to child components.

> In React, data flows down the component tree _(one-way data flow)_. _State_, can be passed from parent component to child as a prop.

#### Using props

Take a look at index3.html for an example.

A cool trick that isn't mentioned in that doc is using the ternary operator to have a default case to fall back to if a prop is not set:

```jsx
function Header({ title }) {
  return <h1> {title ? title : "Default title"} </h1>;
}

function HomePage() {
  return (
    <div>
      <Header />
    </div>
  );
}
```

#### Iterating through lists

As there several cases that come up that need data displayed as lists, using array methods help us to manipulate our data and generate UI elements that are similar but carry different information.
Let's add this array of names to our `HomePage` component:

```jsx
function HomePage() {
  const names = ["Joe", "Mary", "Winnie", "Daniele"];

  return (
    <div>
      <Header title="Eat.Pray.Love" />
      <ul>
        {names.map((name) => {
          <li>{name}</li>;
        })}
      </ul>
    </div>
  );
}
```

We've used `arru.map()` method to iterate over the array of names and an arrow function to map a name to a list item.

Notice how we use curly braces`{}` to weave in and out of JSX and JavaScript blocks.

Running this code however will return an error about a missing `key` prop.  
React needs something to uniquely identify the items in an array to be updated in the DOM.

For now we can use the names, since they're all unique, but it is recommended to use something like an item ID as it is guaranteed to be truly unique.

```jsx
function HomePage() {
  const names = ["Joe", "Mary", "Winnie", "Daniele"];

  return (
    <div>
      <Header title="Eat.Pray.Love" />
      <ul>
        {names.map((name) => {
          <li>
            key={name}>{name}
          </li>;
        })}
      </ul>
    </div>
  );
}
```

#### Displaying several DOM nodes for each list item

When each item needs to render not one but several DOM nodes, the short `<>...</>` **Fragment** syntax won't let us pass a key.  
In this case, we'll need to either group them into a single `<div/>` or use the slightly longer `<Fragment>` syntax:

```jsx
const names = [{id: 0, name: "Joe", like: "funny"}, {id: 1, name: "Mary", like: "dependable"}, {id: 2, name: "Winnie", like: "caring"}, {id: 3, name: "Daniele", like:"good-different"];

////////////////////

import {Fragment} from 'react';

const profiles = names.map(person =>
    <Fragment key={person.id}>
      <h1>{person.name}</h1>
      <p>{person.name} is very {person.like}</p>
    </Fragment>
);
```

## Adding interactivity with state

React helps us add interactivity with state and event handlers.  
To express this, let's add a like button inside our `HomePage` component. First, we add a button element inside the `return` statement *view index4.html*:  
```html

```
### Listening to events
To make the button do something when clicked, you can use the `onClick` event:

```html
<button onclick{}>Like</button>
```

In React, event names are **camelCased**. `onClick` is just one of the many events you can use to respond to user interaction.  
You can use `onChange` for input fields and `onSubmit` for forms.  

We can define a function to **"handle"** events when they are triggered. Let's create one called `handleClick`, just before the return statement:

```jsx
const app = document.getElementById("app");
			function HomePage() {
				const names = [
				{id: 0, name: "Joe", like: "funny"},
				{id: 1, name: "Mary", like: "dependable"},
				{id: 2, name: "Winnie", like: "caring"},
				{id: 3, name: "Daniele", like:"good-different"}
				];

				const [likes, setLikes] = React.useState(0);

				function handleClick() {
					console.log("increment like count");
				};

				return (
					<ul>
						{names.map(person =>
							<li key={person.id}>
								<h1>{person.name}</h1>
								<p>{person.name} is very {person.like}</p>
								<button onClick={handleClick}>Like</button>
							</li>
						)};
					</ul>
				)
			};
			const root = ReactDOM.createRoot(app);
			root.render(<HomePage/>);
)
```

## State and Hooks
React has a set of functions called **Hooks**. They allow you to add additional logic such as state to your component.  
State can be thought of as information in your UI that changes, triggered by user interaction.  

React uses a hook called `useState()` to manage state.  
Just add `useState` to your project, creating an array whose values you can access in your component through array destructuring.

```jsx
const [likes, handleClick] = React.useState();
```
The first item in the array is the state `value`, which you can name anything. Try to be descriptive:  

```jsx
const [likes] = React.useState();
```

The second item in the array is a function to `update` the value. You can name the update function anyhting but it's a common practice to start with `set` followed by the name of the state variable you're updating.  

```jsx
const [likes, setLikes] = React.useState();
```
You can also add the initial value of your `likes` to `0`:   

```jsx
const [likes, setLikes] = React.useState(0);
```

You can then use the state variable in your component to see if the initial state is working:  

```jsx
//...
function HomePage() {
  //...
  const [likes, setLikes] = React.useState(0);
  //...

  return (
    //...
    <button onClick={handleClick}>Like({likes})</button>
    //...
  );
}
```

Then you can call your state updater function, `setLikes` in your `HomePage` component. We can add it in the `handleClick` function we had defined earlier `index5.html`:

```jsx
//...
function HomePage() {
//...
  const [likes, setLikes] = React.useState(0);

  function handleClick() {
    setLikes(likes+1);
  };
  //...

  return (
    //...
    <button onClick={handleClick}>Like({likes})</button>
    //...
  );
}
```

Clicking the like button will now call the `handleClick` function which calls the `setLikes` state updater function with a single argument to the current number of likes +1.

**NB**: [https://nextjs.org/learn/react-foundations/updating-state#:~:text=Note%3A%20Unlike%20props,was%20initially%20created.](https://nextjs.org/learn/react-foundations/updating-state#:~:text=Note%3A%20Unlike%20props,was%20initially%20created.)

### Managing State
For more indepth info on state:  
1. [Adding interactivity](https://react.dev/learn/adding-interactivity)  
2. [Managing State](https://react.dev/learn/managing-state)  


## From React to Nextjs
Having explored what we can do with React, this is the final code:  

```html
<html>
  <body>
    <div id="app"></div>
 
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
 
    <script type="text/jsx">
      const app = document.getElementById("app")
 
      function Header({ title }) {
        return <h1>{title ? title : "Default title"}</h1>
      }
 
      function HomePage() {
        const names = ["Ada Lovelace", "Grace Hopper", "Margaret Hamilton"]
 
        const [likes, setLikes] = React.useState(0)
 
        function handleClick() {
          setLikes(likes + 1)
        }
 
        return (
          <div>
            <Header title="Develop. Preview. Ship." />
            <ul>
              {names.map((name) => (
                <li key={name}>{name}</li>
              ))}
            </ul>
 
            <button onClick={handleClick}>Like ({likes})</button>
          </div>
        )
      }
 
      const root = ReactDOM.createRoot(app);
      root.render(<HomePage />);
    </script>
  </body>
</html>
```

While React excels in building UI, there's a lot that it takes to build a fully functioning scalable application.

In the same directory cotaining our index.html, let's install `nextjs` locally alongside `react` and `react-dom` using npm:  

- First, let's create and empty file called `package.json` with an empty object `{}`:

```json
{}
```


- Then, in our terminal, we istall next and the others:  

```shell
npm install react@latest react-dom@latest next@latest
```  

Once the installation is done we should see the dependencies in the `package.json` file:

```json
{
  "dependencies": {
    "next": "^14.0.3",
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  }
}
```  

You will also notice a file called `package-lock.json` that contains detailed information about the exact versions of each package  

Now we can go to our index.html file and remove the html-related tags, react ad react-dom scripts, babel, document.getElementById() and ReactDom.createRoot() methods and the React part of React.useState() function.  

After all the junk is cleared, import `useState` as follows:  
```jsx
import { useState } from 'react';
```

...and since all the remaining file is stripped to js content, rename the file to `.js` or `.jsx`.

## Creating our first page

Nextjs uses file-system page routing, whereby instead of using code to define the app's routes, it used folders and files.  

- Create a new folder called `app` and move `index.jsx` to the folder.  
- Rename `index.jsx` to `page.jsx`. This makes it the main page of our application.  
- Add `export default` to the `<Homepage />` component so that Next can easily distinguish which component to render as the main component of the page:  

```jsx
//...
export default function HomePage() {...
// ...
```
### Running the development server
Next, so that we can see our new page while developing, add `next dev` script to our `package.json` file:  

```jsx
{
"scripts": {
  "dev": 'next dev'
},
"dependencies": {
  "next": "..."
}
}
```

With that set up, run `npm run dev` in the terminal.  

1. When we navigate to our localhost(with the given port) in the browser, we are met with an error:  

    The error comes up as a result of Next using React Server Components, a feature that allows React to render on the server.  
Since server components don't use `useState`, we'll need to use a client component instead.  

    We shall see the differences between Server and Client components later.  

  2. The file `layout.js` was automatically generated inside our `app` folder.  
  This file holds the general layout of your app. You can use it to hold UI elements shared across all pages(e.g navigation, footer)  

  Looking at the migration, we can see the benefits of Nextjs already:  
  - Removing the complicated configuaration of react and importing the Babel compiler.  
  - We created our first page.  


## Server and Client Components  
To be able to undestand the concept of Client/Server Components, we need to understand - *the environments our code can be executed in: **server** or **client*** and *the network boundary that seperates the two.  

### Server and Client Environments

In the context of web applications:  
- **The client** refers to the browser or user's device that sends a request to the server for the application code. It then receives info back from the server and transforms it to an interface for a user to interact with.  
- **The server** refers to the computer in a data center that stores our application code, receives requests from a client, does some computation and sends the appropriate response.  

Each env has its own set of capabilities and constraints. If you decide to move rendering and data fetching to the server side, less code is sent to your client hence improving your app's performance. However, as we learnt earlier, to make our UI interactive, we need to update the DOM on the client.  

This ends up in some differences in the code we write for the client and the server. Certain ops such as data fetching or managing user state are better suited for one env over the other.

### Network Boundary  
It is a conceptual line that seperates the environments.  
In React, you choose where to put the boundary in your component tree. 
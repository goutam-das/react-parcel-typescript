# react-parcel-typescript

React Setup using Typescript and Parcel
#  Setting up a basic build

After creating an empty directory to set up the project in, we need to run a command to create an npm project:

`npm init`

After following the instructions in this command, you'll have a package.json file with metadata.

# Essential packages

Now it's time to install the packages we'll end up using in the actual application. There are other packages we'll use as a developer, but that the end-user doesn't actually need.

`npm install react react-dom`  

The first package is React, of course, which happens to be our framework of choice. react-dom is necessary for rendering in the browser.

# Developer packages

Now we install all the tools we'll be using to develop.  

`npm install --save-dev parcel-bundler typescript @types/react @types/react-dom`

We use --save-dev to make sure that these won't end up in the code that we end up shipping in production.  

We can setup the typescript compiler with the following command:  

`npx tsc --init`  

We then need to add the following line:

`"jsx": "react"`  

in order to get the Typescript compiler to handle our React html correctly.

# Creating the basic frontend  

Now we need to create a basic UI to test out the packages we've installed:

> index.html   

`<!DOCTYPE html>`  
`<html lang="en">`  
`<head>`  
    <meta charset="UTF-8" />
    <meta name="generator" content="parceljs" />
  </head>
  <body>
    <div id="root"></div>
    <script src="./index.tsx"></script>
  </body>
</html>`

This file contains the basic HTML for our website. React will be filling in the rest, starting from the element with id root. We reference our main typescript file, which will be like this:

index.tsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';

ReactDOM.render(<App />, document.getElementById('root'));
Here we render our main App component in the element with id root. The component is defined like this:

components/App.tsx
import React, { FunctionComponent, useState } from "react";

`const App: FunctionComponent = () => {
  const [count, setCount] = useState(0);
  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </>
  );
};
export default App;`
Understanding everything going on here isn't important, all that matters is having a component with a bit of state (here, the current count) so we can see whether or not HMR works. This component is just a counter we can increment with a button.

Bundling
Now, we can bundle and serve the app by adding the following script to package.json:
"scripts": {
  "dev": "parcel index.html"
}
Now in order to start up a development server, we just run npm run dev. Parcel will magically figure out how to bundle everything based on the imports in the files themselves, without any configuration from us.

We can now navigate to our frontend at http://localhost:1234 in our browser (or whatever else parcel tells us).

We should see something like this:

Counter.

Now, if we change the following line in App.tsx to:
<p>The Count: {count}</p>
the entire page reloads, and the count is lost, before we see our change. This isn't ideal, because we might want to be styling or editing the logic for a specific state. Having to reset the state manually each time is a pain. This is where HMR is useful.

Setting up HMR
We first need to install a package for hot module reloading in React.
npm install --save-dev react-hot-loader
We only need this in development, hence --save-dev.

We now need to modify the root component of our app, in App.tsx:
import { hot } from 'react-hot-loader';
...
declare const module: any;
export default hot(module)(App);
This only needs to be done to the top-level component, as we add more components they can be written normally.

With this simple change, we now have HMR! In order to test this out, try incrementing the counter, and then changing the text in the component. The counter's state shouldn't change even as the text does. This is HMR in action.

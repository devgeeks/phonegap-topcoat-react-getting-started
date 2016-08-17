Our First PhoneGap Topcoat React App
====================================

This is a super-basic intro to creating a React app and having a play with the topcoat components we will be using in the new version of the developer app.

We will use the `phonegap-template-react-hot-reloader` to generate a basic starter app, add the `phonegap-react-topcoat` components package, then lay out some components on a "page".

Requirements
------------

1. Node 5+ and PhoneGap (obv)

2. Topcoat
	- http://topcoat.io/

3. Basic understanding of React (and I guess ES6, babel and webpack)
	- http://exploringjs.com/
	- https://babeljs.io/docs/learn-es2015/
	- https://www.lynda.com/JavaScript-tutorials/Up-Running-ECMAScript-6/424003-2.html
	- https://facebook.github.io/react/docs/getting-started.html
	- https://egghead.io/courses/react-fundamentals

4. The React Topcoat components (clone this somewhere)
	- https://github.com/devgeeks/phonegap-topcoat-react
	- For now, this needs to be installed below from a local clone that you have built:
		- `git clone https://github.com/devgeeks/phonegap-topcoat-react`
		- `cd phonegap-topcoat-react`
		- `npm install`
		- `npm run build`

Getting started
---------------

First we will need a fresh new app created using the PhoneGap React hot loader template
	
```
phonegap create myreactapp --template react-hot-loader
cd myreactapp
npm install
```

To preview our app in the browser (complete with hot reloading) just run `npm start` and open http://localhost:8080/ in a browser.


For now, the easiest way to get Topcoat into the project is to just add all of it to our index.js:

```
npm install topcoat --save
```

We will use Webpack and the css-loader to load the Topcoat CSS from the `node_modules` folder and add it to the head of our `index.html`.

In order to load CSS from `node_modules`, we will need to add it in the 'includes' path for the css-loader in`./webpack.config.js`

Change:

```
{
  test: /\.css$/,
  loaders: ['style', 'css?url=false'],
  include: PATHS.src,
},
```

To:

```
{
  test: /\.css$/,
  loaders: ['style', 'css?url=false'],
  includes: [ PATHS.src, PATHS.node_modules ],
},
```


Then, in `./src/index.js` add the following line after the first CSS import:

```
import 'topcoat/css/topcoat-mobile-light.css';
```

Restarting the hot reloading server (`npm start` again) will show that the Topcoat CSS has already changed the look of the app a little.

Next, we will add the `phonegap-topcoat-react` components:

```
npm install /path/to/phonegap-topcoat-react --save
```

The easiest way to see one of these components in action is to replace the "Say hello" button with a Topcoat Button component.

In `./src/components/Hello.js` we can start by importing the Button component:

```
import { Button } from 'phonegap-topcoat-react';
```

This will import just the `Button` component using destructuring assignment.

We can then use this `Button` instead of the `Tappable` currently on line 35.

```
<Tappable
  className="button-say-hello"
  onTap={ () => this.sayHello('Hello world') }
>Say hello</Tappable>
```

```
<Button
  clickHandler={ () => this.sayHello('Hello world') }
  cta
>Say hello</Button>
```

Notice we change the `onTap` to `clickHandler`? This is just because the `Button` API specifically sets the `onTap` of the resulting `Tappable` using the `clickHandler` prop. We also pass a new prop `cta` to tell our component that we want the Call To Action version of a Topcoat Button.

Lastly, we can use the fact that the `Button` component passes through any props it doesn't recognise. In this case, we will pass in a `style` object as a prop... to move the `Button` back where it belongs.

```
<Button
  clickHandler={ () => this.sayHello('Hello world') }
  cta
  style={
  	{
  		position: 'absolute',
  		bottom: '100px',
  		left: '50%',
  		transform: 'translate(-50%, 0)'
  	}
  }
>Say hello</Button>
```

Just for fun, have a look at the [Components Storybook](https://devgeeks.github.io/phonegap-topcoat-react) and try and add some other components:

```
import { List, ListContainer, ListHeader, ListItem } from 'phonegap-topcoat-react';
```

...etc


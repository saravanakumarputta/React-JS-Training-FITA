# React router setup in React Typescript project

## Install the below packages

`npm i -S react-router-dom`

## Install types package(Since the current react project is typescript)

`npm i -D @types/react-router-dom`

## Import the components from 'react-router-dom'

At the Application root file

```javascript
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";

const App = () => {
	return (
		<Router>
			<div>
				Header
				<Link to='/'>Home</Link>
				<Link to='/contact'>Contact</Link>
				<Link to='/about'>About</Link>
			</div>
			<div>
				Content
				<Switch>
					<Route exact path='/'>
						Home
					</Route>
					<Route exact path='/contact'>
						Contact
					</Route>
					<Route exact path='/avout'>
						About
					</Route>
				</Switch>
			</div>
		</Router>
	);
};
```

## Add history fallback option in webpack.config.js under devServer

`historyApiFallback: true`

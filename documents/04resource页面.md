> src/index.js

<br>
配置路由

	import React from 'react';
	import ReactDOM from 'react-dom';
	import { Provider } from 'react-redux';
	import { createStore, applyMiddleware } from 'redux';
	import { Router, Route, browserHistory } from 'react-router';
	
	import App from './components/app';
	import Resources from './components/resources';
	import reducers from './reducers';
	
	const createStoreWithMiddleware = applyMiddleware()(createStore);
	
	ReactDOM.render(
	  <Provider store={createStoreWithMiddleware(reducers)}>
	    <Router history={browserHistory}>
	        <Route path="/" component={App}>
	            <Route path="resources" component={Resources} />
	        </Route>
	    </Router>
	  </Provider>
	  , document.querySelector('.container'));

<br>

> src/components下创建resources.js

<br>

> src/components/resources.js

<br>

	import React from 'react';
	
	export default () => {
	    return (
	        <div>
	            Super Special Secret Recipe
	            <ul>
	                <li> 1 Cup Suger</li>
	                <li>1 Cup Pepper</li>
	                <li>1 Cup Salt</li>
	            </ul>
	        </div>
	    );
	};

<br>

> src/components/app.js

<br>

	import React, { Component } from 'react';
	import Header from './header';
	
	export default class App extends Component {
	  render() {
	    return (
	        <div>
	            <Header />
	            {this.props.children}
	        </div>    
	      
	    );
	  }
	}

<br>

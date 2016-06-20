Resource页面对应的组件已经创建好。

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

**如何让登录成功的人可以浏览该页面呢？**

<br>

而且，其它的页面可能也会有类似的需求。所以验证要放在一个全局单独的地方，这个单独的地方包含了验证逻辑。

<br>

到这，Higher Order Component就有了用武之地，它接受Resource这个组件，返回Resource这个组件，相当于在Resource组件的外层包裹了一层，但还增加了验证的逻辑。

<br>

> src/components下创建require_authentication.js

<br>

> src/componnets/require_authenticaiton.js

<br>

	import React, { Component } from 'react';
	import { connect } from 'react-redux';
	
	export default function(ComposedComponent){
	    class Authentication extends Component{
	        
	        
	        static contextTypes = {
	            router: React.PropTypes.object
	        }
	        
	        componentWillMount(){
	            if(!this.props.authenticated){
	                this.context.router.push('/');
	            }
	        }
	    
	        componentWillUpdate(nextProps) {
	          if (!nextProps.authenticated) {
	            this.context.router.push('/');
	          }
	        }
	        
	        render(){
	            //console.log('Rendering', ComposedComponent);
	            //console.log(this.props.authenticated);
	            //console.log(this.content)
	            return <ComposedComponent {...this.props} />
	        }
	    }
	    
	    function mapStateToProps(state){
	        return { authenticated: state.authenticated}
	    }
	    
	    return connect(mapStateToProps)(Authentication);
	}

<br>

在路由设置中，需要验证的组件，就可以使用上面的HOC。

<br>

> src/index.js

<br>

	import React from 'react';
	import ReactDOM from 'react-dom';
	import { Provider } from 'react-redux';
	import { createStore, applyMiddleware } from 'redux';
	import { Router, Route, browserHistory } from 'react-router';
	
	import requireAuth from './components/require_authentication';
	import App from './components/app';
	import Resources from './components/resources';
	import reducers from './reducers';
	
	const createStoreWithMiddleware = applyMiddleware()(createStore);
	
	ReactDOM.render(
	  <Provider store={createStoreWithMiddleware(reducers)}>
	    <Router history={browserHistory}>
	        <Route path="/" component={App}>
	            <Route path="resources" component={requireAuth(Resources)} />
	        </Route>
	    </Router>
	  </Provider>
	  , document.querySelector('.container'));

<br>


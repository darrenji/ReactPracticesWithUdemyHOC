首先把所有action.type的类型放在一个文件中。

<br>

> src/actions下创建types.js

<br>

> src/actions/types.js

<br>

	export const CHANGE_AUTH = 'change_auth';

<br>

> src/actions/index.js

<br>

	import {
	    CHANGE_AUTH
	} from './types';
	
	export function authenticate(isLoggedIn){
	    return {
	      type: CHANGE_AUTH,
	      payload: isLoggedIn
	    };
	}

<br>

action返回的Promise结果给到reducer.

<br>

> src/reducers下创建authentication.js

<br>

> src/reducers/authentication.js

<br>

	import {
	    CHANGE_AUTH
	} from '../actions/types';
	
	export default function(state = false, action){
	    switch(action.type){
	        case CHANGE_AUTH:
	            return action.payload;
	    }
	    
	    return state;
	}

<br>

所有的reducer需要到combineReducer中去注册。

<br>

> src/reducers/index.js

<br>

	import { combineReducers } from 'redux';
	import authenticationReducer from './authentication';
	
	const rootReducer = combineReducers({
	  authenticated: authenticationReducer
	});
	
	export default rootReducer;

<br>





---
title: react dva.js路由与页面跳转方法
tags: [react,dva,react-router,dva-router]
date: 2018-05-29 09:00:46
categories: react
---

## 前言

<!--more-->

## 正文

一、 link 跳转

```JS
import { Link } from 'dva/router';
<Link to={
     { 
         pathname:"/ant", 
         query:{foo: 'foo', boo:'boo'},  
         state:{data:'hello'}   
     } 
} >
```

二、点击路由跳转 在effects 里面使用yield put

```JS
<h2 onClick={testClick}>test</h2>
const testClick = () => {
 dispatch({
    type: 'test/redirect',
  });
}
effects: {
    // 路由跳转
    * redirect ({ payload }, { put }) {
      yield put(routerRedux.push('/products', {name: 'dkvirus'}));
    },
},
```




effects 有三个参数:
Effects

put 用于触发 action 。
```JS
yield put({ type: 'todos/add', payload: 'Learn Dva' });
```
call 用于调用异步逻辑，支持 promise 。
```JS
const result = yield call(fetch, '/todos');
```
select 用于从 state 里获取数据。
```JS
const todos = yield select(state => state.todos);
```
基于 action 进行页面跳转
```JS
import { routerRedux } from 'dva/router';

// Inside Effects
yield put(routerRedux.push('/logout'));

// Outside Effects
dispatch(routerRedux.push('/logout'));

// With query
routerRedux.push({
  pathname: '/logout',
  query: {
    page: 2,
  },
});
```
除 push(location) 外还有更多方法，详见这里

示例如下：
```JS
  state: {
    isLogin: false,
    loginfail:false,
  },
  subscriptions: {
    setup({ dispatch, history }) {
      history.listen(location => {
        if (location.pathname.includes('app')) {
          dispatch({
            type: 'loginhook',
          });
        }
      });
    },
  },
    effects: {
    * login({ payload },{call, put}) {
      const { data } = yield call(login, payload);
      if (data && data.success) {    
        yield put({
                  type: 'checklogin',
                  payload:{
                    isLogin:true,
                  }
              });
        yield put(routerRedux.push('/app/users'));
      }else{
        yield put({
                    type: 'loginfail',
                    payload:{
                      loginfail:true,
                    }
                });
      }
    },

    * loginhook({ payload },{select,call, put}){
      const isLogin = yield select(({session}) => session.isLogin);
      console.log('logincheck',isLogin);
      if(isLogin === false){
        yield put((routerRedux.push('/login')));
      }

    },
  },
  reducers: {

    checklogin(state,action) {
      return {...state,isLogin:action.payload.isLogin };
    },

    loginfail(state,action) {
      return {...state, loginfail:action.payload.loginfail};
    },
  }
```



## 小结

## 关于作者
** 珠峰
WEB开发与管理相结合，注重技术与应用结合。现居上海。 

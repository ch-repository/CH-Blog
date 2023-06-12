---
layout: react
title: Hooks详解
date: 2023-06-12 15:41:18
tags:
 - React
categories: coding
comments: false
---

### useState

```js
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

useState会返回一个数组，一个state，一个用于更新state的函数
useState唯一的参数就是初始state，在初始化渲染期间，返回的状态与传入的第一个值相同
#### 注意事项
- React假设当你多次调用useState的时候，你能保证每次渲染时它们的调用顺序是不变的。因为react是根据每个useState定义时的顺序来确定你在更新state值时更新的是哪个state
- 你可以在事件处理函数中或其他一些地方调用这个函数。它类似class组件的this.setState，但是它不会把新的state和旧的state进行合并，而是直接替换
- initialState参数只会在组件的初始化渲染中起作用，后续渲染时会被忽略
- Hook内部使用Object.is来比较新/旧state是否相等，与class组件中的setState方法不同，如果你修改状态的时候，传的状态值没有变化，则不重新渲染
- useState不能直接存储函数或函数组件，他会调用该函数并且将函数的返回值作为最终state值进行存储或更新。如果必须这么做可以作为一个数组的元素或对象的某个属性进行存储
- useState没有设置类似class组件中的setState回调函数来拿到最新state然后进行后续操作，可以通过useEffect并设置相应依赖来实现。因为useEffect就是在渲染完成后调用的

#### useState在异步操作中的状态不同步的问题
函数的运行时独立的，每个函数都有一份独立的作用域。函数的变量时保存在运行时的作用域里面。当我们有一部操作的时候，经常会碰到异步回调的变量引用是之前的，也就是旧的（这里也可以理解成闭包）。比如下面的一个例子：
```js
import React, { useState } from "react";
​
const Counter = () => {
  const [counter, setCounter] = useState(0);
​
  const onAlertButtonClick = () => {
    setTimeout(() => {
      alert("Value: " + counter);
    }, 3000);
  };
​
  return (
    <div>
      <p>You clicked {counter} times.</p>
      <button onClick={() => setCounter(counter + 1)}>Click me</button>
      <button onClick={onAlertButtonClick}>
        Show me the value in 3 seconds
      </button>
    </div>
  );
};
​
export default Counter;
```

当你点击Show me the value in 3 secondes后，紧接着点击Click me使得counter的值从0变成1.三秒后，定时器触发，但alert出来的是0（旧值），但我们希望的结果是当前的状态1

这个问题在class component不会出现，因为class component的属性和方法都存在一个instance上，调用方式是:this.state.xxx和this.method()。因为每次都是从一个不变的instance上进行取值，所以不存在引用是旧的问题。

除了setTimout这种异步，还有类似事件监听函数（比如滚动监听的回调函数）中访问State也会是旧的

这个问题目前的普遍解决方案是使用useRef


### useEffect
什么是react中的副作用操作？
> 指那些没有发生在数据向视图转换过程中的逻辑，如ajax请求、访问原生dom元素、本地持久化缓存、绑定/解绑事件、添加订阅、设置定时器、记录日志等。

副作用操作可以分两类：需要清除的（例如事件绑定/解绑）和不需要清除的。

原先在函数组件内（这里指在react渲染阶段）改变dom、发送ajax请求以及执行其他包含副作用的操作都是不被允许的，因为这可能会产生莫名其妙的bug并破坏UI的一致性

一个需求实现：需要实时让document.title显示你最新的点击次数（count）

class组件实现：
```js
class Counter extends React.Component{
    state = {number:0};
    add = ()=>{
        this.setState({number:this.state.number+1});
    };
    componentDidMount(){
        this.changeTitle();
    }
    componentDidUpdate(){
        this.changeTitle();
    }
    changeTitle = ()=>{
        document.title = `你已经点击了${this.state.number}次`;
    };
    render(){
        return (
            <>
              <p>{this.state.number}</p>
              <button onClick={this.add}>+</button>
            </>
        )
    }
}
```

因为需要实时让document.title显示你最新的点击次数(count)，所以就必须在componentDidMount或componentDidUpdate中编写重复的代码来重新设置document.title

hooks实现：
```js
import React,{Component,useState,useEffect} from 'react';
import ReactDOM from 'react-dom';
function Counter(){
    const [number,setNumber] = useState(0);
    // useEffect里面的这个函数会在第一次渲染之后和更新完成后执行
    // 相当于 componentDidMount 和 componentDidUpdate:
    useEffect(() => {
        document.title = `你点击了${number}次`;
    });
    return (
        <>
            <p>{number}</p>
            <button onClick={()=>setNumber(number+1)}>+</button>
        </>
    )
}
ReactDOM.render(<Counter />, document.getElementById('root'));
```

useEffect就是一个Effect Hook，给函数组件增加了操作副作用的能力。它跟class组件中的componentDidMount、componentDidUpdate和ComponentWillUnmount具有相同的用途，只不过被合并成了一个API

与componentDidMount或componentDidUpdate不同，使用useEffect调度的effect不会阻塞浏览器更新屏幕，这让你的应用看起来相应更快。大多数情况下，effect不需要同步地执行。在个别情况下（例如测量布局），有单独的useLayoutEffect Hook供你使用，其API与useEffect相同

#### 清除副作用
```js
function Counter(){
    let [number,setNumber] = useState(0);
    let [text,setText] = useState('');
    // 相当于componentDidMount 和 componentDidUpdate
    useEffect(()=>{
        console.log('开启一个新的定时器')
        let $timer = setInterval(()=>{
            setNumber(number=>number+1);
        },1000);
        // useEffect 如果返回一个函数的话，该函数会在组件卸载和更新时调用
        // useEffect 在执行副作用函数之前，会先调用上一次返回的函数
        // 如果要清除副作用，要么返回一个清除副作用的函数
       return ()=>{
            console.log('destroy effect');
            clearInterval($timer);
        } 
    });
    // },[]);//要么在这里传入一个空的依赖项数组，这样就不会去重复执行
    return (
        <>
          <input value={text} onChange={(event)=>setText(event.target.value)}/>
          <p>{number}</p>
          <button>+</button>
        </>
    )
}
```

useEffect接收一个函数，该函数会在组件渲染到屏幕之后才执行，该函数有要求：要么返回一个能清除副作用的函数，要么旧不返回任何内容

默认情况下，useEffect在第一次渲染之后和每次更新之后都会执行。而useEffect接收的函数参数所返回的清除副作用的函数则会在组件更新和卸载前执行，然后更新后执行useEffect接收的函数。然后等待下一次组件更新或卸载，执行副作用的函数...如此循环往复

#### 跳过effect进行性能优化
默认情况下，useEffect在每次更新之后都会执行

有时候，我们只想useEffect只在组件挂在时执行，然后卸载时执行清除副作用函数，不想更新时也执行useEffect（比如原生事件的绑定/解绑）或者只想让执行state发生更新时才执行useEffect（如果某些state更新后拿到最新state进行后续操作）

此时，你可以通知react跳过对effect的调用，只要传递数组作为useEffect的第二个可选参数即可

- 如果想执行只运行一次的effect（仅在组件挂在和卸载时执行），可以传递一个空数组作为第二个参数。这就告诉react你的effect不依赖props或state中的任何值，所以它永远都不需要重复执行
- 如果指定state发生更新时才执行useEffect，可以传递一个包含执行state元素的数组作为第二个参数

```js
function Counter(){
    let [number,setNumber] = useState(0);
    let [text,setText] = useState('');
    // 相当于componentDidMount 和 componentDidUpdate
    useEffect(()=>{
        console.log('useEffect');
        let $timer = setInterval(()=>{
            setNumber(number=>number+1);
        },1000);
    },[text]);// 数组表示 effect 依赖的变量，只有当这个变量发生改变之后才会重新执行 efffect 函数
    return (
        <>
          <input value={text} onChange={(event)=>setText(event.target.value)}/>
          <p>{number}</p>
          <button>+</button>
        </>
    )
}
```

> 注意：无论你是否制定了useEffct的第二个参数，useEffect永远都会在组件挂载时执行一次

#### 使用多个useEffect
useEffect可以声明多个，react将按照effect声明的顺序依次调用组件中的每一个effect

我们可以根据具体副作用操作的性质分类将不同种类的操作放到多个useEffect中
```js
// Hooks 版
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
	//dom操作相关的effect
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
	//订阅/取消订阅的相关effect
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
  // ...
}
```

#### useEffect不能接收async作为回调函数
在useEffect中发起异步请求是很常见的场景，由于异步请求通常都是封装好的异步方法，所以新手很容易写成下面这样
```js
function App() {
  const [data, setData] = useState({ hits: [] });
  // 注意 async 的位置
  // 这种写法，虽然可以运行，但是会发出警告
  // 每个带有 async 修饰的函数都返回一个隐含的 promise
  // 但是 useEffect 函数有要求：要么返回清除副作用函数，要么就不返回任何内容
  useEffect(async () => {
    const result = await axios(
      'https://hn.algolia.com/api/v1/search?query=redux',
    );
    setData(result.data);
  }, []);
  return (
    <ul>
      {data.hits.map(item => (
        <li key={item.objectID}>
          <a href={item.url}>{item.title}</a>
        </li>
      ))}
    </ul>
  );
}
```

更优雅的写法：
```js
function App() {
  const [data, setData] = useState({ hits: [] });
  useEffect(() => {
    // 更优雅的方式
    //将异步请求封成一个独立async函数然后在useEffect中调用
    const fetchData = async () => {
      const result = await axios(
        'https://hn.algolia.com/api/v1/search?query=redux',
      );
      setData(result.data);
    };
    fetchData();
  }, []);
  return (
    <ul>
      {data.hits.map(item => (
        <li key={item.objectID}>
          <a href={item.url}>{item.title}</a>
        </li>
      ))}
    </ul>
  );
}
```

### useLayoutEffect
与useEffect的区别

- useEffect是异步非阻塞调用，useLayoutEffect是同步阻塞调用
- useEffect在浏览器绘制后调用（即Renderer commit阶段结束后），useLayoutEffect在DOM变更（React的更新）后，浏览器绘制前完成所有操作（即Renderer commit阶段的Layout子阶段同步执行）

使用场景：
> 大部分情况下使用useEffect即可。针对小部分特殊情况如短时间内出发了多次状态更新导致渲染多次以致屏幕闪烁的情况，使用useLayoutEffect会在浏览器渲染之前同步更新DOM数据，哪怕是多次的操作，也会在渲染前一次性处理完，再交给浏览器绘制。这样不会导致闪屏现象发生。


### useReducer
useReducer和redux中的reducer很像。useState内部就是靠useReducer来实现的

useReducer接收两个参数：reducer函数含（preState和action两个参数）和初始化的state
```js
    // 官方 useReducer Demo
    // 第一个参数：应用的初始化
    const initialState = {count: 0};

    // 第二个参数：state的reducer处理函数
    function reducer(state, action) {
        switch (action.type) {
            case 'increment':
              return {count: state.count + 1};
            case 'decrement':
               return {count: state.count - 1};
            default:
                throw new Error();
        }
    }

    function Counter() {
        // 返回值：最新的state和dispatch函数
        const [state, dispatch] = useReducer(reducer, initialState);
        return (
            <>
                // useReducer会根据dispatch的action，返回最终的state，并触发rerender
                Count: {state.count}
                // dispatch 用来接收一个 action参数「reducer中的action」，用来触发reducer函数，更新最新的状态
                <button onClick={() => dispatch({type: 'increment'})}>+</button>
                <button onClick={() => dispatch({type: 'decrement'})}>-</button>
            </>
        );
    }
```

选择useReducer还是useState

- 如果你的页面state很简单，可以直接使用useState
- 如果你的页面state比较复杂（state是一个对象或者state非常多散落在各处）请使用useReducer
- 对于复杂的state操作逻辑（比如某项操作同时操作或更新多个state值），嵌套的state的对象，推荐使用useReducer
- 如果你的页面组件层级比较深，并且需要子组件触发state的变化，可以考虑useReducer+useContext


### useContext
接收一个context对象（React.createContext的返回值）并返回context的当前值。当前的context值由上层组件中距离当前组件最近的<MyContext.Provider>的value prop决定

当组件上层最近的<MyContext.Provider>更新时，该Hook会触发重渲染，并使用最新传递给MyContext provider的context value值

useContext(MyContext)相当于class组件中的static contextType = MyContext或者<MyContext.Consumer>

```js
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

使用useContext和useReducer模拟实现简易Redux

Provider组件
```js
import React, { useReducer } from 'react'
import './App.css'
import ComponentA from './components/ComponentA'
import ComponentB from './components/ComponentB'
import ComponentC from './components/ComponentC'
const initialState = 0
const reducer = (state, action) => {
    switch (action) {
        case 'increment':
            return state + 1
        case 'decrement':
            return state - 1
        case 'reset':
            return initialState
        default:
            return state
    }
}

export const CountContext = React.createContext()

function App() {
    const [count, dispatch] = useReducer(reducer, initialState)
    return (
        <CountContext.Provider
            value={{ countState: count, countDispatch: dispatch }}
        >
            <div className="App">
                {count}
                <ComponentA />
                <ComponentB />
                                <ComponentC />
            </div>
        </CountContext.Provider>
    )
}

export default App    
```

Component A:
```js
//组件A
function ComponentA() {
  const countContext = useContext(CountContext)
  return (
    <div>
      Component A {countContext.countState}
      <button onClick={() => countContext.countDispatch('increment')}>Increment</button>
            <button onClick={() => countContext.countDispatch('decrement')}>Decrement</button>
            <button onClick={() => countContext.countDispatch('reset')}>Reset</button>
    </div>
  )
}
```

Component B:
```js
function ComponentB() {
  const countContext = useContext(CountContext)
  return (
    <div>
      Component B {countContext.countState}
      <button onClick={() => countContext.countDispatch('increment')}>Increment</button>
            <button onClick={() => countContext.countDispatch('decrement')}>Decrement</button>
            <button onClick={() => countContext.countDispatch('reset')}>Reset</button>
    </div>
  )
}
```

### useRef
useRef返回一个可变的ref对象，其.current属性被初始化入被传入的参数（initialValue）。返回的ref对象在组件的整个生命周期内不变

> useRef返回的ref对象在组件的整个生命周期内保持不变，也就是说每次重新渲染函数组件时，返回的ref对象都是同一个。
> 也就是说，useRef的更新不会引起当前组件或子组件的更新渲染

但使用React.createRef，每次重新渲染组件都会重新创建ref

示例：用useRef存储dom节点
```js
function Child() {
    const inputRef = useRef();
    console.log('input===inputRef', input === inputRef);
    input = inputRef;
    function getFocus() {
        inputRef.current.focus();
    }
    return (
        <>
            <input type="text" ref={inputRef} />
            <button onClick={getFocus}>获得焦点</button>
        </>
    )
}
```

用useRef解决useState异步更新不同步的问题：

针对上面useState谈到的异步更新不同步的问题，用useRef返回的immutable RefObject（把值保存在current属性上）来保存state，你可以把useRef存储的值堪称class组件实例中通过this存储的属性。然后取值方式从counter变成了->counterRef.current。如下：
```js
import React, { useState, useRef, useEffect } from "react";
​
const Counter = () => {
  const [counter, setCounter] = useState(0);
  const counterRef = useRef(counter);
​
  const onAlertButtonClick = () => {
    setTimeout(() => {
      alert("Value: " + counterRef.current);
    }, 3000);
  };
​
  useEffect(() => {
    counterRef.current = counter;
  });
​
  return (
    <div>
      <p>You clicked {counter} times.</p>
      <button onClick={() => setCounter(counter + 1)}>Click me</button>
      <button onClick={onAlertButtonClick}>
        Show me the value in 3 seconds
      </button>
    </div>
  );
};
​
export default Counter
```

React.forwardRef

在useRef出来之前，由于函数组件是没有实力的，所以函数组件无法使用ref属性来获取dom引用，而对应的解决方法就是React.forwardRef:
TextInput函数组件：
```js
const TextInput =  forwardRef((props,ref) => {
	//设置input标签node节点作为TextInput组件的ref引用
  return <input ref={ref}></input>
})
```

```js
function TextInputWithFocusButton() {
  // 关键代码
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // 关键代码，`current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
  return (
    <>
      // 用useRef存储TextInput设置的ref引用
     
      <TextInput ref={inputEl}></TextInput>
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
```

上面例子说明forwardRef和useRef配合，可以在父组件中操作子组件的ref对象

### useCallback
useCallback缓存一个函数，这个函数如果是由父组件作为props传递给子组件，或者自定义hooks里面的函数（通常自定义hooks里面的函数不会依赖于引用它的组件里面的数据），这时候我们可以考虑缓存这个函数，好处：

- 不用每次重新声明新的函数，避免释放内存，分配的计算资源浪费
- 子组件不会因为这个函数的变动重新渲染。（和React.memo搭配使用）

```js
function Example(){

    const [count, setCount]= useState(1);


    const getNum = useCallback(() => {
        return (555 * 666666 )+count
        //只有count值改变时，才会重新计算
    },[count])

    const Child = React.memo(({getNum}) =>{
        return <h4>总和{getNum()}</h4>
    })

    return <div>
           <Child getNum={getNum}/>
        <button onClick={() => setCount(count + 1)}>+1</button>
        </div>

}
```
上面例子，将一个函数交给useCallBack处理并且作为props传递给memo包裹的子组件并子组件调用该方法，定义只有当coutn变化时才会触发子组件重新渲染

因为通过useCallBack包裹后的函数通过props传递给子组件的永远是该函数的调用

### useMemo
useMemo主要用于渲染过程优化，两个参数依次是计算函数（通常是组件函数）和依赖状态列表，当依赖的状态发生改变时，才会触发计算函数的执行。如果没有指定依赖，则每一次渲染过程都会执行该计算函数。

useMemo的返回值就是计算函数的返回值
```js
function Example(){

    const [count, setCount]= useState(1);


    const getNum = useMemo(() => {
        return (555 * 666666 )+count
        
        //只有count值改变时，才会重新计算
    },[count])

    return <div>
    <h4>总和:{getNum}</h4>
        <button onClick={() => setCount(count + 1)}>+1</button>
        </div>

}

export default Example;
```

上面例子只有当count变化时才会触发getNum函数的重新计算和渲染。如果不使用useMemo则任何一个state发生变化都会导致组件重新渲染进而导致getNum重新计算，耗费性能

useMemo和useCallback的区别
useMemo和useCallback接收的参数都是一样，第一个参数为回调，第二个参数为要依赖的数据

共同作用：

- 仅仅依赖数据发生变化，才会重新计算结果，也就是起到缓存的作用。

两者区别：

- useMemo计算结果是计算函数返回来的值，主要用于缓存计算结果的值，应用场景如：需要计算的状态
- useCallback计算结果是计算函数，主要用于缓存函数，应用场景如：需要缓存的函数，因为函数式组件每次任何一个state的变化，整个组件都会被重新刷新，一些函数是没有必要重新刷新的，此时就应该缓存起来，提高性能，和减少资源浪费。

> 注意：当父组件更新渲染（state和props变化）时，无论子组件的props是否改变都会默认更新渲染子组件。所以这种情况下，若你的useMemo或useCallback时用来传给子组件的props时，都必须借助React memo来包裹子组件完成缓存，才能避免子组件的无效多余更新


### 自定义Hook
自定义的Hook必须以use开头，这个约定非常重要，不遵循的话，由于无法判断某个函数是否包含对其内部Hook的调用，React将无法自动检查你的Hook是否违反了Hook的规则。

##### useDidMount
```js
import { useEffect } from 'react';

const useDidMount = fn => useEffect(() => fn && fn(), []);

export default useDidMount;
```

##### useWillUnmount
useEffect时已经提起过，其允许返回一个清除副作用的函数，当依赖项为[]时，其相当与componentWillUnMount
```js
import { useEffect } from 'react';

const useWillUnmount = fn => useEffect(() => () => fn && fn(), []);

export default useWillUnmount;
```

##### 实现类似class组件可支持回调的setState方法
class组件更新状态时，setState可以通过第二个参数拿到更新完毕后的回调函数。很遗憾，虽然hooks函数的useState第二个参数回调支持类似class组件的setState第一个参数的用法（通过传入一个函数并将函数的返回值作为新的state进行更新），但不支持第二个参数回调，但是很多业务场景中我们又希望hooks组件能支持更新后的回调这一方法。

借助useRef和useEffect配合useState来实现这一功能：
```js
const useXState = (initState) => {
    const [state, setState] = useState(initState)
    //表示有state值更新了
    let isUpdate = useRef()
    const setXState = (state, cb) => {
        //这里setState是使用了函数参数的方式更新useState的值，而不是直接更新成指定的参数值
        setState(prev => {
            isUpdate.current = cb
            return typeof state === 'function' ? state(prev) : state
        })
    }
    useEffect(() => {
        if(isUpdate.current) {
        	//存在更新state，则执行回调
            isUpdate.current()
        }
    })

    return [state, setXState]
}

export default useXState
```

说明：
利用useRef的特性来作为标识区分挂载还是更新，当执行setState时，会传入和setState一摸一样的参数，并且将回调赋值给useRef的current属性，这样在更新完成时，我们手动调用current即可实现更新后的回调这一功能

##### Hooks vs Render Props vs HOC
没有hooks之前，高阶组件和Render Props本质上都是将复用逻辑提升到父组件中。这样就能够避免HOC和Render Props带来的【嵌套地狱】。但是，像Context的和这样有父子层级关系（树状结构关系）的，还是只能使用Render Props或者HOC。

对于Hooks、Render Props和告诫组件来说，它们都有各自的使用场景

Hooks:
> 替代Class的大部分用例，除了getSnapshotBeforeUpdate和componentDidCatch还不支持。可提取复用逻辑。除了有明确父子关系的，其他场景都可以使用Hooks。

Render Props:
> 在组件渲染上拥有更高的自由度，可以根据父组件提供的数据进行动态渲染。适合有明确父子关系的场景

高阶组件：
适合用来注入，并且生成一个新的可复用组件。适合用来写插件

不过，能使用Hooks的场景还是应该优先使用Hooks，其次才是Render Props和HOC。当然，Hooks、Render Props和HOC不是对立的关系。我们既可以用Hook来写Render Props和HOC，也可以在HOC中使用Render Props和Hooks。


参考文章：https://blog.csdn.net/qq_43293207/article/details/117631126
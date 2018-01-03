学习样本项目地址：[地址](https://github.com/sundaypig/react-build-tutorial/tree/master/step0)

**dependencies和devDependencies的区别。**

dependencies就是程序跑起来需要的模块，没有这个模块程序就会报错。  
例如react，或者是开发时的插件

devDependencies见命知意了，开发程序的时候需要的模块了。  
例如编译工具 babel ，koa等

举个例子，你用angularjs框架开发一个程序，开发阶段需要用到gulp来构建你的开发和本地运行环境。所以angularjs一定要放到dependencies里，因为以后程序到生产环境也要用。gulp则是你用来压缩代码，打包等需要的工具，程序实际运行的时候并不需要，所以放到dev里就ok了。  
再深入一些，你写程序要用ES6标准，浏览器并不完全支持，所以你要用到babel来转换代码。程序里有消息提示，你想用toaster。同样一个开发用，一个运行用。所以babel放dev，toaster放dependencies。

webpack 中间件

1. webpack-dev-middleware
2. webpack-hot-middleware

   * React 只会更新必要的部分
   * ![](https://i.imgur.com/NLbjaSI.gif)
   * 论是使用函数或是类来声明一个组件，它决不能修改它自己的props。
   * 因为 this.props 和 this.state 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。  

            // Wrong
            this.setState({
              counter: this.state.counter + this.props.increment,
            });

        要修复它，请使用第二种形式的 setState() 来接受一个函数而不是一个对象。 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数：

            // Correct
            this.setState((prevState, props) => ({
              counter: prevState.counter + props.increment
            }));

    - 想象一个组件树作为属性的瀑布，每个组件的状态就像一个额外的水源，它连接在一个任意点，但也流下来。
    - 不能使用返回 false 的方式阻止默认行为。你必须明确的使用 preventDefault e 是一个合成事件，不需要担心跨浏览器的兼容性问题


            function ActionLink() {
              function handleClick(e) {
                e.preventDefault();
                console.log('The link was clicked.');
              }

              return (
                <a href="#" onClick={handleClick}>
                  Click me
                </a>
              );
            }

    - 处理this，在dva框架中可以使用 @Bind 装饰器，平时需要指定onClick={(e) => this.handleClick(e)} 或者在controuctor中this.handleClick = this.handleClick.bind(this);
    - 值得注意的是，通过 bind 方式向监听函数传参，在类组件中定义的监听函数，事件对象 e 要排在所传递参数的后面
    - 可以通过使用{}在JSX内构建一个元素集合

            const numbers = [1, 2, 3, 4, 5];
            const listItems = numbers.map((number) =>
              <li>{number}</li>
            );
            ReactDOM.render(
              <ul>{listItems}</ul>,
              document.getElementById('root')
            );
    - 通常，我们使用来自数据的id作为元素的key **如果列表可以重新排序，我们不建议使用索引来进行排序，因为这会导致渲染变得很慢。** 元素的key只有在它和它的兄弟节点对比时才有意义  


            比方说，如果你提取出一个ListItem组件，你应该把key保存在数组中的这个<ListItem />元素上，而不是放在ListItem组件中的<li>元素上。

            function ListItem(props) {
              // 对啦！这里不需要指定key:
              return <li>{props.value}</li>;
            }

            function NumberList(props) {
              const numbers = props.numbers;
              const listItems = numbers.map((number) =>
                // 又对啦！key应该在数组的上下文中被指定
                <ListItem key={number.toString()}
                          value={number} />

              );
              return (
                <ul>
                  {listItems}
                </ul>
              );
            }

            const numbers = [1, 2, 3, 4, 5];
            ReactDOM.render(
              <NumberList numbers={numbers} />,
              document.getElementById('root')
            );

        这么做有时可以使你的代码更清晰，但有时这种风格也会被滥用。就像在JavaScript中一样，何时需要为了可读性提取出一个变量，这完全取决于你。但请记住，如果一个map()嵌套了太多层级，那可能就是你提取出组件的一个好时机。

    - 当你有处理多个受控的input元素时，你可以通过给每个元素添加一个name属性，来让处理函数根据 event.target.name的值来选择做什么。
    - 状态提升



            const scaleNames = {
              c: 'Celsius',
              f: 'Fahrenheit'
            };

            function toCelsius(fahrenheit) {
              return (fahrenheit - 32) * 5 / 9;
            }

            function toFahrenheit(celsius) {
              return (celsius * 9 / 5) + 32;
            }

            function tryConvert(temperature, convert) {
              const input = parseFloat(temperature);
              if (Number.isNaN(input)) {
                return '';
              }
              const output = convert(input);
              const rounded = Math.round(output * 1000) / 1000;
              return rounded.toString();
            }

            function BoilingVerdict(props) {
              if (props.celsius >= 100) {
                return <p>The water would boil.</p>;
              }
              return <p>The water would not boil.</p>;
            }

            class TemperatureInput extends React.Component {
              constructor(props) {
                super(props);
                this.handleChange = this.handleChange.bind(this);
              }

              handleChange(e) {
                this.props.onTemperatureChange(e.target.value);
              }

              render() {
                const temperature = this.props.temperature;
                const scale = this.props.scale;
                return (
                  <fieldset>
                    <legend>Enter temperature in {scaleNames[scale]}:</legend>
                    <input value={temperature}
                           onChange={this.handleChange} />
                  </fieldset>
                );
              }
            }

            class Calculator extends React.Component {
              constructor(props) {
                super(props);
                this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
                this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
                this.state = {temperature: '', scale: 'c'};
              }

              handleCelsiusChange(temperature) {
                this.setState({scale: 'c', temperature});
              }

              handleFahrenheitChange(temperature) {
                this.setState({scale: 'f', temperature});
              }

              render() {
                const scale = this.state.scale;
                const temperature = this.state.temperature;
                const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
                const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

                return (
                  <div>
                    <TemperatureInput
                      scale="c"
                      temperature={celsius}
                      onTemperatureChange={this.handleCelsiusChange} />
                    <TemperatureInput
                      scale="f"
                      temperature={fahrenheit}
                      onTemperatureChange={this.handleFahrenheitChange} />
                    <BoilingVerdict
                      celsius={parseFloat(celsius)} />
                  </div>
                );
              }
            }

            ReactDOM.render(
              <Calculator />,
              document.getElementById('root')
            );

    - React 具有强大的组合模型，react建议使用组合而不是继承来复用组件之间的代码。  react建议这些组件使用 children 属性将子元素直接传递到输出



    - <></> 是 <React.Fragment/> 的语法糖

            class Columns extends React.Component {
              render() {
                return (
                  <>
                    <td>Hello</td>
                    <td>World</td>
                  </>
                );
              }
            }

            <table>
              <tr>
                <td>Hello</td>
                <td>World</td>
              </tr>
            </table>

            class Table extends React.Component {
              render() {
                return (
                  <table>
                    <tr>
                      <Columns />
                    </tr>
                  </table>
                );
              }
            }
        key 是唯一可以传递给 Fragment 的属性

        <></> 语法不能接受键值或属性

        如果你需要一个带 key 的片段，你可以直接使用 <React.Fragment /> 。


            function Glossary(props) {
              return (
                <dl>
                  {props.items.map(item => (
                    // 没有`key`，将会触发一个key警告
                    <React.Fragment key={item.id}>
                      <dt>{item.term}</dt>
                      <dd>{item.description}</dd>
                    </React.Fragment>
                  ))}
                </dl>
              );
            }

## webpack 基础配置

* .babelrc 文件配置babel。

  ```
    {
        "presets": [
            [
                "latest",
                {
                    "modules": false
                }
            ],
            "stage-0",
            "react"
        ],
        "plugins": [
            "transform-runtime",
            "transform-decorators-legacy"
        ],
        "env": { // 在特定环境下执行的插件
            "production": {
              "plugins": ["transform-react-constant-elements"]
            }
          }
    }
  ```

## applyMiddleware

### middleware 的函数签名是 \({ getState, dispatch }\) =&gt; next =&gt; action。

```
import { createStore, applyMiddleware } from 'redux'
import todos from './reducers'
```

接受 Store 的 dispatch 和 getState 函数作为命名参数

```
function logger({ getState }) { //
```

该函数会被传入 被称为 next 的下一个 middleware 的 dispatch 方法

```
  return (next) => (action) => {
```

并返回一个接收 action 的新函数

```
    console.log('will dispatch', action)

    // 调用 middleware 链中下一个 middleware 的 dispatch。
    let returnValue = next(action)

    console.log('state after dispatch', getState())

    // 一般会是 action 本身，除非
    // 后面的 middleware 修改了它。
    return returnValue
  }
}
```

遵循 Redux middleware API 的函数。每个 middleware 接受 Store 的 dispatch 和 getState 函数作为命名参数，并返回一个函数

```
let createStoreWithMiddleware = applyMiddleware(logger)(createStore)
let store = createStoreWithMiddleware(todos, [ 'Use Redux' ])

store.dispatch({
  type: 'ADD_TODO',
  text: 'Understand the middleware'
})
// (将打印如下信息:)
// will dispatch: { type: 'ADD_TODO', text: 'Understand the middleware' }
// state after dispatch: [ 'Use Redux', 'Understand the middleware' ]
```

### createStore\(reducer, \[initialState\], enhancer\)

* reducer \(Function\): 接收两个参数，分别是当前的 state 树和要处理的 action，返回新的 state 树。  
* \[initialState\] \(any\): 初始时的 state。
* enhancer \(Function\): Store enhancer 是一个组合 store creator 的高阶函数，返回一个新的强化过的 store creator  

```
    import { createStore } from 'redux'


reducer 当前的 state 树和要处理的 action，返回新的 state 树

    function todos(state = [], action) {
      switch (action.type) {
        case 'ADD_TODO':
          return state.concat([ action.text ])
        default:
          return state
      }
    }

    let store = createStore(todos, [ 'Use Redux' ])

    store.dispatch({
      type: 'ADD_TODO',
      text: 'Read the docs'
    })

    console.log(store.getState())
    // [ 'Use Redux', 'Read the docs' ]
```

### store Store 就是用来维持应用所有的 state 树 的一个对象。 改变 store 内 state 的惟一途径是对它 dispatch 一个 action。

1. getState\(\)
2. dispatch\(action\)
3. subscribe\(listener\)
4. replaceReducer\(nextReducer\)

### combineReducers\(reducers\)

reducers/todos.js

```
export default function todos(state = [], action) {
  switch (action.type) {
  case 'ADD_TODO':
    return state.concat([action.text])
  default:
    return state
  }
}
```

reducers/counter.js  
.....

```
import { combineReducers } from 'redux'
import todos from './todos'
import counter from './counter'
```

将所有的reducers 合并到一起

```
export default combineReducers({
  todos,
  counter
})
```

然后通过 createStore\(reducers\) 集合管理

### bindActionCreators\(actionCreators, dispatch\)

1. actionCreators \(Function or Object\): 一个 action creator，或者键值是 action creators 的对象。
2. dispatch \(Function\): 一个 dispatch 函数，由 Store 实例提供。

### compose\(...functions\)  \(arguments\): 需要合成的多个函数。

使用组合

```
finalCreateStore = compose(
    applyMiddleware(...middleware),
    require('redux-devtools').devTools(),
    require('redux-devtools').persistState(
      window.location.href.match(/[?&]debug_session=([^&]+)\b/)
    ),
    createStore
  );
```

不使用组合

```
  // finalCreateStore =
  //   applyMiddleware(middleware)(
  //     devTools()(
  //       persistState(window.location.href.match(/[?&]debug_session=([^&]+)\b/))(
  //         createStore
  //       )
  //     )
  //   );
```

# React-router

# webpack

# node




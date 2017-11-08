一、 **打包去掉 console.log\(\)**

> > webpack.config.js 的 plugins 里面加上
> >
> > drop\_debugger: true,
> >
> > drop\_console: true

```
new webpack.optimize.UglifyJsPlugin({
  compress:{
    warnings: false,
    drop_debugger: true,
    drop_console: true
  }
})
```

**二、react router 4 结合webpack按需加载**

```
   route4相对于route更新较大，route 3的方法已不适用于4

   import loadSomething from 'bundle-loader?lazy!./Something'（官方给的引入文件例子，会报错）

   我们首先需要一个异步加载的包装组件Bundle。Bundle的主要功能就是接收一个组件异步加载的方法，并返回相应的react组件：
```

* ```
  import React, { Component } from 'react'

  class Bundle extends Component {
      state = {
          // short for "module" but that's a keyword in js, so "mod"
          mod: null
      }

      componentWillMount() {
          this.load(this.props)
      }

      componentWillReceiveProps(nextProps) {
          if (nextProps.load !== this.props.load) {
              this.load(nextProps)
          }
      }

      load(props) {
          this.setState({
              mod: null
          })
          //注意这里，使用Promise对象; mod.default导出默认
          props.load().then((mod) => {
              this.setState({
                  mod: mod.default ? mod.default : mod
              });
          });

      }

      render() {
          return this.state.mod ? this.props.children(this.state.mod) : "";
      }
  }

  export default Bundle
  ```

然后在定义路由的页面用bundle包装文件

```
const Chat = (props) => (
    <Bundle load={() => import('./component/chat')}>
        {(Chat) => <Chat {...props}/>}
    </Bundle>
);
```

路由定义可以不用改变：

```
<Route path="/chat" component={Chat}/>
```

这里贴一下路由配置完整文件（demo地址：[https://github.com/han88829/react-router-domV4-mobx](https://github.com/han88829/react-router-domV4-mobx "github")）：

```
import React, { Component } from 'react';
import { BrowserRouter as Router, Route, Switch, Redirect, withRouter } from 'react-router-dom';
import Admin from './component/admin';
// import Login from './component/login';
import Bundle from './Bundle';//按需加载

const Login = (props) => (
  <Bundle load={() => import('./component/login')}>
    {(Login) => <Login {...props} />}
  </Bundle>
)

class RouterS extends Component {

  render() {
    return (
      <Router>
        <div>
          <Switch>
            <Route path="/home" component={Admin} />
            <Route path="/login" exact component={Login} />

            {/* 重定向路由 */}
            {<Redirect from="/" to="/home/app" />}

          </Switch>
        </div>
      </Router>
    );
  }
}


export default RouterS;
```

参考地址：[http://www.jianshu.com/p/547aa7b92d8c](http://www.jianshu.com/p/547aa7b92d8c "简书")


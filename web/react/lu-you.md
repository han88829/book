### 

### 一、在组件中定义子路由，跳转空白页面详细代码如下：

父组件

```
class Main extends Component {
render() {
return (
<Router basename={'/m'}>
<div>
<Switch>
<Route path='/aa' exact component={Index}/>
</Switch>

</div>
</Router>
);
}
```



子组件

```
const Home = () => (
<div>
<h2>Home</h2>
</div>
)
const Index = ({match}) => (
<div>
<Link to="/aa/home">首页</Link>
<div className="App" >
<Switch>
<Route path='/aa/home' exact component={Home}/>
</Switch>
</div>
</div>
)
export default Index
```

在子组件点击首页时，路由页面发生变化，但是并没有显示Home，这是因为在父组件定义路由时使用了精确匹配，所以无法渲染Home页面；

父组件改造如下：

```
class Main extends Component {
render() {
return (
<Router basename={'/m'}>
<div>
<Switch>
<Route path='/aa' exact component={Index}/> //exact需要去掉
</Switch>

</div>
</Router>
);
}
```




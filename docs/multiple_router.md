
## 多路由使用说明

### 获取Actions 方法
~~import { Actions } from 'react-native-router-flux-cyq';~~
```js
// 第一种
// class ...
  componentDidMount() {
    this.actions = this.refs[`router${routerLevel}`].getRoutes(); // 必须等组件装载完获取
  }

// 第二种
// class ...
  static contextTypes = {
    routes: PropTypes.object,
  };

  constructor(props, context) {
    super(props);
    this.Actions = context.routes;
  }
```

### 嵌套子路由
```jsx
  // 主router
  <Scene
    key="hourlyWorkerApp"
    component={HourlyWorkerApp}
    hideNavBar            // 隐藏主路由的导航
    hideTabBar
    onBack={() => { Actions.pop(); }}
    title="小时工APP"
  />

  // 子router HourlyWorkerApp
  <Scene
    key="hourlyWorkerNav"
    initial={'hourlyWorkerNav' == uri}
    showBack={'hourlyWorkerNav' == uri}    // 显示回退按钮
    onBack={() => { 'hourlyWorkerNav' == uri ? this.props.onBack() : Actions.pop() }}
    component={HourlyWorkerNav}
    hideNavBar={false}
    hideTabBar
    title="小时工招聘"
  />
```

### 从父应用跳进子应用
```js
  RouterUtil.ACL('hourlyWorkerApp', { uri: 'workingDays', params });
  // or
  Actions.hourlyWorkerApp({ uri: 'workingDays', params });
```

### 子应用跳回父应用
```js
  RouterUtil.callback('basicinfo')
```

### 初始化RouterUtil
```js
  _initStore() {
    // init redux store from realm
    appStore.dispatch({ type: 'SAGA_INIT_STORE' });
  }

  _initActions() {
    const { service } = this.props;
    this.actions = this.refs.router.getRoutes(); // 必须等组件装载完获取
    RouterUtil.init(this.actions, service);
  }
  
  componentWillMount() {
    this._initStore();
  }

  componentDidMount() {
    this._initActions();
  }
```

### 子应用
```js

// 子 router id 计数器
let routerLevel = 0;  // 多次进入子应用场景
class childApp extends Component {
  constructor(props) {
    super(props);
    routerLevel++;
    this.actions = null;
  }

  render() {
    return (
      <Provider store={store}>
        <Router
          ref={"router" + routerLevel}
          routerID={"AgentApp" + routerLevel}
          ...
        >
        ...
    )
  }
}
```

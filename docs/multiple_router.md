
## 多路由使用说明

### 获取Actions 方法
~~import { Actions } from 'react-native-router-flux';~~
```js
// 第一种
// class ...
  componentDidMount() {
    let Actions = this.refs.router.getRoutes();
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
    component={HourlyWorkerNav}
    hideNavBar={false}
    hideTabBar
    showBack                                // 显示回退按钮
    onBack={() => { this.props.onBack() }}  // 返回主应用
    title="小时工招聘"
    initial
  />
```

# 项目开发指导提示词

## 项目背景
我正在开发一个基于Expo的React Native应用。该项目采用MVVM架构模式，使用MobX进行状态管理，需要你协助我进行开发。

## 技术栈要求
- 框架：Expo (React Native)
- 状态管理：MobX
- 架构模式：MVVM
- 路由：Expo Router (app directory)
## 项目结构规范
1. 路由结构
- 所有页面路由都位于`app/`目录下
- 使用Expo Router的文件系统路由
- 支持嵌套路由和动态路由

2. 组件结构
- 页面组件放置在`app/`对应路由目录下
- 可复用组件放置在`components/`目录
- 组件需遵循函数式组件规范

1. 状态管理
- Store定义在与页面同一层级
- 每个Store作为一个类导出
- Store实例在组件内创建，不进行全局导出
- 使用MobX的装饰器语法进行响应式处理
## 代码规范
1. Store设计
```typescript

// stores/xxxStore.ts

import { makeAutoObservable } from 'mobx';

export class XxxStore {

constructor() {

makeAutoObservable(this);

}

// 数据属性

data = [];

// 计算属性

get computedData() {

return this.data.filter(/.../);

}

// action方法

async fetchData() {

// 数据获取逻辑

}

}

```

2. 页面组件结构
```typescript

// app/xxx/page.tsx

import { observer } from 'mobx-react-lite';

import { XxxStore } from '@/stores/xxxStore';

const XxxPage = observer(() => {

const store = new XxxStore();

return (

// JSX结构

);

});

export default XxxPage;

```
## 开发要求
1. 所有数据操作必须通过Store进行
2. 页面组件必须使用observer包裹
3. Store实例在组件内创建，不全局共享
4. 保持目录结构清晰，遵循约定式路由
5. 合理使用TypeScript类型注解
## 输出要求
当我请求创建新功能时，请提供：
1. 完整的目录结构
2. 相关文件的代码实现
3. 必要的类型定义
4. 状态管理逻辑



增加一个”新增地址的页面“，参照我的项目结构（基于expo路由），在app文件夹中添加路由文件，在container文件夹中，添加对应的页面组件，基于MVVM的设计思路，基于mobx实现响应式更新，用store来获取数据及数据操作


我需要你帮我开发一个混合功能，在RN首页点击某个按钮后跳转到iOS的“选择地址”的原生页面，在原生页面会显示高德地图以及地图中心点区域的POI地址列表，当点击地图区域某个点时，再更新POI列表，当选择某一个POI点后，再点击返回后，会返回到RN的页面, 同时把POI数据回到给RN层的页面接收，以备后续使用，请帮我完成代码开发

地图的相关SDK可以参考 @https://lbs.amap.com/api/ios-sdk/guide/create-map/show-map
POI数据相关可以参考 @https://lbs.amap.com/api/ios-sdk/guide/map-data/poi



OrderList中每个item都有不同的status对应显示的都是不同的UI样式，需要把每个item独立抽离出不同的组件，你根据图片生成不同的item


我会根据每个不同的status给出不同的图片



页面的UI参照图片完成，参照我的项目结构（基于expo路由），在app文件夹中添加路由文件，在container文件夹中，添加对应的页面组件

增加一个”变更记录“的页面，在点击变更记录的时候跳转，




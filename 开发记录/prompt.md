增加一个”新增地址的页面“，参照我的项目结构（基于expo路由），在app文件夹中添加路由文件，在container文件夹中，添加对应的页面组件，基于MVVM的设计思路，基于mobx实现响应式更新，用store来获取数据及数据操作


我需要你帮我开发一个混合功能，在RN首页点击某个按钮后跳转到iOS的“选择地址”的原生页面，在原生页面会显示高德地图以及地图中心点区域的POI地址列表，当点击地图区域某个点时，再更新POI列表，当选择某一个POI点后，再点击返回后，会返回到RN的页面, 同时把POI数据回到给RN层的页面接收，以备后续使用，请帮我完成代码开发

地图的相关SDK可以参考 @https://lbs.amap.com/api/ios-sdk/guide/create-map/show-map
POI数据相关可以参考 @https://lbs.amap.com/api/ios-sdk/guide/map-data/poi



OrderList中每个item都有不同的status对应显示的都是不同的UI样式，需要把每个item独立抽离出不同的组件，你根据图片生成不同的item


我会根据每个不同的status给出不同的图片


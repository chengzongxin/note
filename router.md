# 路由文档


## 标签页路由 (/(tabs))

首页: /index

工作台: /workbench

我的: /mine

  

## 订单相关路由 (/(order))

订单详情: /(order)/detail

- 参数: orderNo: string - 订单编号

  

我的订单: /(order)/myOrders

- 参数: index: string - 订单列表tab索引

  

预约上门: /(order)/schedule

- 参数: isReBook: string - 是否为重新预约

  

## 测量相关路由 (/(measure))

测量信息: /measureInfo

- 参数:

- orderNo: string - 订单编号

- type: string - 测量类型

  

测量类别: /categories

- 参数:

- roomId: string - 房间ID

- type: string - 类型

  

测量模板: /template

- 参数:

- categoryId: string - 类别ID

- roomId: string - 房间ID

- type: string - 类型

  

测量附加信息列表: /measureAdditional/index

- 参数: orderNo: string - 订单编号

  

测量附加信息编辑: /measureAdditional/edit

- 参数:

- orderNo: string - 订单编号

- id?: string - 编辑时的记录ID

- mode: 'add' | 'edit' - 操作模式

  

## 钱包相关路由 (/(wallet))

银行卡列表: /(bankCard)/(bankCardList)/index

绑定银行卡: /(bankCard)/(bindBankCard)/index

账单列表: /(bill)/(billList)/index

设置支付密码: /(password)/(setPassword)/index

钱包主页: /(walletPage)/index

  

## 服务相关路由 (/(service))

服务区域检查: /serviceAreaCheck/index

  

服务区域选择: /serviceAreaSelection/index

- 参数:

- type: string - 区域类型

- title: string - 页面标题

- serviceAreaIds: string - 已选区域ID

- serviceAreaNames: string - 已选区域名称

  

服务类别检查: /serviceCategoryCheck/index

服务类别选择: /serviceCategorySelection/index

- 参数: index: string - 类别索引

  

## 消息相关路由 (/(message))

消息中心: /messageCenter/index

  

## 登录认证路由 (/(login))

登录: /login

注册: /register

忘记密码: /forgetPW

确认密码: /confirmPW

恢复密码: /recoverPW

更新密码: /updatePW

  

## 我的相关路由 (/(mine))

关于页面: /about

意见反馈: /feedback

反馈详情: /feedbackDetail

- 参数: feedbackId: string - 反馈ID

  

## WebView路由 (/(webview))

网页视图: /index

- 参数:

- url: string - 网页URL

- title: string - 页面标题

  

## 特殊路由

404页面: /+not-found
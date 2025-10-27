18219111991
验证码：9999

16219111991

223:
13246601822

测试环境后台：
http://192.168.2.223/#/main/system/order_center
16219111991

测试环境数据库：
192.168.2.161 3306
root
Hxc%123456


1. 页面卡主-订单大厅
2. 签到备注是不是必填（200m外必填）
3. 提交完工要判断是否离场



测量列表

房间列表（以远程为主）
/app-api/system/measure-order-room/list


具体测量项数据-获得房间测量项列表
/app-api/system/measure-order-term/list

选择分类
/app-api/system/measure-term/list-all
/app-api/system/measure-term/list-simple

房间测量项批量上传
/app-api/system/measure-order-term/batch-upload
测量单据房间批量上传
/app-api/system/measure-order-room/batch-upload


我的订单Tab：
待二次上门审批
待退单审批



上周：11.11~11.16
1. 工作台功能开发完成
2. 修改师傅端上的部分bug
3. 测量模块测量信息页面、房间页面、创建房间、复制房间等功能开发
4. 批量上传测量项功能开发
5. 数据库备份功能开发
本周：11.18~11.22
1. 批量上传房间功能
2. 二次上门功能逻辑
3. 备份功能完成
4. bug修复



二次上门：
背景：不修改原来的记录，产生一个副本，修改自己的记录
副本（测量单，房间，测量项）


逻辑：


1. 修改测量项，产生一条新的测量项，房间列表里关联新的测量项，（移除之前的测量项）

二次上门：
1. app直接修改原有数据，上传的时候会带标识type=2
2. 后端会返回两条一样的_id,过滤掉后面给的_id（要做下排序）





3. 修改还是修改本地
4. 上传
   
   

那我这边逻辑，

房间：新建房间的时候，默认type=1，二次上门的时候，新建或复制，type=2，之后就按照type取值传给你就行
测量项： 新建，默认type=1，二次上门，新建和编辑，type=2


用户端APP缺失功能
1，项目详情NSB数据展示
2，订单详情测量信息展示
3，订单信息测量附加信息展示
4，订单评价默认评价展示
5，APP消息通知，消息提醒、公告通知、（iOS、安卓推送）
6，APP升级提醒
7，APP协议与隐私展示
8，APP多用户切换
9，BUG修复
10，页面还原


师傅端APP缺失功能
1，APP消息通知，消息提醒、公告通知、（iOS、安卓推送）
2，APP升级提醒
3，APP协议与隐私展示
4，支付模块实现
5，测量附加模块
6，师傅技能选择到二级
7，项目详情NBS数据展示
8，BUG修复、（测量模块BUG）
9，页面还原
10，测量备份
11，订单状态
12，首页订单模块


bug：
二次上门审批，不选择自己，审批通过后，订单变成已完成，需要隐藏签到



测量版本控制方案：

1. 旧数据-》新库（x）
	1. 数据库版本迁移
2. 请求头带版本号
	1. 返回版本号所需的数据结构
3. 新数据-》旧库（x）
	1. 返回版本号所需的数据结构




师傅端：
1. ~~消息中心模块~~
2. ~~消息通知、推送、语音播报~~
3. ~~公告通知~~                 ============（api）
4. ~~nbs的项目、订单数据的同步~~
5. ~~测量订单的测量附加信息~~
6. ~~版本升级~~
7. 账号注销
8. ~~服务协议、隐私政策~~
9. 测量二次上门
10. 备份多账号问题
11. 接单大厅筛选
12. 我的订单-其他筛选
13. 师傅技能选择到二级
14. 测量bug
15. 订单bug



bugs：
1. 接单、拒单，两个toast
2. 接单后，要刷新接单大厅列表
3. 待退单审批、已退单，显示了施工中tab


1. 已完成，施工工期不能编辑
2. 二次上门，不能修改上次的施工工期
3. 再次上门审批，修改工期，调用接口，同步订单详情工期进度



消息中心

https://www.pmdaniu.com/clouds/198016/24f9e08c964c28e98f67b37bf6932cdc-34618/start.html#id=vumr39&p=app&g=1


预约，重新预约，endTime，23：59：59
二次上门，也是


```
<key>NSBluetoothAlwaysUsageDescription</key>

<string>App需要使用蓝牙来连接测量设备</string>

<key>NSBluetoothPeripheralUsageDescription</key>

<string>App需要使用蓝牙来连接测量设备</string>

<key>NSLocationWhenInUseUsageDescription</key>

<string>我们需要访问您的位置数据，以便在您使用应用时为您提供定位服务。</string>

<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>

<string>我们需要访问您的位置数据，以便为您提供准确的位置信息和服务。</string>

<key>NSAppleMusicUsageDescription</key>

<string>我们需要访问您的Apple Music，以提供音乐相关的功能。</string>

<key>NSCameraUsageDescription</key>

<string>我们需要使用您的相机来拍摄照片和视频。</string>

<key>NSPhotoLibraryUsageDescription</key>

<string>我们需要访问您的照片库以便上传图片。</string>

<key>NSMicrophoneUsageDescription</key>

<string>我们需要访问您的麦克风以录制声音。</string>
```


1. 师傅端、用户端 app store审核问题修改
2. iOS通知推送测试验证修改
3. UI优化
4. bug修复
5. 备份逻辑优化







我需要增加一个功能
在打包成功后，会推送一条企业微信的通知到群里，请帮我完成脚本开发

webhook地址是
https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=dfd626b0-9f70-49a9-9eae-e90df09d5050 

下面是企业微信开发者文档
@https://developer.work.weixin.qq.com/document/path/91770 
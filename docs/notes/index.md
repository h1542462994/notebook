## 用户中心

### 需求

好用的，规范的用户中心系统。用户中心系统包含用户数据，用户验证，oauth三个模块。

- 返回序列
	- status:

- 用户状态验证
    - refresh_token
    - token


- 路由：
	- api/tool 工具接口
		- time 获取服务器的时间 {display=[none, standard]}
        - encrypt 加密 {value=str:urlencoded}
    - api/app
        - create 创建应用
    - api/user 用户系统接口中心
		- register 注册 {name=str password=encrypt(time,slice) email=str:email nickname=str
        - login 登录 {name=str password=encrypt(time,slice) device=str app=str} 
        - auto 自动登录
        - exit 退出登录
        - state 状态
        
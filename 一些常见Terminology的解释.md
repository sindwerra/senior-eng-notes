

### 三大云服务类型：
1. SaaS（Software as a service）
	a. 软件即服务
	b. 用户基本上只需要一个用户名和密码即可以使用供应商提供的全部服务
	c. 现实例子如salesforce、veeva
2. PaaS（Platform as a service）
	a. 平台即服务
	b. 供应商提供了一些中间层次的工具供使用比如OS、中间件、数据库之类的服务
	c. 现实例子如heroku、Google App Engine
3. IaaS（Infrastructure as a service）
	a. 架构即服务
	b. 供应商只提供最基本的硬件服务比如计算（CPU）、存储（硬盘空间）、网络服务，其他一切如操作系统安装、应用还有数据持久化方案都需要用户自己来实现
	c. 现实例子如AWS、Azure、阿里云
	

基本上来说，IaaS是最底层的云服务供应等级，而SaaS已经和普通的网站无异，属于最上层的云服务提供方式

### API Gateway与BFF
API Gateway是一种微服务中常用的用来隔离前端和后端微服务的中间层，尤其来作为一个代理调用各种微服务
BFF的意思是Backend for Frontend，其本质和API Gateway差不多
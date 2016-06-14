# 分布式任务调度平台xxl-job
源码地址

- github地址：https://github.com/xuxueli/xxl-job
- git.osc地址：http://git.oschina.net/xuxueli0323/xxl-job

博客地址

- oschina地址：http://my.oschina.net/xuxueli/blog/690978
- cnblogs地址：http://www.cnblogs.com/xuxueli/p/5021979.html
- csdn地址：http://blog.csdn.net/xuxueli0323/article/details/51674330

技术交流群(仅作技术交流)：367260654    [![image](http://pub.idqqimg.com/wpa/images/group.png)](http://shang.qq.com/wpa/qunwpa?idkey=4686e3fe01118445c75673a66b4cc6b2c7ce0641528205b6f403c179062b0a52)


# 特点：
- 1、简单：支持通过Web页面对任务进行CRUD操作，操作简单，一分钟上手；
- 2、动态：支持动态修改任务状态、暂停/恢复任务，以及终止运行中任务，即时生效；
- 3、调度HA：“调度中心”基于集群Quartz实现，可保证调度中心HA；
- 4、任务HA：任务支持多地址配置，可保证任务HA（Failover）；
- 5、一致性：“调度中心”通过DB锁保证集群分布式调度的一致性；
- 6、自定义任务参数：支持在线配置调度任务入参，即时生效；
- 7、调度线程池：调度系统多线程触发调度运行，确保调度精确执行，不被堵塞；
- 8、执行日志：支持在线查看调度结果，并且查看完整的执行日志；
- 9、邮件报警：任务失败时支持邮件报警，同时可自定义失败次数阀值；
- 10、支持登录验证；
- 11、GLUE：提供Web IDE，支持在线开发任务逻辑代码，动态发布，实时编译生效，省略部署上线的过程。
	
# 新版本 V1.1.x，特性
**【于V1.1.x版本，XXL-JOB正式应用于我司，内部定制别名为 “Ferrari”，新接入应用推荐使用最新版本V1.3.x】**

- 1、简单：支持通过Web页面对任务进行CRUD操作，操作简单，一分钟上手；
- 2、动态：支持动态修改任务状态，动态暂停/恢复任务，即时生效；
- 3、服务HA：任务信息持久化到mysql中，Job服务天然支持集群，保证服务HA；
- 4、任务HA：某台Job服务挂掉，任务会平滑分配给其他的某一台存活服务，即使所有服务挂掉，重启时或补偿执行丢失任务；
- 5、一个任务只会在其中一台服务器上执行；
- 6、任务串行执行；
- 7、支持自定义参数；
- 8、支持远程任务执行终止；

# 新版本 V1.2.x，新特性
- 1、支持任务分组；
- 2、支持“本地任务”、“远程任务”；
- 3、底层通讯支持两种方式，Servlet方式 + JETTY方式；
- 4、支持“任务日志”；
- 5、支持“串行执行”，并行执行；
	
	说明：V1.2版本将系统架构按功能拆分为：
		调度模块（调度中心）：负责管理调度信息，按照调度配置发出调度请求；
		执行模块（执行器）：负责接收调度请求并执行任务逻辑；
		通讯模块：负责调度模块和任务模块之间的信息通讯；
	优点：
		解耦：任务模块提供任务接口，调度模块维护调度信息，业务相互独立；
		高扩展性；
		稳定性；

# 新版本 V1.3.x，新特性
- 1、遗弃“本地任务”模式，推荐使用“远程任务”，易于系统解耦，任务对应的JobHander统称为“执行器”；
- 2、遗弃“servlet”方式底层系统通讯，推荐使用JETTY方式，调度+回调双向通讯，重构通讯逻辑；
- 3、UI交互优化：左侧菜单展开状态优化，菜单项选中状态优化，任务列表打开表格有压缩优化；
- 4、【重要】“执行器”细分为：BEAN、GLUE两种开发模式，简介见下文：
	
		“执行器” 模式简介：
			BEAN模式执行器：每个执行器都是Spring的一个Bean实例，XXL-JOB通过注解@JobHander识别和调度执行器；
			GLUE模式执行器：每个执行器对应一段代码，在线Web编辑和维护，动态编译生效，执行器负责加载GLUE代码和执行；
			
# 新版本V1.3.1
- 1、更新项目目录结构：
		/xxl-job-admin -------------------- 【调度中心】：负责管理调度信息，按照调度配置发出调度请求；
		/xxl-job-core ----------------------- 公共依赖
		/xxl-job-executor-example ------ 【执行器】：负责接收调度请求并执行任务逻辑；
		/db ---------------------------------- 建表脚本
		/doc --------------------------------- 用户手册
- 2、在新的目录结构上，升级了用户手册；
- 3、优化了一些交互和UI；
	
# 新版本1.3.2
- 1、调度逻辑进行事务包裹；
- 2、执行器异步回调执行日志；
- 3、【重要】在 “调度中心” 支持HA的基础上，扩展执行器的Failover支持，支持配置多执行期地址；

# 规划中
- 1、任务终止时，任务队列中调度回调通过被终止的接口；
- 2、任务执行规则自定义：假如前一个任务正在执行，后续调度执行规则支持自定义；
		串行（默认，当前逻辑）：后续调度入调度队列；
		并行：后续调度并行执行；
		Pass：后续调度被Pass；
- 3、兼容oracle；
- 4、任务依赖；

# 源码目录说明
- /xxl-job-admin					【调度中心】：负责管理调度信息，按照调度配置发出调度请求；
- /xxl-job-core					公共依赖
- /xxl-job-executor-example	【执行器】：负责接收调度请求并执行任务逻辑；
- /db		建表脚本
- /doc	用户手册
	
# Tips
我司大众点评已接入XXL-JOB，内部别名《Ferrari》（于V1.1.x版本，XXL-JOB正式应用于我司，内部定制别名为 “Ferrari”，新接入应用推荐使用最新版本V1.3.x）。自2016-01-21接入至2016-05-20未知，内部XXL-JOB系统已调度45000余次，表现优异。新接入应用推荐使用最新版本V1.3，因为经过两个大版本的更新，系统的任务模型、UI交互模型以及底层调度通讯模型都有了较大的提升，核心功能更加稳定高效。

XXL-JOB已接入多家公司的线上产品线，接入场景如电商业务，O2O业务和大数据作业等，截止2016-05-20为止，XXL-JOB已接入的公司包括不限于：

- 1、大众点评；
- 2、山东学而网络科技有限公司；
- 3、安徽慧通互联科技有限公司；
- 4、人人聚财金服；
- 5、上海棠棣信息科技股份有限公司
- 6、……

更多接入公司，欢迎在https://github.com/xuxueli/xxl-job/issues/1 登记。
	

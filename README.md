# springCloud-eureka-server-cluster

#### 项目介绍
springboot集成Eureka实现集群

#### 软件架构
软件架构说明


#### 安装教程

1. xxxx
2. xxxx
3. xxxx

#### 使用说明

高可用的注册中心实现思路
Eureka通过“伙伴”机制实现高可用。每一台Eureka都需要在配置中指定另一个Eureka的地址作为伙伴，Eureka启动时会向自己的伙伴节点获取当前已经存在的注册列表， 这样在向Eureka集群中新加机器时就不需要担心注册列表不完整的问题。
除此之外，Eureka还支持Region和Zone的概念。其中一个Region可以包含多个Zone。Eureka在启动时需要指定一个Zone名，即当前Eureka属于哪个zone, 如果不指定则属于defaultZone。Eureka Client也需要指定Zone, Client(当与Ribbon配置使用时)在向Server获取注册列表时会优先向自己Zone的Eureka发请求，如果自己Zone中的Eureka全挂了才会尝试向其它Zone。Region和Zone可以对应于现实中的大区和机房，如在华北地区有10个机房，在华南地区有20个机房，那么分别为Eureka指定合理的Region和Zone能有效避免跨机房调用，同时一个地区的Eureka坏掉不会导致整个该地区的服务都不可用。
本文我们通过运行eureka-server多个实例，并进行互相注册的方式来实现高可用的部署，我们只需要将Eureke Server配置其他可用的serviceUrl就能实现高可用部署。
我们可以创建三个注册中心节点，每个节点进行两两注册，实现完全对等的效果，可以达到集群的最高可用性，任何一个节点挂掉都不会影响服务的注册与发现。

1、服务器准备
		在winsow环境下测试 
		127.0.0.1       eureka-7001.com
		127.0.0.1       eureka-7002.com
		127.0.0.1       eureka-7003.com
		
2、创建三个项目
 		eureka-server-7001;
 		eureka-server-7002;
 		eureka-server-7003;
 		
3、修改application.yml

		eureka-server-7001的
				server:
				  port: 7001
				security:
				  basic:
				    enabled: false   # 启用安全认证处理
				#  user:
				#    name: edmin     # 用户名
				#    password: mldnjava  # 密码
				spring:
				  application:
				    name: eureka-server-7001
				eureka: 
				  client: # 客户端进行Eureka注册的配置
				    service-url:
				      defaultZone: http://eureka-7002.com:7002/eureka,http://eureka-7003.com:7003/eureka
				    register-with-eureka: false    # 当前的微服务不注册到eureka之中
				    fetch-registry: false     # 不通过eureka获取注册信息
				  instance: # eureak实例定义
				hostname: eureka-7001.com # 定义Eureka实例所在的主机名称

			eureka-server-7002的
					server:
					  port: 7002
					security:
					  basic:
					    enabled: false   # 启用安全认证处理
					#  user:
					#    name: edmin     # 用户名
					#    password: mldnjava  # 密码
					spring:
					  application:
					    name: eureka-server-7002
					eureka: 
					  client: # 客户端进行Eureka注册的配置
					    service-url:
					      defaultZone: http://eureka-7001.com:7001/eureka,http://eureka-7003.com:7003/eureka
					    register-with-eureka: false    # 当前的微服务不注册到eureka之中
					    fetch-registry: false     # 不通过eureka获取注册信息
					  instance: # eureak实例定义
					hostname: eureka-7002.com # 定义Eureka实例所在的主机名称
		
		eureka-server-7003的
				server:
				  port: 7003
				security:
				  basic:
				    enabled: false   # 启用安全认证处理
				#  user:
				#    name: edmin     # 用户名
				#    password: mldnjava  # 密码
				spring:
				  application:
				    name: eureka-server-7003
				eureka: 
				  client: # 客户端进行Eureka注册的配置
				    service-url:
				      defaultZone: http://eureka-7001.com:7001/eureka,http://eureka-7002.com:7002/eureka
				    register-with-eureka: false    # 当前的微服务不注册到eureka之中
				    fetch-registry: false     # 不通过eureka获取注册信息
				  instance: # eureak实例定义
				 hostname: eureka-7003.com # 定义Eureka实例所在的主机名称

4、分别启动三个项目
访问http://localhost:7002/ 

# springCloud-eureka-server-cluster

#### ��Ŀ����
springboot����Eurekaʵ�ּ�Ⱥ

#### ����ܹ�
����ܹ�˵��


#### ��װ�̳�

1. xxxx
2. xxxx
3. xxxx

#### ʹ��˵��

�߿��õ�ע������ʵ��˼·
Eurekaͨ������顱����ʵ�ָ߿��á�ÿһ̨Eureka����Ҫ��������ָ����һ��Eureka�ĵ�ַ��Ϊ��飬Eureka����ʱ�����Լ��Ļ��ڵ��ȡ��ǰ�Ѿ����ڵ�ע���б� ��������Eureka��Ⱥ���¼ӻ���ʱ�Ͳ���Ҫ����ע���б����������⡣
����֮�⣬Eureka��֧��Region��Zone�ĸ������һ��Region���԰������Zone��Eureka������ʱ��Ҫָ��һ��Zone��������ǰEureka�����ĸ�zone, �����ָ��������defaultZone��Eureka ClientҲ��Ҫָ��Zone, Client(����Ribbon����ʹ��ʱ)����Server��ȡע���б�ʱ���������Լ�Zone��Eureka����������Լ�Zone�е�Eurekaȫ���˲Ż᳢��������Zone��Region��Zone���Զ�Ӧ����ʵ�еĴ����ͻ��������ڻ���������10���������ڻ��ϵ�����20����������ô�ֱ�ΪEurekaָ�������Region��Zone����Ч�����������ã�ͬʱһ��������Eureka�������ᵼ�������õ����ķ��񶼲����á�
��������ͨ������eureka-server���ʵ���������л���ע��ķ�ʽ��ʵ�ָ߿��õĲ�������ֻ��Ҫ��Eureke Server�����������õ�serviceUrl����ʵ�ָ߿��ò���
���ǿ��Դ�������ע�����Ľڵ㣬ÿ���ڵ��������ע�ᣬʵ����ȫ�Եȵ�Ч�������Դﵽ��Ⱥ����߿����ԣ��κ�һ���ڵ�ҵ�������Ӱ������ע���뷢�֡�

1��������׼��
		��winsow�����²��� 
		127.0.0.1       eureka-7001.com
		127.0.0.1       eureka-7002.com
		127.0.0.1       eureka-7003.com
		
2������������Ŀ
 		eureka-server-7001;
 		eureka-server-7002;
 		eureka-server-7003;
 		
3���޸�application.yml

		eureka-server-7001��
				server:
				  port: 7001
				security:
				  basic:
				    enabled: false   # ���ð�ȫ��֤����
				#  user:
				#    name: edmin     # �û���
				#    password: mldnjava  # ����
				spring:
				  application:
				    name: eureka-server-7001
				eureka: 
				  client: # �ͻ��˽���Eurekaע�������
				    service-url:
				      defaultZone: http://eureka-7002.com:7002/eureka,http://eureka-7003.com:7003/eureka
				    register-with-eureka: false    # ��ǰ��΢����ע�ᵽeureka֮��
				    fetch-registry: false     # ��ͨ��eureka��ȡע����Ϣ
				  instance: # eureakʵ������
				hostname: eureka-7001.com # ����Eurekaʵ�����ڵ���������

			eureka-server-7002��
					server:
					  port: 7002
					security:
					  basic:
					    enabled: false   # ���ð�ȫ��֤����
					#  user:
					#    name: edmin     # �û���
					#    password: mldnjava  # ����
					spring:
					  application:
					    name: eureka-server-7002
					eureka: 
					  client: # �ͻ��˽���Eurekaע�������
					    service-url:
					      defaultZone: http://eureka-7001.com:7001/eureka,http://eureka-7003.com:7003/eureka
					    register-with-eureka: false    # ��ǰ��΢����ע�ᵽeureka֮��
					    fetch-registry: false     # ��ͨ��eureka��ȡע����Ϣ
					  instance: # eureakʵ������
					hostname: eureka-7002.com # ����Eurekaʵ�����ڵ���������
		
		eureka-server-7003��
				server:
				  port: 7003
				security:
				  basic:
				    enabled: false   # ���ð�ȫ��֤����
				#  user:
				#    name: edmin     # �û���
				#    password: mldnjava  # ����
				spring:
				  application:
				    name: eureka-server-7003
				eureka: 
				  client: # �ͻ��˽���Eurekaע�������
				    service-url:
				      defaultZone: http://eureka-7001.com:7001/eureka,http://eureka-7002.com:7002/eureka
				    register-with-eureka: false    # ��ǰ��΢����ע�ᵽeureka֮��
				    fetch-registry: false     # ��ͨ��eureka��ȡע����Ϣ
				  instance: # eureakʵ������
				 hostname: eureka-7003.com # ����Eurekaʵ�����ڵ���������

4���ֱ�����������Ŀ
����http://localhost:7002/ 

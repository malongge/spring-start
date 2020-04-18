# 1


start.spring.io: 通过该网址生成项目
actuator: /actuator/health(检查服务状态地址)
mvn clean package -Dmaven.test.skip (打包程序部署)
ll 命令查看目录详情

parents 模式：

    	<parent>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-parent</artifactId>
    		<version>2.2.6.RELEASE</version>
    		<relativePath/> <!-- lookup parent from repository -->
    	</parent>
    	
改为非 parents 模式:

    <dependencyManagement>
    		<dependencies>
    			<dependency>
    				<groupId>org.springframework.boot</groupId>
    				<artifactId>spring-boot-dependencies</artifactId>
    				<version>2.2.6.RELEASE</version>
    				<type>pom</type>
    				<scope>import</scope>
    			</dependency>
    		</dependencies>
    </dependencyManagement>
    
mvn plugin 增加 repackage

    <build>
    	<plugins>
    			<plugin>
    				<groupId>org.springframework.boot</groupId>
    				<artifactId>spring-boot-maven-plugin</artifactId>
    				<executions>
    					<execution>
    						<goals>
    							<goal>repackage</goal>
    						</goals>
    					</execution>
    				</executions>
    			</plugin>
    		</plugins>
    </build>

执行 jar 包：

     java -jar start-0.0.1-SNAPSHOT.jar

# 2 

暴露 actuator 断点配置

    management.endpoints.web.exposure.include=<comma separated endpoints you wish to expose>
    
常用的 actuator 路径

    /health, /env, /metrics, /beans
    
通过环境变量控制，使用的配置文件

    spring.profiles.active=${SPRING_PROFILES_ACTIVE:simple-jdbc}
    
datasource 的配置可以自定义配置

    spring.datasource.url=jdbc:h2:mem:testdb
    spring.datasource.username=sa
    spring.datasource.password=
    spring.datasource.schema=classpath:${SPRING_PROFILES_ACTIVE}-schema.sql
    spring.datasource.data=classpath:${SPRING_PROFILES_ACTIVE}-data.sql
    spring.datasource.hikari.maximumPoolSize=5
    spring.datasource.hikari.minimumIdle=5
    spring.datasource.hikari.idleTimeout=600000
    spring.datasource.hikari.connectionTimeout=30000
    spring.datasource.hikari.maxLifetime=1800000
    
默认链接池为：hikari

排除调某些 bean 的注入：

    @SpringBootApplication(exclude = { *.class, ...}
    
druid 配置参考：

    https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE
    
配置 druid 为连接持，需要先排除掉默认的 hikari：

    <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-jdbc</artifactId>
                <exclusions>
                    <exclusion>
                        <artifactId>HikariCP</artifactId>
                        <groupId>com.zaxxer</groupId>
                    </exclusion>
                </exclusions>
    </dependency>
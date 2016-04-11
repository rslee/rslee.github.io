# 日常问题

## 2016-04月

### Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'cwspuaas.hibernate_sequence' doesn't exist

该问题的原因是在于jpa的配置文件，我这边的配置文件如下：
```xml
<bean id="mainEntityManagerFactory"
		class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="dataSource" ref="mainDataSource"></property>
		<property name="packagesToScan" value="cn.rslee.uaas"></property>
		<property name="persistenceUnitName" value="mysqldb"></property>
		<property name="jpaVendorAdapter">
			<bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
				<property name="showSql" value="true"></property>
				<property name="database" value="MYSQL"></property>
			</bean>
		</property>
		<property name="jpaProperties">
			<props>
				<!--设置外连接抓取树的最大深度 -->
				<prop key="hibernate.max_fetch_depth">3</prop>
				<prop key="hibernate.jdbc.fetch_size">18</prop>
				<prop key="hibernate.jdbc.batch_size">10</prop>
				<!-- 自动建表类型 validate|create|create-drop|update -->
				<!--<prop key="hibernate.hbm2ddl.auto">create</prop>-->
				<!-- 是否显示SQL -->
				<prop key="hibernate.show_sql">true</prop>
				<!-- 显示SQL是否格式化 -->
				<prop key="hibernate.format_sql">false</prop>
				<!-- 关闭二级缓存 -->
				<prop key="hibernate.cache.provider_class">org.hibernate.cache.NoCacheProvider</prop>
				<!-- 关闭实体字段映射校验 -->
				<prop key="javax.persistence.validation.mode">none</prop>
			</props>
		</property>
	</bean>

```
网上查询情况来看，理论上面配置了这一段应该就可以解决问题了，但是实际的情况却是没有：
```xml

<bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
  <property name="showSql" value="true"></property>
  <property name="database" value="MYSQL"></property>
</bean>
```

参考的链接：
1. [对ORM的支持 之 8.4 集成JPA ——跟我学spring3](http://jinnianshilongnian.iteye.com/blog/1439369)
2. [使用Spring框架写JPA程序](http://blog.sina.com.cn/s/blog_4fb490ff0100xbly.html)

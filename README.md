#!/bin/bash

# Assuming your XML content is in a file named config.xml
file="config.xml"

# Extract username and password values from non-commented lines
username=$(grep -oP '^[^#]*(?!<!--)<property name="username" value="([^"]*)"' "$file" | awk -F '"' '{print $4}')
password=$(grep -oP '^[^#]*(?!<!--)<property name="password" value="([^"]*)"' "$file" | awk -F '"' '{print $4}')

echo "Username: $username"
echo "Password: $password"


<property name="driverClassName" value="org.postgresql.Driver" />
		<property name="url" value="jdbc:postgresql://50.21.25.115:3330/Private Cert Repo"/>                         
		<property name="username" value="abdc" />
		<property name="password" value="abdns" />

		<property name="initialSize" value="${database.initialSize}"/>
		<property name="validationQuery" value="${database.validationQuery}"/>
		<property name="testWhileIdle" value="${database.testWhileIdle}"/>
		<property name="minIdle" value="${database.minIdle}"/>
		<property name="maxIdle" value="${database.maxIdle}"/>
		<property name="maxActive" value="${database.maxActive}"/>
		<property name="timeBetweenEvictionRunsMillis" value="${database.timeBetweenEvictionRunsMillis}"/>
		<property name="numTestsPerEvictionRun" value="${database.numTestsPerEvictionRun}"/>

<!--
	<bean id="entityManagerFactory"
		class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="jpaVendorAdapter">
			<bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
				<property name="showSql" value="true" />
				<property name="generateDdl" value="true" />
				<property name="databasePlatform" value="org.hibernate.dialect.PostgreSQLDialect" />
			</bean>
		</property>
		<property name="persistenceUnitName" value="config-store" />
	</bean>

	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="org.postgresql.Driver" />
		<property name="url" value="jdbc:postgresql://40.51.12.154:3330/Private Cert Repo"/>                         
		<property name="username" value="zyxg" />
		<property name="password" value="shdhhjj" />

		<property name="initialSize" value="${database.initialSize}"/>
		<property name="validationQuery" value="${database.validationQuery}"/>
		<property name="testWhileIdle" value="${database.testWhileIdle}"/>
		<property name="minIdle" value="${database.minIdle}"/>
		<property name="maxIdle" value="${database.maxIdle}"/>
		<property name="maxActive" value="${database.maxActive}"/>
		<property name="timeBetweenEvictionRunsMillis" value="${database.timeBetweenEvictionRunsMillis}"/>
		<property name="numTestsPerEvictionRun" value="${database.numTestsPerEvictionRun}"/>
	</bean>
-->

  <display-name>OHT_BUILD</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  


	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>		
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:resources/oht-context.xml</param-value>
	</context-param>
    
    
	
	<servlet>
		<servlet-name>Jersey REST Service</servlet-name>
		<servlet-class>com.sun.jersey.spi.container.servlet.ServletContainer</servlet-class>
		<init-param>
			<param-name>com.sun.jersey.config.property.packages</param-name>
			<param-value>com.ecw</param-value>
		</init-param>

		<load-on-startup>2</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>Jersey REST Service</servlet-name>
		<url-pattern>/rest/*</url-pattern>
	</servlet-mapping>
	<filter>
    <display-name>ApproveListController</display-name>
    <filter-name>ApproveListController</filter-name>
    <filter-class>approvelistendpoint.ApproveListController</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>ApproveListController</filter-name>
    <url-pattern>/*</url-pattern>
    <dispatcher>FORWARD</dispatcher>
    <dispatcher>REQUEST</dispatcher>
  </filter-mapping>
    
</web-app>


I want to remove below part using sed command

<servlet>
		<servlet-name>Jersey REST Service</servlet-name>
		<servlet-class>com.sun.jersey.spi.container.servlet.ServletContainer</servlet-class>
		<init-param>
			<param-name>com.sun.jersey.config.property.packages</param-name>
			<param-value>com.ecw</param-value>
		</init-param>

		<load-on-startup>2</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>Jersey REST Service</servlet-name>
		<url-pattern>/rest/*</url-pattern>
	</servlet-mapping>

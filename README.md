	<servlet>
		<servlet-name>ExecutionServiceInitializer</servlet-name>
		<display-name>Execution Service Initializer</display-name>
		<servlet-class>com.ecw.servlets.ExecutionServiceInitializer</servlet-class>
		<load-on-startup>4</load-on-startup>
	</servlet>
	

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




After:


<servlet>
		<servlet-name>ExecutionServiceInitializer</servlet-name>
		<display-name>Execution Service Initializer</display-name>
		<servlet-class>com.ecw.servlets.ExecutionServiceInitializer</servlet-class>
		<load-on-startup>4</load-on-startup>
	</servlet>
	
	<servlet>
		<servlet-name>CacheInitializer</servlet-name>
		<display-name>Cache Initializer</display-name>
		<servlet-class>com.ecw.servlets.CacheInitializer</servlet-class>
		<load-on-startup>7</load-on-startup>
	</servlet>

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








sed '/<\/servlet>/i \
\
\t<servlet> \
\t\t<servlet-name>CacheInitializer<\/servlet-name> \
\t\t<display-name>Cache Initializer<\/display-name> \
\t\t<servlet-class>com.ecw.servlets.CacheInitializer<\/servlet-class> \
\t\t<load-on-startup>7<\/load-on-startup> \
\t<\/servlet>' before_after_file.xml




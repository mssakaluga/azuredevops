jihu	<servlet>
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




sed '/<\/servlet>/a \
\
    <servlet> \
        <servlet-name>CacheInitializer<\/servlet-name> \
        <display-name>Cache Initializer<\/display-name> \
        <servlet-class>com.ecw.servlets.CacheInitializer<\/servlet


sed '/<\/servlet>/a \
\
    <servlet> \
        <servlet-name>CacheInitializer<\/servlet-name> \
        <display-name>Cache Initializer<\/display-name> \
        <servlet-class>com.ecw.servlets.CacheInitializer<\/servlet-class> \
        <load-on-startup>7<\/load-on-startup> \
    <\/servlet>' before_after_file.xml > output_file.xml





sed '$!N; /<\/servlet>/s/.*/&\n\
\
    <servlet> \
        <servlet-name>CacheInitializer<\/servlet-name> \
        <display-name>Cache Initializer<\/display-name> \
        <servlet-class>com.ecw.servlets.CacheInitializer<\/servlet-class> \
        <load-on-startup>7<\/load-on-startup> \
    <\/servlet>/' before_after_file.xml > output_file.xml



sed -e '/<\/servlet>/{:a;N;/<\/servlet>/!ba;r new_servlet_block.txt' -e '}' before_after_file.xml > output_file.xml




sed -e '/<\/servlet>/{:a;N;/<\/servlet>/!ba; \
\
    <servlet> \
        <servlet-name>CacheInitializer<\/servlet-name> \
        <display-name>Cache Initializer<\/display-name> \
        <servlet-class>com.ecw.servlets.CacheInitializer<\/servlet-class> \
        <load-on-startup>7<\/load-on-startup> \
    <\/servlet>' -e '}' before_after_file.xml > output_file.xml


sed '/<\/servlet>$/a <vaibhav>' web.xml





sed -i '/<load-on-startup>4<\/load-on-startup>/,/<\/servlet>/ s,\(</servlet>\),\1\n\t<servlet> \n\t\t\<servlet-name>CacheInitializer<\/servlet-name> \n\t\t\<display-name>Cache Initializer<\/display-name> \n\t\t\<servlet-class>com.ecw.servlets.CacheInitializer<\/servlet-class> \n\t\t\<load-on-startup>7<\/load-on-startup> \n\t\<\/servlet>,1' /eClinicalWorks/ecwinstall/detail/vaibhav/web.xml



command = "sed -i '/<load-on-startup>4<\\/load-on-startup>/,/<\\/servlet>/ s,\\(<\\/servlet>\\),\\1\\n\\t<servlet> \\n\\t\\t<servlet-name>CacheInitializer<\\/servlet-name> \\n\\t\\t<display-name>Cache Initializer<\\/display-name> \\n\\t\\t<servlet-class>com.ecw.servlets.CacheInitializer<\\/servlet-class> \\n\\t\\t<load-on-startup>7<\\/load-on-startup> \\n\\t<\\/servlet>,1' /eClinicalWorks/ecwinstall/detail/vaibhav/web.xml"

system(command)

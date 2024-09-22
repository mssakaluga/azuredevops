Before:

<bean id="serverInfoLoader"
		  class="com.ecw.ihub.commons.controller.ServerInfoLoader" init-method="init"
		  destroy-method="shutdown" scope="singleton">
		<property name="serverDetailsDao" ref="serverDetailsDao" />
		<property name="loggingManager" ref="loggingManager" />
	</bean>

	<bean id="appContextProvider" class="catalog.ApplicationContextProvider" scope="singleton">
	</bean>

	<util:properties id="velocityProperties">
		<prop key="resource.loader">class</prop>
		<prop key="class.resource.loader.class">org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader
		</prop>
		<prop key="runtime.log.logsystem.class">org.apache.velocity.runtime.log.SimpleLog4JLogSystem</prop>
		<prop key="runtime.log.logsystem.log4j.category">velocity</prop>
		<prop key="runtime.log.logsystem.log4j.logger">velocity</prop>
	</util:properties>

	<bean id="velocityEngine" class="org.apache.velocity.app.VelocityEngine">
		<constructor-arg ref="velocityProperties"/>
	</bean>

	<bean id="ceqCwDocumentTransactionLogsDAO" class="com.ecw.oht.dao.CeqCwDocumentQueryLogDAO" scope="prototype">
		<property name="serverInfoLoader" ref="serverInfoLoader"/>
	</bean>

	<bean id="iheDocumentDAO" class="com.ecw.oht.dao.IheDocumentDAO" scope="prototype"/>
		  
</beans>

After File:
<bean id="serverInfoLoader"
		  class="com.ecw.ihub.commons.controller.ServerInfoLoader" init-method="init"
		  destroy-method="shutdown" scope="singleton">
		<property name="serverDetailsDao" ref="serverDetailsDao" />
		<property name="loggingManager" ref="loggingManager" />
	</bean>

	<bean id="appContextProvider" class="catalog.ApplicationContextProvider" scope="singleton">
	</bean>

	<util:properties id="velocityProperties">
		<prop key="resource.loader">class</prop>
		<prop key="class.resource.loader.class">org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader
		</prop>
		<prop key="runtime.log.logsystem.class">org.apache.velocity.runtime.log.SimpleLog4JLogSystem</prop>
		<prop key="runtime.log.logsystem.log4j.category">velocity</prop>
		<prop key="runtime.log.logsystem.log4j.logger">velocity</prop>
	</util:properties>

	<bean id="velocityEngine" class="org.apache.velocity.app.VelocityEngine">
		<constructor-arg ref="velocityProperties"/>
	</bean>

	<bean id="ceqCwDocumentTransactionLogsDAO" class="com.ecw.oht.dao.CeqCwDocumentQueryLogDAO" scope="prototype">
		<property name="serverInfoLoader" ref="serverInfoLoader"/>
	</bean>

	<bean id="iheDocumentDAO" class="com.ecw.oht.dao.IheDocumentDAO" scope="prototype"/>

	<bean id="interopMilestoneConfigDao" class="com.ecw.ihub.dao.InteropMilestoneConfigDao" scope="prototype">
	</bean>
		
	<bean id="secFilterHelper" class="com.ecw.security.SecurityControlFilterHelper"></bean>
	
	<bean id="crossSite" class="com.ecw.security.SecurityControlWrapperFilter">
		<property name="secFilterHelper" ref="secFilterHelper"/>
	</bean>
	
	<bean id="logRequestInterceptorFilterMapping" class="com.ecw.security.LogRequestInterceptorFilterMapping"></bean>
</beans>

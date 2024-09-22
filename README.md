Before file:
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jaxws="http://cxf.apache.org/jaxws"
	xmlns:jaxrs="http://cxf.apache.org/jaxrs" xmlns:util="http://www.springframework.org/schema/util"
	xmlns:jms="http://www.springframework.org/schema/jms" xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans 	http://www.springframework.org/schema/beans/spring-beans.xsd 
	http://cxf.apache.org/jaxrs						http://cxf.apache.org/schemas/jaxrs.xsd
	http://cxf.apache.org/jaxws 					http://cxf.apache.org/schemas/jaxws.xsd
	http://www.springframework.org/schema/util 		http://www.springframework.org/schema/util/spring-util.xsd
	http://www.springframework.org/schema/jms 		http://www.springframework.org/schema/jms/spring-jms-3.0.xsd">

	<bean id="encryptedPropertyPlaceholder" class="com.ecw.datasource.DecryptPropertyConfigurer">
		<property name="locations">
			<list>
				<value>classpath:resources/ihub.properties</value>
				<value>classpath:oht.properties</value>
			</list>
		</property>
		<property name="placeholderPrefix" value="${"/>
		<property name="placeholderSuffix" value="}"/>
		<property name="propertiesToDecrypt" value="oht.DBUser, oht.DBPassword, oht.DBUrl,
		activemq.cda.consumer.username, activemq.cda.consumer.password, activemq.cda.consumer.brokerUrl,
		activemq.pdq.consumer.brokerUrl, activemq.pdq.consumer.username, activemq.pdq.consumer.password,
		activemq.audit.producer.brokerUrl, activemq.audit.producer.username, activemq.audit.producer.password,
		activemq.errornotify.producer.brokerUrl, activemq.errornotify.producer.username, activemq.errornotify.producer.password"/>
		<property name="beansToVisit" value="dataSource,consumer_connectionFactory,pdq_consumer_connectionFactory,audit_producer_connectionFactory,errornotify_producer_connectionFactory"/>
	</bean>

	<bean id="propertyConfigurer"
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="locations">
			<list>
				<value>classpath:resources/ihub.properties</value>
				<value>classpath:oht.properties</value>
			</list>
		</property>
	</bean>


	<!-- ******************** JMS Configuration START ******************** -->

					<!--********************CDA Consumer Start******************** -->
					
	 <bean id="consumer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		 p:userName="${activemq.cda.consumer.username}" p:password="${activemq.cda.consumer.password}" p:brokerURL="${activemq.cda.consumer.brokerUrl}" />

<!-- 	A POJO that implements the JMS message listener -->
 	<bean id="cdaConsumer" class="com.ecw.ihub.activemq.CDAConsumer" scope="prototype">
		<property name="ohtPassKey" value="${ihub.passKey}" />
		<property name="apuIdPassKey" value="${ihub.apuid.passkey}" />
		<property name="resourceId" value="${audit.resourceId}" />
		<property name="gson" ref="Gson" />
		<property name="logPnR" ref="PNRTransactionLogDAO" />
	</bean>

	<jms:listener-container container-type="default" connection-factory="consumer_connectionFactory" acknowledge="auto"
		concurrency="${activemq.cda.minConsumers.maxConsumers}" cache="consumer">
		<jms:listener destination="${activemq.cda.consumer.queueName}" ref="cdaConsumer" />
	</jms:listener-container> 
				
					<!-- ******************** CDA Consumer End ******************** -->

	<!--******************** PDQ Consumer Start ******************** -->
 
	<bean id="pdq_consumer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		p:userName="${activemq.pdq.consumer.username}" p:password="${activemq.pdq.consumer.password}" p:brokerURL="${activemq.pdq.consumer.brokerUrl}" />	

<!-- A POJO that implements the JMS message listener -->
	<bean id="pdqMessageListener" class="com.ecw.ihub.activemq.PDQConsumer" scope="prototype">
		<property name="ohtPassKey" value="${ihub.passKey}" />
		<property name="resourceId" value="${audit.resourceId}" />
		<property name="gson" ref="Gson" />
	</bean>

	<jms:listener-container container-type="default"
		connection-factory="pdq_consumer_connectionFactory" acknowledge="auto"
		concurrency="${activemq.pdq.minConsumers.maxConsumers}" cache="consumer">
		<jms:listener destination="${activemq.pdq.consumer.queueName}"
			ref="pdqMessageListener" />
	</jms:listener-container> 
	<!-- ******************** PDQ Consumer End ******************** -->

	<bean id="GenericMessageProducer" class="com.ecw.ihub.activemq.GenericMessageProducer" scope="prototype" />

	<!-- ********************Audit Producer******************** -->

	<bean id="auditProducerTemplate" class="org.springframework.jms.core.JmsTemplate" p:connectionFactory-ref="audit_producer_connectionFactory"
		p:defaultDestination-ref="destination_audit" />
		
	<bean id="audit_producer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		p:userName="${activemq.audit.producer.username}" p:password="${activemq.audit.producer.password}" p:brokerURL="${activemq.audit.producer.brokerUrl}" />		
		
	<bean id="destination_audit" class="org.apache.activemq.command.ActiveMQQueue">
		<constructor-arg value="${activemq.audit.producer.queueName}" />
	</bean>

	<!-- ********************END Audit Producer******************** -->

	<!--  ******************** JMS Configuration END ******************** -->

	<bean id="SendCDA" class="com.ecw.ihub.source.SendCDA" scope="prototype"/>
	
	<bean id="Gson" class="com.google.gson.Gson" scope="prototype" />
	
	<bean id="AuditBean" class="com.ecw.ihub.commons.beans.AuditBean" scope="prototype" />
	
	<bean id="PNRTransactionLogDAO" class="com.ecw.oht.dao.PnRTransactionLogDAO" scope="prototype" />
	
	<bean id="String" class="java.lang.String" scope="prototype" />

	<!-- ********************IG Error Notification******************** -->
	<bean id="errorNotifyProducerTemplate" class="org.springframework.jms.core.JmsTemplate"
		  p:connectionFactory-ref="errornotify_producer_connectionFactory"
		  p:defaultDestination-ref="destination_ihub"/>

	<bean id="errornotify_producer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		  p:userName="${activemq.errornotify.producer.username}" p:password="${activemq.errornotify.producer.password}"
		  p:brokerURL="${activemq.errornotify.producer.brokerUrl}"/>

	<bean id="destination_ihub" class="org.apache.activemq.command.ActiveMQQueue">
		<constructor-arg value="${activemq.errornotify.producer.queueName}"/>
	</bean>

	<bean id="igErrorNotificationProducer" class="com.ecw.ihub.activemq.IgErrorNotificationProducer" scope="prototype">
		<constructor-arg name="errorNotifyQueueTemplate" ref="errorNotifyProducerTemplate" />
		<constructor-arg name="ihubKey" value="${ihub.passKey}"/>
		<constructor-arg name="gson" ref="Gson"/>
		<constructor-arg name="messageProducer" ref="GenericMessageProducer"/>
	</bean>

	<!-- ********************END IG Error Notification******************** -->

	<!--********************Doc listing Consumer Start******************** -->

	<bean id="doc_metadata_consumer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		  p:userName="${activemq.doclisting.consumer.username}" p:password="${activemq.doclisting.consumer.password}" p:brokerURL="${activemq.doclisting.consumer.brokerUrl}" />

	<!-- 	A POJO that implements the JMS message listener -->
	<bean id="docMetadataConsumer" class="com.ecw.ihub.activemq.DocMetadataConsumer" scope="prototype">
		<constructor-arg name="pnrTransactionLogDao" ref="PNRTransactionLogDAO"/>
		<constructor-arg name="xdSbIti42Service" ref="XDSb_ITI_42_Service"/>
		<constructor-arg name="apuIdPassKey" value="${ihub.apuid.passkey}"/>
		<constructor-arg name="ohtPassKey" value="${ihub.passKey}"/>
	</bean>

	<bean name="XDSb_ITI_42_Service" class="ihe.oht.source.service.XDSb_ITI_42_Service" scope="prototype">
		<constructor-arg name="xdsb_iti_42" ref="XDSb_ITI_42"/>
	</bean>

	<bean name="XDSb_ITI_42" class="ihe.oht.source.api.XDSb_ITI_42" scope="prototype">
		<property name="ecwOID" value="${ecwOId}"/>
		<property name="epr" value="${iti42EndpointURL}"/>
		<property name="repositoryUniqueId" value="${repositoryUniqueId}"/>
		<property name="documentUniqueIdPrefix" value="${documentUniqueIdPrefix}"/>
	</bean>

	<jms:listener-container container-type="default" connection-factory="doc_metadata_consumer_connectionFactory" acknowledge="auto"
							concurrency="${activemq.doclisting.minConsumers.maxConsumers}" cache="consumer">
		<jms:listener destination="${activemq.doclisting.consumer.queueName}" ref="docMetadataConsumer" />
	</jms:listener-container>

	<!-- ******************** Doc Listing Consumer End ******************** -->


	<bean id="GetDocuments" class="com.ecw.ihub.consumer.GetDocuments" scope="prototype">
		<property name="ccrRequester" ref="CCRRequester"/>
		<property name="gson" ref="Gson" />
		<property name="errorNotificationProducer" ref="igErrorNotificationProducer"/>
		<property name="documentQueryLogDAO" ref="ceqCwDocumentTransactionLogsDAO" />
		<property name="iheDocumentDAO" ref="iheDocumentDAO" />
		<property name="storageRemote" 					value="${ihub.storage.remote}" />
		<property name="storageRemoteUrl" 				value="${ihub.storage.remote.url}" />
		<property name="storageRemoteContainer" 		value="${ihub.storage.remote.container}" />
		<property name="storageRemoteAccountName" 		value="${ihub.storage.remote.accountName}" />
		<property name="storageRemoteAccountKey" 		value="${ihub.storage.remote.accountKey}" />
	</bean>

	<bean id="CCRRequester" class="com.ecw.ihub.rest.client.CCRRequester" scope="prototype">
		<property name="eHXUrl" value="${ehx.ccrrequest.rest.endpoint}" />
	</bean>

	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="${oht.DBDriver}" />
		<property name="url" value="#{'${oht.DBUrl}' + '/' + '${oht.DBName}'}"/>
		<property name="username" value="${oht.DBUser}" />
		<property name="password" value="${oht.DBPassword}" />
	</bean>

	<bean id="serverDetailsDao" class="com.ecw.ihub.commons.dao.ServerDetailsDao" scope="prototype">
		<constructor-arg value="oht_server_details"></constructor-arg>
		<constructor-arg ref="dataSource"></constructor-arg>
	</bean>

	<bean id="loggingManager" class="com.ecw.ihub.commons.logging.LoggingManager" scope="singleton"></bean>

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


After file:

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jaxws="http://cxf.apache.org/jaxws"
	xmlns:jaxrs="http://cxf.apache.org/jaxrs" xmlns:util="http://www.springframework.org/schema/util"
	xmlns:jms="http://www.springframework.org/schema/jms" xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans 	http://www.springframework.org/schema/beans/spring-beans.xsd 
	http://cxf.apache.org/jaxrs						http://cxf.apache.org/schemas/jaxrs.xsd
	http://cxf.apache.org/jaxws 					http://cxf.apache.org/schemas/jaxws.xsd
	http://www.springframework.org/schema/util 		http://www.springframework.org/schema/util/spring-util.xsd
	http://www.springframework.org/schema/jms 		http://www.springframework.org/schema/jms/spring-jms-3.0.xsd">

	<bean id="encryptedPropertyPlaceholder" class="com.ecw.datasource.DecryptPropertyConfigurer">
		<property name="locations">
			<list>
				<value>classpath:resources/ihub.properties</value>
				<value>classpath:oht.properties</value>
			</list>
		</property>
		<property name="placeholderPrefix" value="${"/>
		<property name="placeholderSuffix" value="}"/>
		<property name="propertiesToDecrypt" value="oht.DBUser, oht.DBPassword, oht.DBUrl,
		activemq.cda.consumer.username, activemq.cda.consumer.password, activemq.cda.consumer.brokerUrl,
		activemq.pdq.consumer.brokerUrl, activemq.pdq.consumer.username, activemq.pdq.consumer.password,
		activemq.audit.producer.brokerUrl, activemq.audit.producer.username, activemq.audit.producer.password,
		activemq.errornotify.producer.brokerUrl, activemq.errornotify.producer.username, activemq.errornotify.producer.password"/>
		<property name="beansToVisit" value="dataSource,consumer_connectionFactory,pdq_consumer_connectionFactory,audit_producer_connectionFactory,errornotify_producer_connectionFactory"/>
	</bean>

	<bean id="propertyConfigurer"
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="locations">
			<list>
				<value>classpath:resources/ihub.properties</value>
				<value>classpath:oht.properties</value>
			</list>
		</property>
	</bean>


	<!-- ******************** JMS Configuration START ******************** -->

					<!--********************CDA Consumer Start******************** -->
					
	 <bean id="consumer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		 p:userName="${activemq.cda.consumer.username}" p:password="${activemq.cda.consumer.password}" p:brokerURL="${activemq.cda.consumer.brokerUrl}" />

<!-- 	A POJO that implements the JMS message listener -->
 	<bean id="cdaConsumer" class="com.ecw.ihub.activemq.CDAConsumer" scope="prototype">
		<property name="ohtPassKey" value="${ihub.passKey}" />
		<property name="apuIdPassKey" value="${ihub.apuid.passkey}" />
		<property name="resourceId" value="${audit.resourceId}" />
		<property name="gson" ref="Gson" />
		<property name="logPnR" ref="PNRTransactionLogDAO" />
	</bean>

	<jms:listener-container container-type="default" connection-factory="consumer_connectionFactory" acknowledge="auto"
		concurrency="${activemq.cda.minConsumers.maxConsumers}" cache="consumer">
		<jms:listener destination="${activemq.cda.consumer.queueName}" ref="cdaConsumer" />
	</jms:listener-container> 
				
					<!-- ******************** CDA Consumer End ******************** -->

	<!--******************** PDQ Consumer Start ******************** -->
 
	<bean id="pdq_consumer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		p:userName="${activemq.pdq.consumer.username}" p:password="${activemq.pdq.consumer.password}" p:brokerURL="${activemq.pdq.consumer.brokerUrl}" />	

<!-- A POJO that implements the JMS message listener -->
	<bean id="pdqMessageListener" class="com.ecw.ihub.activemq.PDQConsumer" scope="prototype">
		<property name="ohtPassKey" value="${ihub.passKey}" />
		<property name="resourceId" value="${audit.resourceId}" />
		<property name="gson" ref="Gson" />
	</bean>

	<jms:listener-container container-type="default"
		connection-factory="pdq_consumer_connectionFactory" acknowledge="auto"
		concurrency="${activemq.pdq.minConsumers.maxConsumers}" cache="consumer">
		<jms:listener destination="${activemq.pdq.consumer.queueName}"
			ref="pdqMessageListener" />
	</jms:listener-container> 
	<!-- ******************** PDQ Consumer End ******************** -->

	<bean id="GenericMessageProducer" class="com.ecw.ihub.activemq.GenericMessageProducer" scope="prototype" />

	<!-- ********************Audit Producer******************** -->

	
	<!-- ********************END Audit Producer******************** -->

	<!--  ******************** JMS Configuration END ******************** -->

	<bean id="SendCDA" class="com.ecw.ihub.source.SendCDA" scope="prototype"/>
	
	<bean id="Gson" class="com.google.gson.Gson" scope="prototype" />
	
	<bean id="AuditBean" class="com.ecw.ihub.commons.beans.AuditBean" scope="prototype" />
	
	<bean id="PNRTransactionLogDAO" class="com.ecw.oht.dao.PnRTransactionLogDAO" scope="prototype" />
	
	<bean id="String" class="java.lang.String" scope="prototype" />

	<!-- ********************IG Error Notification******************** -->
	<bean id="errorNotifyProducerTemplate" class="org.springframework.jms.core.JmsTemplate"
		  p:connectionFactory-ref="errornotify_producer_connectionFactory"
		  p:defaultDestination-ref="destination_ihub"/>

	<bean id="errornotify_producer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		  p:userName="${activemq.errornotify.producer.username}" p:password="${activemq.errornotify.producer.password}"
		  p:brokerURL="${activemq.errornotify.producer.brokerUrl}"/>

	<bean id="destination_ihub" class="org.apache.activemq.command.ActiveMQQueue">
		<constructor-arg value="${activemq.errornotify.producer.queueName}"/>
	</bean>

	<bean id="igErrorNotificationProducer" class="com.ecw.ihub.activemq.IgErrorNotificationProducer" scope="prototype">
		<constructor-arg name="errorNotifyQueueTemplate" ref="errorNotifyProducerTemplate" />
		<constructor-arg name="ihubKey" value="${ihub.passKey}"/>
		<constructor-arg name="gson" ref="Gson"/>
		<constructor-arg name="messageProducer" ref="GenericMessageProducer"/>
	</bean>

	<!-- ********************END IG Error Notification******************** -->

	<!--********************Doc listing Consumer Start******************** -->

	<bean id="doc_metadata_consumer_connectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory"
		  p:userName="${activemq.doclisting.consumer.username}" p:password="${activemq.doclisting.consumer.password}" p:brokerURL="${activemq.doclisting.consumer.brokerUrl}" />

	<!-- 	A POJO that implements the JMS message listener -->
	<bean id="docMetadataConsumer" class="com.ecw.ihub.activemq.DocMetadataConsumer" scope="prototype">
		<constructor-arg name="pnrTransactionLogDao" ref="PNRTransactionLogDAO"/>
		<constructor-arg name="xdSbIti42Service" ref="XDSb_ITI_42_Service"/>
		<constructor-arg name="apuIdPassKey" value="${ihub.apuid.passkey}"/>
		<constructor-arg name="ohtPassKey" value="${ihub.passKey}"/>
	</bean>

	<bean name="XDSb_ITI_42_Service" class="ihe.oht.source.service.XDSb_ITI_42_Service" scope="prototype">
		<constructor-arg name="xdsb_iti_42" ref="XDSb_ITI_42"/>
	</bean>

	<bean name="XDSb_ITI_42" class="ihe.oht.source.api.XDSb_ITI_42" scope="prototype">
		<property name="ecwOID" value="${ecwOId}"/>
		<property name="epr" value="${iti42EndpointURL}"/>
		<property name="repositoryUniqueId" value="${repositoryUniqueId}"/>
		<property name="documentUniqueIdPrefix" value="${documentUniqueIdPrefix}"/>
	</bean>

	<jms:listener-container container-type="default" connection-factory="doc_metadata_consumer_connectionFactory" acknowledge="auto"
							concurrency="${activemq.doclisting.minConsumers.maxConsumers}" cache="consumer">
		<jms:listener destination="${activemq.doclisting.consumer.queueName}" ref="docMetadataConsumer" />
	</jms:listener-container>

	<!-- ******************** Doc Listing Consumer End ******************** -->


	<bean id="GetDocuments" class="com.ecw.ihub.consumer.GetDocuments" scope="prototype">
		<property name="ccrRequester" ref="CCRRequester"/>
		<property name="gson" ref="Gson" />
		<property name="errorNotificationProducer" ref="igErrorNotificationProducer"/>
		<property name="documentQueryLogDAO" ref="ceqCwDocumentTransactionLogsDAO" />
		<property name="iheDocumentDAO" ref="iheDocumentDAO" />
		<property name="storageRemote" 					value="${ihub.storage.remote}" />
		<property name="storageRemoteUrl" 				value="${ihub.storage.remote.url}" />
		<property name="storageRemoteContainer" 		value="${ihub.storage.remote.container}" />
		<property name="storageRemoteAccountName" 		value="${ihub.storage.remote.accountName}" />
		<property name="storageRemoteAccountKey" 		value="${ihub.storage.remote.accountKey}" />
		<property name="interopMilestoneConfigDao" ref="interopMilestoneConfigDao" />
	</bean>

	<bean id="CCRRequester" class="com.ecw.ihub.rest.client.CCRRequester" scope="prototype">
		<property name="eHXUrl" value="${ehx.ccrrequest.rest.endpoint}" />
	</bean>

	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="${oht.DBDriver}" />
		<property name="url" value="#{'${oht.DBUrl}' + '/' + '${oht.DBName}'}"/>
		<property name="username" value="${oht.DBUser}" />
		<property name="password" value="${oht.DBPassword}" />
	</bean>

	<bean id="serverDetailsDao" class="com.ecw.ihub.commons.dao.ServerDetailsDao" scope="prototype">
		<constructor-arg value="oht_server_details"></constructor-arg>
		<constructor-arg ref="dataSource"></constructor-arg>
	</bean>

	<bean id="loggingManager" class="com.ecw.ihub.commons.logging.LoggingManager" scope="singleton"></bean>

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


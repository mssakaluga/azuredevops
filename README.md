Before file:
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

After file:

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

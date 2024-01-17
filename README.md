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

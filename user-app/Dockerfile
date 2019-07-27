FROM payara/5.192

# Initialize the configurable environment variables
ENV HOME_DIR=/opt/payara\
    PAYARA_DIR=/opt/payara/appserver\
    PAYARA_LIB_DIR=/opt/payara/appserver/glassfish/lib\
    SCRIPT_DIR=/opt/payara/scripts\
    CONFIG_DIR=/opt/payara/config\
    DEPLOY_DIR=/opt/payara/deployments\
    PASSWORD_FILE=/opt/payara/passwordFile\
    # Payara Server Domain options
    DOMAIN_NAME=production\
    ADMIN_USER=admin\
    ADMIN_PASSWORD=admin \
    # Utility environment variables
    JVM_ARGS=\
    PAYARA_ARGS=\
    DEPLOY_PROPS=\
    POSTBOOT_COMMANDS=/opt/payara/config/post-boot-commands.asadmin\
    PREBOOT_COMMANDS=/opt/payara/config/pre-boot-commands.asadmin
ENV PATH="${PATH}:${PAYARA_DIR}/bin"
COPY --chown=payara:payara lib/* ${PAYARA_LIB_DIR}/

RUN ${PAYARA_DIR}/bin/asadmin --user=${ADMIN_USER} --passwordfile=${PASSWORD_FILE} start-domain ${DOMAIN_NAME} &&\
${PAYARA_DIR}/bin/asadmin --user=${ADMIN_USER} --passwordfile=${PASSWORD_FILE} create-jdbc-connection-pool \
--validateAtmostOncePeriodInSeconds=0 \
--sqlTraceListeners=fish.payara.jdbc.SilentSqlTraceListener  \
--statementLeakReclaim=false  \
--ping=false  \
--driverClassname=  \
--maxWaitTimeInMillis=60000  \
--wrapJdbcObjects=true  \
--connectionValidationMethod=table  \
--steadyPoolSize=8  \
--idleTimeoutInSeconds=300  \
--maxConnectionUsageCount=0  \
--matchConnections=false  \
--connectionCreationRetryIntervalInSeconds=10  \
--associateWithThread=false  \
--isConnectionValidationRequired=false  \
--connectionLeakReclaim=false  \
--datasourceClassname=com.mysql.jdbc.jdbc2.optional.MysqlConnectionPoolDataSource  \
--connectionCreationRetryAttempts=0  \
--statementCacheSize=0  \
--resType=javax.sql.ConnectionPoolDataSource  \
--statementLeakTimeoutInSeconds=0  \
--failAllConnections=false  \
--statementTimeoutInSeconds=-1  \
--lazyConnectionEnlistment=false  \
--pooling=true  \
--poolResizeQuantity=2  \
--allowNonComponentCallers=false  \
--connectionLeakTimeoutInSeconds=0  \
--isIsolationLevelGuaranteed=true  \
--maxPoolSize=32  \
--nonTransactionalConnections=false  \
--lazyConnectionAssociation=false user-pool  && \
${PAYARA_DIR}/bin/asadmin --user=${ADMIN_USER} --passwordfile=${PASSWORD_FILE} set resources.jdbc-connection-pool.user-pool.property.password=demo \
resources.jdbc-connection-pool.user-pool.property.user=tom \
resources.jdbc-connection-pool.user-pool.property.databaseName=UserDB \
resources.jdbc-connection-pool.user-pool.property.serverName=192.168.189.33 \
resources.jdbc-connection-pool.user-pool.property.portNumber=3306 && \
${PAYARA_DIR}/bin/asadmin --user=${ADMIN_USER} --passwordfile=${PASSWORD_FILE} create-jdbc-resource --description="User DB" --enabled=true --poolName=user-pool --target=domain jdbc/userdb &&\
${PAYARA_DIR}/bin/asadmin --user=${ADMIN_USER} --passwordfile=${PASSWORD_FILE} create-resource-ref --enabled=true --target=server jdbc/userdb && \
${PAYARA_DIR}/bin/asadmin --user=${ADMIN_USER} --passwordfile=${PASSWORD_FILE} stop-domain

CMD ${SCRIPT_DIR}/init_1_generate_deploy_commands.sh && exec ${SCRIPT_DIR}/startInForeground.sh

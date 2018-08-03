# Docker Java/MySQL Tomcat Sample
This is a simple Java application with MySQL.

# Provision MySQL 5.7 Pod
Database user name : dbadmin
Database password : dbpassword
Root password : password
Database Name : testdb

# Connect to MySQL Pod through pod terminal and excute below commands :
$mysql -u dbadmin -p dbpassword
$use testdb;
$create table testdata (id int not null auto_increment primary key,foo varchar(25),bar int);
$insert into testdata values(null, 'hello', 12345);

# Edit and change mysql connection ip to cluster service ip and ensure correct database connection values in dbtest/META-INF/context.xml file
  Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
               maxActive="100" maxIdle="30" maxWait="10000"
               username="dbadmin" password="dbpassword" driverClassName="com.mysql.jdbc.Driver"
               url="jdbc:mysql://<internal_mysql_service_ip>:3306/testdb"
               
# Check Dockerfile and ensure Imagestream/Image is available and accessible. If required change the FROM value to appropriate Imagestream/Tag :
FROM registry.access.redhat.com/jboss-webserver-3/webserver31-tomcat8-openshift


# Login through oc cli to openshift and change to project
$oc login https://<master-api-server-ip>:<port(8443)>/ -u <user> -p <password>
$oc project tomcat-mysql-demo <or project name where mysql pod was provisioned>
$oc new-app http://<git_server_repo_url> --strategy=docker
$oc expose service <service-name(tomcat-mysql)>
  
# Test application
Access the application url via browser and it should list the values inserted into database. Add more values through mysql terminal and refresh browser page.

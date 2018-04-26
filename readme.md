# Spring PetClinic Sample Application
This is a fork of Sping Pet Clinic. 
Please refer https://github.com/spring-projects/spring-petclinic for the details of the project.

## Changes in the fork. 
   This fork dockerized the GCP application. 
   Default application is configured to use MySQL on GCP Cloud. Database drivers dependencis are changed.
   Kubernetes specs have been provided to run on k8s
   
   Note: You can change the application.properties and run with another MYSQL and run this Docker Container.
 
## Setup GCP Database and Enviornment.

1) Login to GCP Portal and create a project ttjohn-petclinic
2) Create Cloud SQL using the command "gcloud sql instances create pet-instance"
3) Create a database using "gcloud sql databases create petclinic --instance pet-instance"
4) Find the connection name using "gcloud beta sql instances describe pet-instance |grep connectionName"

## Running petclinic in GCP with out container
Change the mqsql connection name in the application-mysql.properties.
```
	git clone https://github.com/spring-projects/spring-petclinic.git
	cd spring-petclinic
	./mvnw -DskipTests spring-boot:run
 
```
You can then access petclinic via webpreview of GCP Cloud shell

## Running petclinic in GCP in docker

Use the following commands to build, push and run the docker images.  
You need to enable the Google Container API for the project to push the images in the following link
https://console.cloud.google.com/apis/api/containerregistry.googleapis.com/overview?project=<your-project-id>

```
	./mvnw -DskipTests install dockerfile:build
        ./mvnw -DskipTests dockerfile:push
        docker run -p 8080:8080 -t gcr.io/ttjohn-petclinic/spring-petclinic:2.0.0
 
```
You can view the application view web preview in port 8080

## POM Changes for Cloud SQL

Following are the pom changes already made to run with Cloud SQL

```
  <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" ...>
    ...
    <!-- Add Spring Cloud GCP Dependency BOM -->
    <dependencyManagement>
        <dependencies>
          <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-gcp-dependencies</artifactId>
          <version>1.0.0.M2</version>
          <type>pom</type>
          <scope>import</scope>
          </dependency>
      </dependencies>
    </dependencyManagement>
    <dependencies>
      ...
      <!-- Add CloudSQL Starter for MySQL -->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-gcp-starter-sql-mysql</artifactId>
      </dependency>
      ...
    </dependencies>
    <repositories>
      <!-- Use Spring Milestone Repository -->
      <repository>
        <id>repository.spring.milestone</id>
        <name>Spring Milestones Repository</name>
        <url>http://repo.spring.io/milestone</url>
      </repository>
    </repositories>
 
```
Change the src/main/resources/application-myql.properties as follows 

```
database=mysql

# Delete the rest of the original content of the file and replace with the following:
spring.cloud.gcp.sql.database-name=petclinic
spring.cloud.gcp.sql.instance-connection-name=YOUR_CLOUD_SQL_INSTANCE_CONNECTION_NAME

# Initialize the database since the newly created Cloud SQL database has no tables. The following flag is for Spring Boot 2.
spring.datasource.initialization-mode=always
 
```
Change the src/main/resources/application.properties as follows 

```
# Keep the content of the file the same
...

# In the last line, add mysql to the spring.profiles.active property
spring.profiles.active=mysql
 
```

## Changes for creating docker images for GCP

Added the Docker file
Changed the pom.xml for the maven plug-in to build docker images.
```
	
<properties>
   <docker.image.prefix>your gcp project name</docker.image.prefix>
</properties>

<build>
    <plugins>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>dockerfile-maven-plugin</artifactId>
            <version>1.3.6</version>
            <configuration>
                <repository>gcr.io/${docker.image.prefix}/${project.artifactId}</repository>
                 <tag>${project.version}</tag>
                <buildArgs>
                    <JAR_FILE>target/${project.build.finalName}.jar</JAR_FILE>
                </buildArgs>
            </configuration>
        </plugin>
    </plugins>
</build>
 
```


# License

The Spring PetClinic sample application is released under version 2.0 of the [Apache License](http://www.apache.org/licenses/LICENSE-2.0).

[spring-petclinic]: https://github.com/spring-projects/spring-petclinic
[spring-framework-petclinic]: https://github.com/spring-petclinic/spring-framework-petclinic
[spring-petclinic-angularjs]: https://github.com/spring-petclinic/spring-petclinic-angularjs 
[javaconfig branch]: https://github.com/spring-petclinic/spring-framework-petclinic/tree/javaconfig
[spring-petclinic-angular]: https://github.com/spring-petclinic/spring-petclinic-angular
[spring-petclinic-microservices]: https://github.com/spring-petclinic/spring-petclinic-microservices
[spring-petclinic-reactjs]: https://github.com/spring-petclinic/spring-petclinic-reactjs
[spring-petclinic-graphql]: https://github.com/spring-petclinic/spring-petclinic-graphql
[spring-petclinic-kotlin]: https://github.com/spring-petclinic/spring-petclinic-kotlin
[spring-petclinic-rest]: https://github.com/spring-petclinic/spring-petclinic-rest

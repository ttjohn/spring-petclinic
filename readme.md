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

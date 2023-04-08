Jenkins Pipeline for Java-springboot based application using Maven, SonarQube, Docker, Argo CD and Kubernetes
-------------------------------------------------------------------------------------------------------------------
![image](https://user-images.githubusercontent.com/111578142/230550209-1edd036f-5b63-4f86-8f7f-e3af3a9dbbdd.png)

Here are the step-by-step details to set up an end-to-end Jenkins pipeline for a Java application using SonarQube , Docker, Argo CD and Kubernetes:

Prequesites:

* Java application code hosted on a Git repository
* Ec2 server with Jenkins,Sonarqube and Docker Installed.(t2.medium or above)
* Kubernetes cluster (For testing purpose Minikube is also fine)
* Argo CD controller deployed on K8s cluster using K8s Operators.

Steps:

1. Jenkins plugins to Install:
---------------------------------
   1.1 Github plugin
   
   1.2 Maven Integration plugin
   
   1.3 Docker
   
   1.4 Docker Pipeline plugin
   
   1.5 Sonarqube plugin
   
   1.6 Salck Notifications
      
2. Create a new Jenkins pipeline:

   2.1 In Jenkins, create a new pipeline job and configure it with the Git repository URL for the Java application. 
   
   2.3 Add a Jenkinsfile to the Git repository to define the pipeline stages.
   
   2.3 Add docker-creds(Username & Password), github(API Token) and sonarqube token in jenkins. (Manage Jenkins >>  Manage credentails >> Add credentials) 

3. Define the pipeline stages:

    Stage 1: Checkout the source code from Git.
    
    Stage 2: Build the Java application using Maven.
    
    Stage 3: Run unit tests using JUnit and maven targets like mvn test && mvn verify.
    
    Stage 4: Run SonarQube analysis to check the code quality.
    
    Stage 5: Package the application into a JAR file.
    
    Stage 6: Use the Jar file to build the docker image using the dockerfile specified in the repository.
    
    Satge7: Update the docker image tag using shell script to update the image tag in k8s manifest file.
    
    Stage 7: Promote the application to a production environment using Argo CD.

4. Configure Jenkins pipeline stages:

    Stage 1: Use the Git plugin to check out the source code from the Git repository.
    
    Stage 2: Use the Maven Integration plugin to build the Java application.
    
    Stage 3: Use the JUnit and Mockito plugins to run unit tests.
    Stage 4: Use the SonarQube plugin to analyze the code quality of the Java application.
    Stage 5: Use the Maven Integration plugin to package the application into a JAR file.
    Stage 6: Use the Kubernetes Continuous Deploy plugin to deploy the application to a test environment using Helm.
    Stage 7: Use a testing framework like Selenium to run user acceptance tests on the deployed application.
    Stage 8: Use Argo CD to promote the application to a production environment.

5. Set up Argo CD:

    5.1 Install and configure Argo CD using K8s opeartors on the Kubernetes cluster.
           https://operatorhub.io/operator/argocd-operatorusing -- Recommended way of doing
            https://medium.com/@mehmetodabashi/installing-argocd-on-minikube-and-deploying-a-test-application-caa68ec55fbf   -- Simplest way of doing
    5.2 Set up a Git repository for Argo CD to track the changes and sync the Kubernetes manifests accordingly.
    

7. Run the Jenkins pipeline:

   7.1 Configure Github webhooks which will automatically detect the change in src code and trigger the Jenkins pipeline to start the CI/CD process for the Java application.
   7.2 Monitor the pipeline stages and fix any issues that arise.



# Spring Boot based Java web application
 
This is a simple Sprint Boot based Java application that can be built using Maven. Sprint Boot dependencies are handled using the pom.xml 
at the root directory of the repository.

This is a MVC architecture based application where controller returns a page with title and message attributes to the view.

## Execute the application locally and access it using your browser

Checkout the repo and move to the directory

```
git clone https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s/sprint-boot-app
cd java-maven-sonar-argocd-helm-k8s/sprint-boot-app
```

Execute the Maven targets to generate the artifacts

```
mvn clean package
```

The above maven target stroes the artifacts to the `target` directory. You can either execute the artifact on your local machine
(or) run it as a Docker container.

** Note: To avoid issues with local setup, Java versions and other dependencies, I would recommend the docker way. **


### Execute locally (Java 11 needed) and access the application on http://localhost:8080

```
java -jar target/spring-boot-web.jar
```

### The Docker way

Build the Docker Image

```
docker build -t ultimate-cicd-pipeline:v1 .
```

```
docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
```

Hurray !! Access the application on `http://<ip-address>:8010`


## Next Steps

### Configure a Sonar Server locally

```
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```

Hurray !! Now you can access the `SonarQube Server` on `http://<ip-address>:9000` 


Happy Learning😊😊


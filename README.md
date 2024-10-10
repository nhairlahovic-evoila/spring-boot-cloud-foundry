# Spring Boot Cloud Foundry Deployment Demo

This project demonstrates how to deploy a Spring Boot application to Cloud Foundry using the provided `manifest.yml`.

## Prerequisites
- **Cloud Foundry CLI**: Make sure you have the Cloud Foundry CLI installed.
- **Cloud Foundry Account**: You must have access to a Cloud Foundry environment.
- **Java 17**: Ensure that Java 17 is installed on your machine.

## Deployment Steps

### 1. Install Cloud Foundry CLI

To get started, you need to install the Cloud Foundry CLI tool. Please refer to the [official documentation](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) for instructions on installing the CLI:

After installation, verify the installation and check the version by running:

```bash
cf version
```

Example output:

```bash
cf version 6.26.0+9c9a261fd.2017-04-06
```

### 2. Log in to Cloud Foundry

Once the CLI is installed, log in to your Cloud Foundry environment by running the following command:

```bash
cf login --skip-ssl-validation
```

You will be prompted to enter your credentials.

Example:
```bash
API endpoint: api.example.io
Email> emailaddress@domain.com
Password>
```

After successful authentication, you will see confirmation of the targeted organization and space:
```bash
Authenticating...
OK

Targeted org <yourorganization>
Targeted space development

API endpoint:   https://api.run.pivotal.io (API version: 2.101.0)
User:           emailaddress@domain.com
Org:            <yourorganization>
Space:          development
```

You are now logged in and ready to deploy your Spring Boot application.

### 3. Clone the Repository and Initialize Submodule

If you haven't already, clone the repository that contains the Spring Boot application and initialize the submodules:

```bash
git clone https://github.com/nhairlahovic-evoila/spring-boot-cloud-foundry-demo.git
cd spring-boot-cloud-foundry-demo
git submodule update --init
```

This will ensure that all necessary submodules, including the greeting service, are initialized and updated in the repository.

### 4. Build the Application

Navigate to the `greeting-service` directory and build the Spring Boot application using Maven:

```bash
cd greeting-service
./mvnw clean package
```

This will generate the JAR file `greeting-service-0.0.1-SNAPSHOT.jar` in the `target` directory.

### 5. Deploy to Cloud Foundry

Once the JAR file is built, navigate to the root directory where the `manifest.yml` file is located, and deploy the application to Cloud Foundry using the provided `manifest.yml`:
```bash
cf push
```

This command will:
- Use the configuration in `manifest.yml`: Cloud Foundry will automatically use the settings specified in the manifest for deployment.
- Deploy the `greeting-service-0.0.1-SNAPSHOT.jar`: The JAR file from the `greeting-service/target/` directory will be deployed.
- Set Java Runtime to Version 17: The `JBP_CONFIG_OPEN_JDK_JRE` environment variable in the manifest ensures that Java 17 is used for the application.
- Define the Environment Variable `GREETING`: The `GREETING` environment variable is set to "Hi", which will be used by the Spring Boot application to display the greeting message.

### 6. Access the Application

After a successful deployment, Cloud Foundry will provide a URL to access the application. You can use the following command to check the app's routes:

```bash
cf apps
```

The app will be accessible via the route `http://spring-app-demo.<your-cloud-foundry-domain>/`.


### 7. Managing Your Application

Here are some useful commands for managing your deployed Spring Boot application on Cloud Foundry:

**1. Scaling the Application**
To scale the number of instances running your application, modify the instances value in the `manifest.yml` file:
```yaml
instances: 2
```

Then redeploy the app:
```bash
cf push
```

**2. Monitoring and Viewing Logs**

To monitor your application's logs, use the following command:
```bash
cf logs spring-app-demo --recent
```

This will display recent logs and help you debug or monitor the app’s performance.

**3. Cleaning Up**
To delete the application from Cloud Foundry, run:

```bash
cf delete spring-app-demo
```

This will stop and remove the application from your Cloud Foundry environment.

















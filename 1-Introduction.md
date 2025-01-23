# Continuous Integration (CI) on AWS Cloud

## **Overview**
In an Agile SDLC environment, developers frequently update and test code as part of their workflow. However, tracing errors quickly during frequent commits can become challenging. To address this, **Continuous Integration (CI)** ensures that every code commit is automatically tested, providing early detection of issues and maintaining code quality.

This project implements CI using AWS Cloud services, creating an automated pipeline for code integration, testing, and artifact management.

---

## **Solution**
We leverage **Continuous Integration (CI)** to ensure that every commit undergoes automated testing and quality checks. CI allows us to streamline the development process by reducing manual intervention and identifying issues early.

### **Services Used**
- **Bitbucket**: Code repository for source control.
- **AWS CodeArtifact**: Manages Maven dependencies.
- **AWS CodeBuild**: Builds code and artifacts.
- **SonarCloud**: Performs static code analysis.
- **AWS S3**: Stores generated artifacts.
- **AWS SNS**: Sends notifications to developers.
- **AWS CodePipeline**: Connects all services to automate the CI workflow.

---

## **Workflow**
1. Developers push code to the **Bitbucket repository**.
2. **AWS CodePipeline** is triggered on every commit.
3. **SonarCloud** analyzes the code and runs tests to ensure quality.
4. **AWS CodeArtifact** provides Maven dependencies required for building the code.
5. If the code passes the quality checks, **AWS CodeBuild** executes `mvn build` to generate artifacts.
6. The generated artifact is stored in an **S3 bucket**.
7. Notifications about the pipeline's status are sent via **AWS SNS**.

---

## **Implementation Steps**

### **1. Bitbucket Repository**
- Create an account on Bitbucket and set up a repository.
- Configure SSH authentication for secure communication between your local environment and the repository.
- Migrate the **vProfile** project source code from GitHub to Bitbucket.

### **2. AWS CodeArtifact**
- Create a **CodeArtifact** repository to store Maven dependencies.
- Update `pom.xml` and `settings.xml` files to include CodeArtifact repository details.
- Review the **buildspec.yml** file for configurations related to dependency resolution.

### **3. SonarCloud Setup**
- Create a SonarCloud account and set up project details.
- Integrate SonarCloud with the pipeline to perform code analysis on every commit.

### **4. AWS Parameter Store**
- Store sensitive details like **SonarCloud tokens** securely in AWS Parameter Store.
- Reference Parameter Store details in the `buildspec.yml` file.

### **5. AWS CodeBuild Job for Code Analysis**
- Understand and configure the **buildspec.yml** file for the code analysis job.
- Update `pom.xml` and `settings.xml` files to reference the CodeArtifact repository.
- Create a CodeBuild job specifically for SonarCloud code analysis.

### **6. Build Job for Artifact Creation**
- Configure the **buildspec.yml** file for building artifacts.
- Create an S3 bucket to store the generated artifacts.
- Create an AWS CodeBuild job to build the code and store artifacts in S3.
- Test and verify the build process.

### **7. AWS CodePipeline**
- Set up **SNS notifications** to alert the team about pipeline status.
- Create an **AWS CodePipeline** to automate the integration of all services.
- Test the complete pipeline to ensure smooth execution.

---

## **Benefits**
- **Automation**: Reduces manual intervention, ensuring consistent code quality.
- **Early Detection**: Identifies issues early, reducing debugging time.
- **Scalability**: Leverages AWS services to handle increasing workloads seamlessly.
- **Notifications**: Keeps the team informed of pipeline status through SNS alerts.

---

## **Conclusion**
This project demonstrates how Continuous Integration (CI) can be effectively implemented on the cloud using AWS services. By automating code testing, analysis, and artifact generation, we ensure a faster and more reliable development workflow. The modular design of the pipeline also makes it easy to adapt and scale as project requirements grow.

---

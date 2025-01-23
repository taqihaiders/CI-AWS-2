# SonarCloud and AWS Parameter Store Setup

## **1. SonarCloud Setup**

### **Steps to Configure SonarCloud**
1. **Login and Generate Token**:
   - Log in to your [SonarCloud account](https://sonarcloud.io/).
   - Navigate to the security section and generate a token. For example:
     ```
     Token: d9a7c5d05f1284643463b69f0d088c90b6732949
     ```

2. **Create an Organization**:
   - Manually create a new organization on SonarCloud.
   - Follow the prompts to set up your organization.

3. **Analyze a New Project**:
   - Once the organization is created, select **Analyze a New Project**.
   - Create a new project under the organization.
   - Go to the **Project Information** section and:
     - Copy the **Project Key**.
     - Copy the **Organization Name**.
   - Save these details securely for later use in the pipeline.

---

## **2. AWS Parameter Store Setup**

### **Purpose of Parameter Store**
To ensure sensitive information (e.g., tokens, credentials, keys) is not accidentally exposed in your pipeline, we use AWS Parameter Store. By doing so, we securely store secrets and access them in the build process through environment variables.

### **Steps to Configure Parameter Store**
1. **Access AWS Systems Manager**:
   - Go to the AWS Management Console and navigate to **Systems Manager**.
   - Locate **Parameter Store** in the left-hand menu.

2. **Create Parameters**:
   - For each secret (e.g., SonarCloud tokens, project keys, organization name):
     - Click **Create Parameter**.
     - Choose a parameter name (e.g., `/ci/sonarcloud/token`).
     - Set the type to **SecureString**.
     - Enter the corresponding value (e.g., the token or project key).
     - Save the parameter.

3. **Reference Parameters in `buildspec.yml`**:
   - In your `buildspec.yml` file, replace sensitive information with placeholders. For example:
     ```yaml
     env:
       variables:
         SONAR_TOKEN: "/ci/sonarcloud/token"
         PROJECT_KEY: "/ci/sonarcloud/project_key"
         ORG_NAME: "/ci/sonarcloud/org_name"
     ```
   - The pipeline will retrieve these values from Parameter Store during execution.

---

## **Benefits of Using Parameter Store**
- **Enhanced Security**: Secrets are encrypted and securely stored.
- **Centralized Management**: Manage all sensitive information in one place.
- **Easy Integration**: AWS services like CodeBuild and CodePipeline natively support Parameter Store.

---

## **Conclusion**
By setting up SonarCloud and AWS Parameter Store, we ensure that code quality analysis is integrated into our pipeline while safeguarding sensitive information. SonarCloud provides insights into code quality, and Parameter Store ensures a secure and efficient way to manage secrets in our CI/CD process.

---

## **Key Links**
- [SonarCloud Documentation](https://sonarcloud.io/documentation)
- [AWS Parameter Store Documentation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)


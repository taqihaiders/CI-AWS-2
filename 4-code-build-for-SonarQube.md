# AWS CodeBuild Setup for Sonar Analysis

## **1. Modify `pom.xml` and `settings.xml`**

### **Steps to Update Repository URL**
1. Go to **AWS CodeArtifact**:
   - Select the `maven-central-repo` repository.
   - Click on **View Repository Connections**.
   - Select **Maven** and copy the provided URL.

2. Update `pom.xml`:
   - Open the `pom.xml` file in your project.
   - Replace the existing repository URL with the one copied from CodeArtifact.

3. Update `settings.xml`:
   - Open the `settings.xml` file.
   - Replace the repository URL with the same URL copied earlier.

---

## **2. Modify the `buildspec.yml` File**

### **Steps to Update `buildspec.yml`**
1. **Move Buildspec File**:
   - Drag the `buildspec.yml` file to the root level of your project directory.
   - Ensure the file is named `buildspec.yml`.

2. **Update Export Command**:
   - Replace the `export` command with the **CodeArtifact Authorization Token** command:
     - Go to **CodeArtifact** and locate the command under **Export a CodeArtifact Authorization Token**.
     - Copy the token command and paste it into the `buildspec.yml` file.

3. **Commit Changes**:
   - Save the changes, commit them, and push the updated files to the Bitbucket repository:
     ```bash
     git add buildspec.yml pom.xml settings.xml
     git commit -m "Updated buildspec and Maven repo details"
     git push origin <branch-name>
     ```

---

## **3. Create a CodeBuild Project**

### **Steps to Set Up CodeBuild**
1. Go to **AWS CodeBuild** and click **Create Project**.
2. Provide a **Project Name**.
3. Select **Source** as **Bitbucket**:
   - Connect your Bitbucket account.
   - Select the repository containing your project.
4. Configure Build Environment:
   - Choose **Ubuntu** as the build server.
   - Select **Use Buildspec File** since the file is already in the root of your project.
5. Leave other settings as default and click **Create Project**.

---

## **4. Fix Permissions for Parameter Store**

### **Why Permissions are Needed**:
During the first attempt, the CodeBuild process may fail due to insufficient permissions to access the AWS Parameter Store.

### **Steps to Fix Permissions**:
1. **Create an IAM Policy**:
   - Go to the **IAM** service and create a new policy.
   - Select **Systems Manager** as the service.
   - Configure the policy to grant access to **Parameter Store** (choose specific actions and resources as needed).
   - Name the policy and save it.

2. **Attach Policy to CodeBuild Role**:
   - Go to **IAM Roles**.
   - Locate the role created for your CodeBuild project.
   - Attach the newly created Parameter Store policy.
   - Additionally, attach the **CodeArtifact ReadOnlyAccess** policy.

---

## **5. Running a CodeBuild**

### **Steps to Start a Build**
1. Go to your **CodeBuild Project** and click **Start Build**.
2. Monitor the build process:
   - On the first attempt, the build may fail due to issues in the `buildspec.yml` file or missing permissions.
   - Fix any errors (e.g., buildspec syntax issues) and retry the build.

3. Once resolved, the build should complete successfully.

---

## **Outcome**
- The build process was successfully completed after resolving initial errors in the `buildspec.yml` file and setting up correct permissions.
- The project integrates seamlessly with AWS CodeArtifact and Parameter Store for secure and efficient builds.

---

## **Key Notes**
- Always ensure that sensitive information (e.g., tokens) is securely stored in AWS Parameter Store.
- Test the `buildspec.yml` file locally or in a development environment to catch errors early.

---

## **Key Links**
- [AWS CodeArtifact Documentation](https://docs.aws.amazon.com/codeartifact/latest/userguide/welcome.html)
- [AWS CodeBuild Documentation](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html)
- [AWS IAM Policies Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)


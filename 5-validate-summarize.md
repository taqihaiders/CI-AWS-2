# AWS CodeBuild for Artifact Creation and CodePipeline Setup

## **1. AWS CodeBuild for Artifact Creation**

### **Objective**
This step involves creating an artifact from the source code and storing it in an S3 bucket. A separate `buildspec.yml` file located in the `AWS-files` directory is used for this purpose.

### **Steps to Configure CodeBuild for Artifact Creation**
1. **Modify the Buildspec File**:
   - Open the `AWS-files/build_buildspec.yml` file.
   - Add the **CodeArtifact Authorization Token** command under the "Export from Maven on CodeArtifact" section, similar to the previous step.
   - Save the changes, commit them, and push the updates to the Bitbucket repository:
     ```bash
     git add AWS-files/build_buildspec.yml
     git commit -m "Updated buildspec file for artifact creation"
     git push origin <branch-name>
     ```

2. **Create a New CodeBuild Project**:
   - Go to **AWS CodeBuild** and create a new project.
   - Set the source provider to **Bitbucket** and connect your repository.
   - Specify the buildspec file path as:
     ```
     AWS-files/build_buildspec.yml
     ```
   - Leave other settings as default.
   - Create the project.

3. **Attach IAM Policy**:
   - Go to **IAM Roles** and locate the role created for this CodeBuild project.
   - Attach the **CodeArtifactReadOnlyAccess** policy to allow access to dependencies.

4. **Run the Build**:
   - Start the build process and monitor the logs to ensure the artifact is successfully created and stored in the specified S3 bucket.

---

## **2. Setting Up AWS CodePipeline**

### **Objective**
AWS CodePipeline will integrate all services (e.g., Bitbucket, CodeBuild, S3, and SNS) to create a fully automated CI/CD pipeline.

---

### **Steps to Configure CodePipeline**

#### **a. Create an S3 Bucket**
1. Create an S3 bucket to store artifacts.
2. Inside the bucket, create a folder for storing artifacts (e.g., `artifacts/`).

#### **b. Create an SNS Topic**
1. Go to **SNS** and create a new topic.
2. Create a subscription for the topic:
   - Set **Protocol** to `Email`.
   - Enter your email address and confirm the subscription through the email sent to your inbox.

#### **c. Create a CodePipeline**
1. Go to **AWS CodePipeline** and create a new pipeline.
2. Provide a **Pipeline Name** and create a new **Service Role** for the pipeline.
3. **Source Stage**:
   - Set the source provider to **Bitbucket**.
   - Connect your repository and select the branch to monitor for changes.
4. **Build Stage**:
   - Set the build provider to **AWS CodeBuild**.
   - Select the previously created CodeBuild project for building and testing the code.
5. Skip the deploy stage for now and click **Create Pipeline**.

#### **d. Add Stages to CodePipeline**
1. **Edit the Pipeline**:
   - Go to the pipeline settings and click **Edit**.
2. **Add Code Analysis Stage**:
   - Add a new stage named `CodeAnalysis`.
   - Select the CodeBuild project created for SonarCloud analysis.
3. **Add Artifact Build Stage**:
   - Add a deploy stage after the build stage.
   - Configure the action provider as **S3**.
   - Set the input artifact to `BuildArtifact`.
   - Select the previously created S3 bucket and specify the folder for artifact storage.

---

### **3. Configure Notifications**
1. Go to the **Pipeline Settings** and click **Notifications**.
2. Create a new notification and select the previously created SNS topic.
3. Submit the configuration to enable notifications for pipeline events.

---

## **4. Test the Pipeline**

### **Trigger the Pipeline**
1. Make a minor change in the repository to test the pipeline:
   - For example, add an extra `#` in the `README.md` file.
   - Commit and push the changes:
     ```bash
     git add README.md
     git commit -m "Test pipeline with a minor change"
     git push origin <branch-name>
     ```

2. The pipeline should trigger automatically, performing the following:
   - Fetch the code from Bitbucket.
   - Run quality analysis on SonarCloud.
   - Build the artifact and store it in S3.

---

## **Conclusion**
With this setup, every change in the Bitbucket repository will trigger the CodePipeline. The pipeline automates code analysis, artifact creation, and notifications, ensuring a seamless CI/CD process.

---

## **Key Notes**
- Always ensure the IAM roles have the necessary permissions to access services like Parameter Store, S3, and CodeArtifact.
- Use SNS to stay informed about pipeline events.
- Regularly test the pipeline to ensure reliability.

---

## **Key Links**
- [AWS CodePipeline Documentation](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)
- [AWS CodeBuild Documentation](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html)
- [SNS Notifications](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)


# Bitbucket and AWS CodeArtifact Setup

## **1. Bitbucket Setup**

### **Platform**: [Bitbucket.org](https://bitbucket.org)

### **Steps to Set Up Bitbucket**
1. **Create an Account**:
   - Sign up for an account on Bitbucket.org.

2. **Create a Repository**:
   - Create a new repository to store your project code.

3. **Configure SSH Keys**:
   - **Generate SSH Keys**: Use the following command to generate SSH keys:
     ```bash
     ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
     ```
   - **Add the Public Key**: Copy the generated public key to your Bitbucket account.
   - **Configure SSH for Bitbucket**: Add Bitbucket configuration to your SSH config file:
     ```bash
     Host bitbucket.org
         HostName bitbucket.org
         User git
         IdentityFile ~/.ssh/id_rsa
     ```

4. **Migrate Code from GitHub to Bitbucket**:
   - Clone the source code from GitHub:
     ```bash
     git clone https://github.com/hkhcoder/vprofile-project.git
     ```
   - Remove the existing GitHub remote URL:
     ```bash
     git remote rm origin
     ```
   - Add the Bitbucket repository URL:
     ```bash
     git remote add origin git@bitbucket.org:devopscicd-taqi/vprofile-build2.git
     ```
   - Push the code to the Bitbucket repository:
     ```bash
     git push -u origin --all
     git push -u origin --tags
     ```

---

### **Key Commands for Migration**
```bash
# Clone the GitHub repository
git clone https://github.com/hkhcoder/vprofile-project.git

# Navigate to the project directory
cd vprofile-project/

# Remove GitHub remote URL
git remote rm origin

# Add the Bitbucket remote URL
git remote add origin git@bitbucket.org:devopscicd-taqi/vprofile-build2.git

# Push branches and tags to Bitbucket
git push -u origin --all
git push -u origin --tags


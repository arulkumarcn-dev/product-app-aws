# Complete Step-by-Step Guide for Product App AWS Deployment

## 📋 Overview
This guide covers all 5 tasks for deploying the SpringBoot Product application on AWS with CI/CD pipelines.

---

## ✅ TASK 1: Create GitHub Repository Online

### Step 1.1: Create GitHub Repository
1. Open browser and go to https://github.com
2. Log in to your GitHub account
3. Click the **"+"** icon in top right corner → **"New repository"**
4. Fill in repository details:
   - Repository name: `product-app-aws`
   - Description: `Spring Boot Product REST API with AWS deployment`
   - Select **Public**
   - ✅ Check "Add a README file"
   - Click **"Create repository"**
5. **📸 Take Screenshot** - Ensure your AWS/GitHub username appears in top right corner

### Step 1.2: Create Project Structure Online
Since you can't push from office laptop, create files directly in GitHub:

1. Click **"Add file"** → **"Create new file"**
2. Create `.gitignore`:
   - File name: `.gitignore`
   - Copy content from the `.gitignore` file in your local workspace
   - Click **"Commit changes"**
   - **📸 Take Screenshot**

3. Create `pom.xml`:
   - Click **"Add file"** → **"Create new file"**
   - File name: `pom.xml`
   - Copy content from your local `pom.xml`
   - Commit changes
   - **📸 Take Screenshot**

4. Create directory structure and files:
   - For folders, use syntax like: `src/main/java/com/product/ProductAppAwsApplication.java`
   - Create each Java file:
     - `src/main/java/com/product/ProductAppAwsApplication.java`
     - `src/main/java/com/product/controller/ProductController.java`
     - `src/main/java/com/product/dto/Product.java`
     - `src/main/resources/application.properties`
   - **📸 Take Screenshot** of repository structure

5. Create `Dockerfile`:
   - Click **"Add file"** → **"Create new file"**
   - File name: `Dockerfile`
   - Copy content from the Dockerfile created in workspace
   - Commit changes
   - **📸 Take Screenshot**

6. Create `buildspec.yml`:
   - Click **"Add file"** → **"Create new file"**
   - File name: `buildspec.yml`
   - Copy content from buildspec.yml created in workspace
   - Commit changes
   - **📸 Take Screenshot**

7. Create `Jenkinsfile`:
   - Click **"Add file"** → **"Create new file"**
   - File name: `Jenkinsfile`
   - Copy content from Jenkinsfile created in workspace
   - Remember to update `YOUR_USERNAME` with your actual GitHub username
   - Commit changes
   - **📸 Take Screenshot**

8. **Final Screenshot**: Take screenshot showing complete repository structure

---

## ✅ TASK 2: Jenkins Pipeline on Local Machine

### Step 2.1: Install Prerequisites
1. **Install Java 17**:
   - Download from: https://adoptium.net/
   - Verify: Open PowerShell and run:
     ```powershell
     java -version
     ```
   - **📸 Take Screenshot** of version output

2. **Install Maven**:
   - Download from: https://maven.apache.org/download.cgi
   - Extract to `C:\Maven`
   - Add to PATH: `C:\Maven\bin`
   - Verify:
     ```powershell
     mvn -version
     ```
   - **📸 Take Screenshot**

3. **Install Jenkins**:
   - Download Jenkins WAR file from: https://www.jenkins.io/download/
   - Or install using Windows installer
   - Start Jenkins:
     ```powershell
     java -jar jenkins.war
     ```
   - Access Jenkins at: http://localhost:8080
   - **📸 Take Screenshot** of Jenkins unlock page

### Step 2.2: Configure Jenkins
1. **Initial Setup**:
   - Copy initial admin password from console or file
   - Install suggested plugins
   - Create admin user
   - **📸 Take Screenshot** of Jenkins dashboard

2. **Configure Maven in Jenkins**:
   - Go to **Manage Jenkins** → **Global Tool Configuration**
   - Scroll to **Maven** section
   - Click **Add Maven**
   - Name: `Maven-3.9`
   - ✅ Check "Install automatically"
   - Version: Select latest 3.9.x
   - Click **Save**
   - **📸 Take Screenshot**

3. **Install Git Plugin** (if not already installed):
   - Go to **Manage Jenkins** → **Manage Plugins**
   - Search for "Git plugin"
   - Install if needed
   - **📸 Take Screenshot**

### Step 2.3: Create Jenkins Pipeline
1. **Create New Pipeline Job**:
   - From Jenkins dashboard, click **"New Item"**
   - Enter name: `product-app-pipeline`
   - Select **"Pipeline"**
   - Click **OK**
   - **📸 Take Screenshot**

2. **Configure Pipeline**:
   - In **Pipeline** section:
     - Definition: **"Pipeline script from SCM"**
     - SCM: **Git**
     - Repository URL: `https://github.com/YOUR_USERNAME/product-app-aws.git`
     - Branch: `*/main`
     - Script Path: `Jenkinsfile`
   - Click **Save**
   - **📸 Take Screenshot** of configuration

3. **Run Pipeline**:
   - Click **"Build Now"**
   - Watch the pipeline stages execute
   - **📸 Take Screenshot** of successful build with all stages (green)
   - **📸 Take Screenshot** of console output

4. **View Artifacts**:
   - After successful build, check archived artifacts
   - **📸 Take Screenshot** showing the JAR file artifact

---

## ✅ TASK 3: Deploy to AWS ECS using ECR

### Step 3.1: Setup AWS CLI
1. **Install AWS CLI**:
   - Download from: https://aws.amazon.com/cli/
   - Install and verify:
     ```powershell
     aws --version
     ```
   - **📸 Take Screenshot**

2. **Configure AWS Credentials**:
   ```powershell
   aws configure
   ```
   - Enter Access Key ID
   - Enter Secret Access Key
   - Region: `us-east-1` (or your preferred region)
   - Output format: `json`
   - **📸 Take Screenshot** (hide sensitive data)

### Step 3.2: Build Application Locally
1. **Clone Repository** (to a different folder if needed):
   ```powershell
   cd C:\temp
   git clone https://github.com/YOUR_USERNAME/product-app-aws.git
   cd product-app-aws
   ```

2. **Build with Maven**:
   ```powershell
   mvn clean package -DskipTests
   ```
   - **📸 Take Screenshot** of successful build

3. **Test Locally** (optional):
   ```powershell
   java -jar target/ProductAppAWS-0.0.1-SNAPSHOT.jar
   ```
   - Open browser: http://localhost:8080/product
   - **📸 Take Screenshot** of API response

### Step 3.3: Create ECR Repository
1. **Login to AWS Console**:
   - Open https://console.aws.amazon.com
   - Navigate to **ECR** service
   - **📸 Take Screenshot** with your username visible in top right

2. **Create Repository**:
   - Click **"Create repository"**
   - Repository name: `product-app-repo`
   - Tag immutability: Disabled
   - Scan on push: Enabled
   - Click **"Create repository"**
   - **📸 Take Screenshot** of created repository

3. **Get Repository URI**:
   - Click on repository name
   - Copy the Repository URI
   - Example: `123456789012.dkr.ecr.us-east-1.amazonaws.com/product-app-repo`
   - **📸 Take Screenshot**

### Step 3.4: Push Docker Image to ECR
1. **Authenticate Docker to ECR**:
   ```powershell
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
   ```
   - Replace with your account ID and region
   - **📸 Take Screenshot** of successful login

2. **Build Docker Image**:
   ```powershell
   docker build -t product-app .
   ```
   - **📸 Take Screenshot** of successful build

3. **Tag Docker Image**:
   ```powershell
   docker tag product-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/product-app-repo:latest
   ```
   - **📸 Take Screenshot**

4. **Push to ECR**:
   ```powershell
   docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/product-app-repo:latest
   ```
   - **📸 Take Screenshot** of push progress and completion

5. **Verify in AWS Console**:
   - Go back to ECR in AWS Console
   - Click on repository
   - View image with tag `latest`
   - **📸 Take Screenshot**

### Step 3.5: Create ECS Cluster
1. **Navigate to ECS**:
   - In AWS Console, go to **ECS** service
   - **📸 Take Screenshot** with username visible

2. **Create Cluster**:
   - Click **"Create Cluster"**
   - Cluster name: `product-app-cluster`
   - Infrastructure: **AWS Fargate (serverless)**
   - Click **"Create"**
   - **📸 Take Screenshot** of cluster overview

### Step 3.6: Create Task Definition
1. **Create New Task Definition**:
   - In ECS, go to **Task Definitions**
   - Click **"Create new task definition"**
   - **📸 Take Screenshot**

2. **Configure Task Definition**:
   - Task definition family: `product-app-task`
   - Launch type: **AWS Fargate**
   - Operating system: **Linux**
   - CPU: **0.5 vCPU**
   - Memory: **1 GB**
   - Task role: Create new or use existing
   - **📸 Take Screenshot**

3. **Add Container**:
   - Container name: `product-app-container`
   - Image URI: `123456789012.dkr.ecr.us-east-1.amazonaws.com/product-app-repo:latest`
   - Port mappings:
     - Container port: `8080`
     - Protocol: `TCP`
     - App protocol: `HTTP`
   - Click **"Create"**
   - **📸 Take Screenshot** of task definition

### Step 3.7: Create ECS Service
1. **Create Service**:
   - Go to your cluster: `product-app-cluster`
   - Click **"Create"** in Services tab
   - **📸 Take Screenshot**

2. **Configure Service**:
   - Launch type: **Fargate**
   - Task definition: `product-app-task`
   - Service name: `product-app-service`
   - Number of tasks: `1`
   - **📸 Take Screenshot**

3. **Configure Network**:
   - VPC: Select default VPC
   - Subnets: Select at least 2 subnets
   - Security group:
     - Create new security group
     - Add inbound rule: **TCP 8080** from **Anywhere** (0.0.0.0/0)
   - Public IP: **Enabled**
   - **📸 Take Screenshot** of network configuration

4. **Load Balancing** (Optional but recommended):
   - Type: **Application Load Balancer**
   - Create new load balancer
   - Listener port: `80`
   - Target group port: `8080`
   - Health check path: `/product`
   - **📸 Take Screenshot**

5. **Create Service**:
   - Review and click **"Create"**
   - Wait for service to become active
   - **📸 Take Screenshot** of running service

### Step 3.8: Test Application
1. **Get Public IP**:
   - Click on running task
   - Copy Public IP address
   - **📸 Take Screenshot**

2. **Test API in Browser**:
   - Open browser
   - Go to: `http://PUBLIC_IP:8080/product`
   - You should see JSON response with products
   - **📸 Take Screenshot** showing API response with AWS username visible

3. **Test with Load Balancer** (if configured):
   - Get Load Balancer DNS name from EC2 → Load Balancers
   - Go to: `http://LOAD_BALANCER_DNS/product`
   - **📸 Take Screenshot**

---

## ✅ TASK 4: AWS CodePipeline CI/CD

### Step 4.1: Update buildspec.yml
1. **Edit buildspec.yml in GitHub**:
   - Go to your GitHub repository
   - Click on `buildspec.yml`
   - Click edit (pencil icon)
   - Update `YOUR_ECR_REPOSITORY_URI` with actual ECR URI:
     ```yaml
     REPOSITORY_URI=123456789012.dkr.ecr.us-east-1.amazonaws.com/product-app-repo
     ```
   - Commit changes
   - **📸 Take Screenshot**

### Step 4.2: Create CodeBuild Project
1. **Navigate to CodeBuild**:
   - In AWS Console, go to **CodeBuild**
   - **📸 Take Screenshot** with username visible

2. **Create Build Project**:
   - Click **"Create build project"**
   - Project name: `product-app-build`
   - **📸 Take Screenshot**

3. **Source Configuration**:
   - Source provider: **GitHub**
   - Connect to GitHub (authorize if needed)
   - Repository: Select `product-app-aws`
   - Source version: `main`
   - **📸 Take Screenshot**

4. **Environment Configuration**:
   - Environment image: **Managed image**
   - Operating system: **Ubuntu**
   - Runtime: **Standard**
   - Image: **aws/codebuild/standard:7.0**
   - ✅ Privileged (required for Docker)
   - Service role: Create new or use existing
   - **📸 Take Screenshot**

5. **Buildspec**:
   - Build specifications: **Use a buildspec file**
   - Buildspec name: `buildspec.yml`
   - **📸 Take Screenshot**

6. **Create Build Project**:
   - Click **"Create build project"**
   - **📸 Take Screenshot** of created project

7. **Test Build**:
   - Click **"Start build"**
   - Watch build phases execute
   - **📸 Take Screenshot** of successful build

### Step 4.3: Create CodePipeline
1. **Navigate to CodePipeline**:
   - In AWS Console, go to **CodePipeline**
   - **📸 Take Screenshot**

2. **Create Pipeline**:
   - Click **"Create pipeline"**
   - Pipeline name: `product-app-pipeline`
   - Service role: Create new
   - Click **"Next"**
   - **📸 Take Screenshot**

3. **Source Stage**:
   - Source provider: **GitHub (Version 2)**
   - Connect to GitHub
   - Repository: `product-app-aws`
   - Branch: `main`
   - Output artifact format: **CodePipeline default**
   - Click **"Next"**
   - **📸 Take Screenshot**

4. **Build Stage**:
   - Build provider: **AWS CodeBuild**
   - Project name: `product-app-build`
   - Click **"Next"**
   - **📸 Take Screenshot**

5. **Deploy Stage**:
   - Deploy provider: **Amazon ECS**
   - Cluster name: `product-app-cluster`
   - Service name: `product-app-service`
   - Image definitions file: `imagedefinitions.json`
   - Click **"Next"**
   - **📸 Take Screenshot**

6. **Review and Create**:
   - Review all stages
   - Click **"Create pipeline"**
   - **📸 Take Screenshot** of pipeline overview

7. **Watch Pipeline Execute**:
   - Pipeline will automatically start
   - Watch Source → Build → Deploy stages
   - **📸 Take Screenshot** of successful pipeline execution (all green)

8. **Verify Deployment**:
   - Go to ECS service
   - Check that new task is running with updated image
   - Test API endpoint again
   - **📸 Take Screenshot**

### Step 4.4: Test CI/CD
1. **Make Code Change**:
   - In GitHub, edit `ProductController.java`
   - Add a new product to static data
   - Commit change
   - **📸 Take Screenshot** of commit

2. **Watch Pipeline Trigger**:
   - Go to CodePipeline
   - Pipeline should automatically trigger
   - **📸 Take Screenshot** of pipeline running

3. **Verify Change**:
   - After deployment completes
   - Test API endpoint
   - **📸 Take Screenshot** showing new data

---

## ✅ TASK 5: CloudFormation Pipeline

### Step 5.1: Upload CloudFormation Template
1. **Edit cloudformation-pipeline.yaml**:
   - In GitHub, upload the `cloudformation-pipeline.yaml` file
   - Click **"Add file"** → **"Upload files"**
   - Upload the file from your local workspace
   - Commit
   - **📸 Take Screenshot**

### Step 5.2: Create GitHub Personal Access Token
1. **Generate Token**:
   - Go to GitHub → Settings → Developer settings → Personal access tokens
   - Click **"Generate new token (classic)"**
   - Select scopes: `repo`, `admin:repo_hook`
   - Generate and copy token
   - **📸 Take Screenshot** (hide token value)

### Step 5.3: Deploy CloudFormation Stack
1. **Navigate to CloudFormation**:
   - In AWS Console, go to **CloudFormation**
   - **📸 Take Screenshot** with username visible

2. **Create Stack**:
   - Click **"Create stack"** → **"With new resources"**
   - Template source: **Upload a template file**
   - Upload `cloudformation-pipeline.yaml`
   - Click **"Next"**
   - **📸 Take Screenshot**

3. **Stack Details**:
   - Stack name: `product-app-cf-stack`
   - Parameters:
     - GitHubOwner: `YOUR_GITHUB_USERNAME`
     - GitHubRepo: `product-app-aws`
     - GitHubBranch: `main`
     - GitHubToken: `PASTE_YOUR_TOKEN`
     - ECRRepositoryName: `product-app-cf-repo`
   - Click **"Next"**
   - **📸 Take Screenshot**

4. **Configure Stack Options**:
   - Leave defaults
   - Click **"Next"**
   - **📸 Take Screenshot**

5. **Review and Create**:
   - ✅ Check "I acknowledge that AWS CloudFormation might create IAM resources"
   - Click **"Create stack"**
   - **📸 Take Screenshot**

6. **Monitor Stack Creation**:
   - Watch stack events
   - Wait for **CREATE_COMPLETE** status
   - **📸 Take Screenshot** of successful stack creation

7. **View Outputs**:
   - Click **"Outputs"** tab
   - Copy ECR Repository URI, Pipeline name
   - **📸 Take Screenshot** of outputs

### Step 5.4: Verify CloudFormation Pipeline
1. **Check Created Resources**:
   - Go to **CodePipeline**
   - Find the pipeline created by CloudFormation
   - **📸 Take Screenshot** of pipeline

2. **Check ECR Repository**:
   - Go to **ECR**
   - Find repository created by CloudFormation
   - **📸 Take Screenshot**

3. **Check S3 Artifacts Bucket**:
   - Go to **S3**
   - Find artifacts bucket
   - **📸 Take Screenshot**

4. **Trigger Pipeline**:
   - Make a change in GitHub
   - Watch CloudFormation pipeline execute
   - **📸 Take Screenshot** of successful execution

5. **Compare with Manual Pipeline**:
   - Take screenshot showing both pipelines
   - **📸 Take Screenshot**

---

## 🎯 Final Checklist

### Task 1 - GitHub Repository ✅
- [ ] Repository created online
- [ ] All source files uploaded
- [ ] Dockerfile added
- [ ] buildspec.yml added
- [ ] Jenkinsfile added
- [ ] All screenshots taken with username visible

### Task 2 - Jenkins Pipeline ✅
- [ ] Jenkins installed and running
- [ ] Maven configured
- [ ] Pipeline created
- [ ] Successful build executed
- [ ] Artifacts archived
- [ ] All screenshots taken

### Task 3 - AWS ECS Deployment ✅
- [ ] ECR repository created
- [ ] Docker image pushed to ECR
- [ ] ECS cluster created
- [ ] Task definition created
- [ ] ECS service running
- [ ] Application accessible via browser
- [ ] All screenshots with AWS username visible

### Task 4 - CodePipeline CI/CD ✅
- [ ] CodeBuild project created
- [ ] CodePipeline created
- [ ] All stages (Source, Build, Deploy) configured
- [ ] Successful pipeline execution
- [ ] Automatic deployment verified
- [ ] All screenshots taken

### Task 5 - CloudFormation Pipeline ✅
- [ ] CloudFormation template created
- [ ] Stack deployed successfully
- [ ] All resources created automatically
- [ ] Pipeline functional
- [ ] All screenshots taken

---

## 📝 Important Notes

1. **Security**:
   - Don't commit AWS credentials to GitHub
   - Use IAM roles with least privilege
   - Rotate GitHub tokens regularly
   - Use HTTPS for Git operations from office

2. **Cost Management**:
   - Stop ECS services when not in use
   - Delete unused ECR images
   - Fargate costs are per-second
   - Consider using t2.micro if using EC2

3. **Troubleshooting**:
   - Check CloudWatch Logs for application errors
   - Verify security group allows traffic on port 8080
   - Ensure task has public IP if not using load balancer
   - Check IAM roles have necessary permissions

4. **Best Practices**:
   - Tag all AWS resources
   - Use separate environments (dev, staging, prod)
   - Implement health checks
   - Monitor with CloudWatch
   - Use environment variables for configuration

---

## 🔗 Useful Links

- Reference repository: https://github.com/zen-tech-training/contact-springboot-app
- AWS ECS Documentation: https://docs.aws.amazon.com/ecs/
- AWS CodePipeline: https://docs.aws.amazon.com/codepipeline/
- Jenkins Documentation: https://www.jenkins.io/doc/
- Docker Documentation: https://docs.docker.com/

---

Good luck with your deployment! 🚀

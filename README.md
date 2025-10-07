# TestWheel Automation Plugin

## Introduction

The [TestWheel](https://www.testwheel.com/) Automation Plugin is a powerful plugin designed to integrate seamlessly with DevOps tools for continuous integration and deployment (CI/CD). This plugin automates the execution of TestWheel’s comprehensive testing frameworks within your CI/CD pipelines post-deployment, ensuring reliable testing throughout your development lifecycle.

## Getting Started

To get started with the **TestWheel Automation Plugin**:

1. Install the plugin from the Jenkins Marketplace.  
2. Integrate it with your CI/CD pipeline.

## Prerequisites

Before using the TestWheel Automation Plugin, ensure the following:

- You have an existing Jenkins CI server.  
- You have a registered TestWheel account. [Register](https://app.testwheel.com/registration) or [Login](https://app.testwheel.com/login).  
- Your application is registered in TestWheel, and you’ve obtained the following credentials:
  - **Secure ApiKey**
  - **PrjctKey**

These credentials are required to perform post-deployment test automation using the plugin.

## Installation and Setup

1. Navigate to **Manage Jenkins → Manage Plugins**.  
2. Go to the **Available Plugins** tab.  
3. Search for **TestWheel Automation Plugin** and install it.  
4. After installation, ensure you're logged into your TestWheel account within Jenkins.  

You can integrate the plugin into your pipeline in two ways:

---

## Usage

### **Option 1: Using a Jenkins Pipeline Script**

You can define a task in your CI/CD pipeline using the sample script below:

> 🔧 **Note:** Replace `<YOUR_STAGE_NAME>`, `<YOUR_API_KEY>`, and `<YOUR_PROJECT_KEY>` with your actual values.

```groovy
pipeline {
    agent any
    stages {
        stage('<YOUR_STAGE_NAME>') {
            steps {
                testwheelTrigger(apiKey: '<YOUR_API_KEY>', prjctKey: '<YOUR_PROJECT_KEY>')
            }
        }
    }
}
```
This approach is recommended for teams using declarative pipelines and version-controlled Jenkinsfiles.

### **Option 2: Using the Jenkins UI (Freestyle Project)**

For **Freestyle Jenkins Jobs**:

1. Go to your Jenkins job configuration.  
2. Click **Add build step**.  
3. Select **TestWheelTrigger** from the dropdown list.  
4. Enter your **ApiKey** and **PrjctKey** obtained from TestWheel.  

This option is ideal for users who prefer Jenkins’ UI over code-based pipeline configuration.

---

## Build and Test

Once integrated:

- Upon reaching the **post-deployment** stage, the plugin automatically triggers tests using the provided credentials.  
- A test report will be generated:  
  - ✅ If tests pass — the pipeline proceeds to the next stage.  
  - ❌ If tests fail — the pipeline halts immediately to prevent faulty deployments.

---

## Contribute

We welcome feedback, suggestions, and bug reports.  
Please use the [Contact Us](https://app.testwheel.com/contact-us) page on our website to reach out.
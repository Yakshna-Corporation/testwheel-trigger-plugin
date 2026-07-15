# TestWheel Automation Plugin for Jenkins

## Introduction

The TestWheel Automation Plugin connects [TestWheel](https://testwheel.com) an AI-powered, no-code test automation platform for web and API directly into your Jenkins pipelines. It's built as part of TestWheel's broader [DevOps integration suite](https://www.testwheel.com/devops-integrations), which also supports Azure DevOps and JIRA, so teams can run functional and regression checks automatically the moment a build is deployed, instead of waiting on a manual QA cycle.

Because the plugin triggers real test execution against your deployed application, a failed run stops the pipeline before a broken build reaches users, turning "deploy and hope" into "deploy and verify."

## Why Automate Post-Deployment Testing?

Manual smoke-testing after every deployment doesn't scale, and it's the step most teams quietly skip under release pressure. Wiring TestWheel into Jenkins closes that gap:

- **Consistency** - The same test suite runs the same way on every build, removing tester-to-tester variance.
- **Speed** - Tests kick off automatically post-deployment, with no one needing to remember to trigger them.
- **Early Detection** - Regressions surface within minutes of a release instead of days later in a bug report.
- **Audit Trail** - Every pipeline run produces a test report you can attach to a release or a compliance review.

This pattern is the same one TestWheel uses in production environments with strict release governance which including its work with U.S. Air Force and Department of Defense systems, where regression coverage and reporting for every change are a hard requirement, not a nice-to-have. See the [AFRISS-TF case study](https://www.testwheel.com/afriss-tf) for a real-world example of this at scale.

1. Install the plugin from the Jenkins Marketplace.  
2. Integrate it with your CI/CD pipeline.

## How It Fits Into TestWheel

TestWheel itself handles the actual test creation and execution logic, you can author tests with AI-generated natural-language steps, record-and-playback, or low-code builders across [web](https://www.testwheel.com/web-testing) and [API](https://www.testwheel.com/api-testing) surfaces. This plugin is the trigger mechanism: it tells your registered TestWheel project to run its configured test suite the moment your Jenkins pipeline reaches the post-deployment stage, then reports pass/fail back to Jenkins.

If you're evaluating whether TestWheel is the right fit before wiring up CI/CD, the [How It Works](https://www.testwheel.com/how-it-works) page and [product sheet](https://www.testwheel.com/product-sheet) walk through the platform end to end, and the [FAQ](https://www.testwheel.com/faq/) covers common setup questions.

## Prerequisites

What you need before installing TestWheel plugin:

- A Jenkins server, self-hosted or cloud, already up and running.  
- A TestWheel account. [Register](https://app.testwheel.com/registration) if you don't have one, or [Login](https://app.testwheel.com/login) if you do.  
- Your application already registered as a project inside TestWheel, with two credentials pulled from that project's settings:
  
  - **Secure ApiKey**: Authenticates the plugin against your TestWheel account.
  - **PrjctKey**: Tells TestWheel which project, and which test suite, to run.

These credentials are required to perform post-deployment test automation using the plugin.

## Installation and Setup the plugin

1. In Jenkins, go to **Manage Jenkins → Manage Plugins**.  
2. Click into the **Available Plugins** tab.  
3. Search for **TestWheel Automation Plugin** and install it.  
4. Restart Jenkins if it asks you to, then make sure you're actually logged into TestWheel within Jenkins before you try running anything..  

From here you've got two ways to use it. It mostly comes down to how your team already manages Jenkins jobs.

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
## A quick word on credentials

Don't hardcode the apiKey into a Jenkinsfile that's sitting in version control. That's an easy way to leak it. Instead:

- Store it as a Jenkins Credentials entry (Secret text) and pull it in with credentials('testwheel-api-key').
- If the key ever ends up somewhere, it shouldn't, like building logs, a fork, or a shared repo, rotate it from your TestWheel project settings and move on.
- Keep PrjctKey scoped to the actual application it belongs to. Pointing one project key at a different app pipeline just means you're testing the wrong thing without realizing it.

---
## When something's not working

---
## Questions people usually ask

**Does the plugin create tests, or just run them?** Just runs them. Test creation, whether AI-generated, record-and-playback, or low-code, happens inside TestWheel itself, not here.

**Can I use this for more than one application?** Yes, just add a separate stage per app, each with its own PrjctKey. One key maps to one project.

**If tests fail, does it block the deploy or just warn me?** It blocks. The pipeline halts. If you'd rather it just flag issues without stopping anything, you'd need to move the stage after your deployment gate or adjust it to a non-blocking step yourself.

**Is this just for web apps?** No. It depends what the TestWheel project behind it is set up to test. Could be [web](https://www.testwheel.com/web-testing), [API](https://www.testwheel.com/api-testing), [mobile](https://www.testwheel.com/mobile-testing), or [performance](https://www.testwheel.com/performance-testing).

More in the general [TestWheel FAQ](https://www.testwheel.com/faq) if you're stuck on something else.

---
## Related TestWheel Resources
-  [DevOps integrations](https://www.testwheel.com/devops-integrations): Jenkins, Azure DevOps, JIRA, all in one place
-  [AI test automation](https://www.testwheel.com/ai-test-automation): Turning manual test cases into automated ones
-  [Selenium to AI automation](https://www.testwheel.com/selenium-ai-automation): For teams migrating off an existing Selenium suite
-  [How it works](https://www.testwheel.com/how-it-works): platform overview
-  [Case studies](https://www.testwheel.com/case-studies): including the federal and government work mentioned above
-  [Docs](https://docs.testwheel.com): full technical reference
-  [Blog](https://www.testwheel.com/blog): testing and release engineering writing
  
---

## Contribute

Found a bug, or have a suggestion? Use [Contact Us](https://app.testwheel.com/contact-us), or open an issue here if it's something reproducible.

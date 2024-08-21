# Continuous-Delivery-Pipeline-in-AWS
I am about to walk you through how I developed a pipeline to aid smooth logging of changes to an application, which will require manual approval for the changes. We will create the pipeline using AWS CodePipeline, a service that builds, tests, and deploys your code every time there is a code change.

The following diagram visually represents the services used in this project and how they are connected. This application uses GitHub, AWS Elastic Beanstalk, AWS CodeBuild, and AWS CodePipeline.

![image](https://github.com/Elizzy01/Continuous-Delivery-Pipeline-in-AWS/assets/98459984/124cb918-e497-405b-b7ad-2205877d2d45)

Continuous deployment allows you to deploy revisions to a production environment automatically without explicit approval from a developer, making the entire software release process automated.

Everything done in this tutorial is `Free Tier eligible`.

## What you will accomplish?
In this tutorial, you will:
- create an automated software release pipeline that deploys a live sample app
- create the pipeline using AWS CodePipeline
- use AWS Elastic Beanstalk as the deployment target for the sample app

Before starting this tutorial, you will need an AWS account. If you don't already have one, follow the [Setting Up Your AWS Environment](https://aws.amazon.com/getting-started/guides/setup-environment/) getting started guide for a quick overview. The entire steps are below grouped into five modules.

- [Stage 1 - Setting Up Github REPO](#step-1---setting-up-github-repo)
- [Stage 2 - Deploy Web App](#step-2---deploy-web-app)
- [Stage 3 - Create Build Project](#step-3---create-build-project)
- [Stage 4 - Create Delivery Pipeline](#step-4---create-delivery-pipeline)
- [Stage 5 - Finalize Pipeline and Test](#step-5---finalize-pipeline-and-test)

Let's dig deeper into each of the steps..

## Stage 1 - Setting Up Github REPO
So basically my app was already on a github repo. If it were to be a native AWS architecture, we would rather use **CodeCommit**. Here is the link to the repo [asw-elastic-beanstalk-expressjs-sample](https://github.com/aws-samples/aws-elastic-beanstalk-express-js-sample)

- Click on the 'fork' button on the right top of the repo page to create a copy of the repo in your account.
- Thereafter, clone the repository to have a copy on your local machine.
```
git clone https://github.com/YOUR-USERNAME/aws-elastic-beanstalk-express-js-sample
```
- Open it up with your favourite IDE, mine is VSCode.
- Then go ahead and make changes to the sample code.

![image](https://github.com/user-attachments/assets/f17ec920-a281-4d55-ba8c-18ccbf520aca)

- After opening up the code in your IDE, open up the `app.js` 
- On the `5th line` of the code, inside the 'res.send' quote, edit the statement to whatever you will like displayed.
- Now commit your change, copy/ modify the command below:
```
git add app.js
git commit -m "change message"
```
- Then push
```
git push
```
- To test and see your changes, navigate to your github account, on the left navigation panel, under Repositories, select the one named `aws-elastic-beanstalk-express-js-sample`.
- Choose the `app.js` file. The contents of the file, including your change, should be displayed.

## Stage 2 - Deploy Web App 
In this stage, we are focusing on three things
- Configure and create an AWS Elastic Beanstalk environment
- Deploy a sample web app to AWS Elastic Beanstalk
- Test the sample web app

### Key concepts
- AWS Elastic Beanstalk - A service that makes it easy to deploy your application on AWS. You simply upload your code and Elastic Beanstalk deploys, manages, and scales your application.

- Environment - Collection of AWS resources provisioned by Elastic Beanstalk that are used to run your application.

- EC2 instance - Virtual server in the cloud. Elastic Beanstalk will provision one or more Amazon EC2 instances when creating an environment.

- Web server - Software that uses the HTTP protocol to serve content over the Internet. It is used to store, process, and deliver web pages.

- Platform—Combination of operating system, programming language runtime, web server, application server, and Elastic Beanstalk components. Your application runs using the components provided by a platform.

### Configure an AWS Elastic Beanstalk App
1. In a new browser tab, open the [AWS Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk/home?region=us-west-2#/welcome).
2. **Please take note to check the region where you are provisioning resources. I had issues later on when I had my services provisioned in different regions, however, I finally settled for US-West-2 for all my resources.**

![image](https://github.com/user-attachments/assets/ac5879a1-95d1-403a-bbfc-84b9ebcf8a99)

3. Choose the orange **Create Application button**.
4. Choose **Web server environment** under the Configure environment heading.
5. In the text box under the heading Application name, enter **DevOpsGettingStarted**.
6. In the Platform dropdown menu, under the Platform heading, select **Node.js**. Platform branch and Platform version will automatically populate with default selections.
7. Confirm that the radio button next to Sample application under the Application code heading is selected.
8. Confirm that the radio button next to Single instance (free tier eligible) under the Presets heading is selected.
9. Select Next.
10. On the Configure service access screen, choose **Use an existing service role for Service Role**.

11. For EC2 instance profile dropdown list, the values displayed in this dropdown list may vary, depending on whether your account has previously created a new environment.

![image](https://github.com/user-attachments/assets/20d9e953-acdf-4d71-a6dc-815d943f9fc4)

12. Choose one of the following, based on the values displayed in your list.

      - If aws-elasticbeanstalk-ec2-role displays in the dropdown list, select it from the EC2 instance profile dropdown list.
      - If another value displays in the list, and it’s the default EC2 instance profile intended for your environments, select it from the EC2 instance profile dropdown list.
      - If the EC2 instance profile dropdown list doesn't list any values to choose from, expand the procedure that follows, Create IAM Role for EC2 instance profile.
      - Complete the steps in [Create IAM Role for EC2 instance profile](https://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-create-iam-instance-profile.html) to create an IAM Role that you can subsequently select for the EC2 instance profile. Then, return back to this step.
    
14. Now that you've created an IAM Role, and refreshed the list, it displays as a choice in the dropdown list. Select the IAM Role you just created from the EC2 instance profile dropdown list.
15. Choose `Skip to Review` on the Configure service access page. This will select the default values for this step and skip the optional steps.

![image](https://github.com/user-attachments/assets/bd88c26e-1624-46b6-9e13-4eb4f7715834)

16. The Review page displays a summary of all your choices.

17. Choose Submit at the bottom of the page to initialize the creation of your new environment.

![image](https://github.com/user-attachments/assets/35db1666-783e-454a-9cc5-ccea05512c27)

### Test the environment
18. To test your environment, select the link in **Domain** under the name of your environment

![image](https://github.com/user-attachments/assets/faf0351c-bb6a-484e-8f0b-1ed4fa0cf609)

19. The domain should display this, the Node-js interface.

![AWS EBS open page](https://github.com/user-attachments/assets/bb05320b-305b-4f81-8930-9f9433f0f415)

## Stage 3 - Create Build Project

1. In a new browser tab, open the [AWS CodeBuild console](https://console.aws.amazon.com/codesuite/codebuild/start?region=us-west-2).
2. Choose the orange **Create project** button.
3. In the Project name field, enter **Build-DevOpsGettingStarted**.
4. Select GitHub from the **Source Provider** dropdown menu.
5. Confirm that the **Connect using OAuth** radio button is selected.
6. Choose the white **Connect to GitHub** button. A new browser tab will open asking you to give AWS CodeBuild access to your GitHub repo.
7. Choose the green **Authorize aws-codesuite** button.
8. Enter your GitHub password.
9. Choose the orange `Confirm` button.
10. Select Repository in my GitHub account.
11. Enter `aws-elastic-beanstalk-express-js-sample` in the search field.
12. Select the repo you forked in Module 1. After selecting your repo, your screen should look like this:

![image](https://github.com/user-attachments/assets/4675faae-239b-4b3a-b65e-caebb7b7ed83)

Confirm that Managed Image is selected.

13. Select **Amazon Linux 2** from the **Operating system** dropdown menu.

14. Select Standard from the Runtime(s) dropdown menu.

15. Select `aws/codebuild/amazonlinux2-x86_64-standard:3.0`from the Image dropdown menu.

16. Confirm that **Always use the latest image** for this runtime version is selected for Image version.

17. Confirm that **Linux** is selected for Environment type.

18. Confirm that **New service role** is selected.
    

### Creating the Buildspec file for the project

19. Select Insert build commands.
20. Choose Switch to editor.
21. Replace the Buildspec in the editor with the code below:

```
version: 0.2
phases:
    build:
        commands:
            - npm i --save
artifacts:
    files:
        - '**/*'
```
22. Choose the orange Create build project button. You should now see a dashboard for your project.

### Testing the build

23. Choose the orange *Start build* button. This will load a page to configure the build process.
24. Confirm that the loaded page references the correct GitHub repo.
25. Choose the orange *Start build* button.
`Wait for the build to complete. As you are waiting you should see a green bar at the top of the page with the message Build started, the progress for your build under Build log, and, after a couple minutes, a green checkmark and a Succeeded message confirming the build worked.`

![AWS code build](https://github.com/user-attachments/assets/bd1b58b7-5577-4b78-9ece-86ebcb173993)


## Stage 4 - Create Delivery Pipeline
**Key concepts**
Continuous delivery—Software development practice that allows developers to release software more quickly by automating the build, test, and deploy processes.

Pipeline—Workflow model that describes how software changes go through the release process. Each pipeline is made up of a series of stages.

Stage—Logical division of a pipeline, where actions are performed. A stage might be a build stage, where the source code is built and tests are run. It can also be a deployment stage, where code is deployed to runtime environments.

Action—Set of tasks performed in a stage of the pipeline. For example, a source action can start a pipeline when source code is updated, and a deploy action can deploy code to a compute service like AWS Elastic Beanstalk.

### Create the new pipeline

1. In a browser window, open the [AWS CodePipeline console](https://console.aws.amazon.com/codesuite/codepipeline/start?region=us-west-2).
2. Choose the orange **Create pipeline** button. A new screen will open up so you can set up the pipeline.
3. In the Pipeline name field, enter **Pipeline-DevOpsGettingStarted**.
4. Confirm that **New service role** is selected.
5. Choose the orange *Next* button.

### Configure Source Stage
1. Select *GitHub version 1* from the **Source provider** dropdown menu.
2. Choose the `white Connect` to GitHub button. A new browser tab will open asking you to give AWS CodePipeline access to your GitHub repo.
3. Choose the green *Authorize aws-codesuite* button. Next, you will see a green box with the message You have successfully configured the action with the provider.
4. From the Repository dropdown, select the repo you created in Module 1.
5. Select **main** from the branch dropdown menu.
6. Confirm that **GitHub webhooks** is selected.
7. Choose the orange **Next** button.

### Configure the build stage
1. From the **Build provider** dropdown menu, select `AWS CodeBuild`.
2. Under Region confirm that the US West (Oregon) Region is selected. *Remember all your resources need to be provisioned in the same region to avoid issues later on.*
3. Select *Build-DevOpsGettingStarted*  under Project name.
4. Choose the orange *Next* button.

### Configure the deploy stage
1. Select **AWS Elastic Beanstalk** from the Deploy provider dropdown menu.
2. Under Region, confirm that the **US West (Oregon) Region** is selected.
3. Select the field under Application name and confirm you can see the app **DevOpsGettingStarted** created in Module 2.
4. Select **DevOpsGettingStarted-env** from the Environment name textbox.
5. Choose the orange **Next** button. You will now see a page where you can review the pipeline configuration.
6. Choose the orange **Create pipeline** button.

![pipeine deployed](https://github.com/user-attachments/assets/911232f1-b3e6-4b09-ae99-95d1871f7c6b)

While watching the pipeline execution, you will see a page with a green bar at the top. This page shows all the steps defined for the pipeline and, after a few minutes, each will change from blue to green.

Once the Deploy stage has switched to green and it says Succeeded, choose AWS Elastic Beanstalk. A new tab listing your AWS Elastic Beanstalk environments will open.
Select the URL in the Devopsgettingstarted-env row. You should see a webpage with a white background and the text you included in your GitHub commit in Module 1.

![AWS deploy](https://github.com/user-attachments/assets/40497f06-2a73-49c8-b827-5c895a825ca1)

The content of my app
![Final web page view](https://github.com/user-attachments/assets/b0d7dfe7-24f5-45f2-bac2-ed6c8e99f029)


## Stage 5 - Finalize Pipeline and Test
**Key concepts**
Approval action — Type of pipeline action that stops the pipeline execution until someone approves or rejects it.

Pipeline execution —Set of changes, such as a merged commit, released by a pipeline. Pipeline executions traverse the pipeline stages in order. Each pipeline stage can only process one execution at a time. To do this, a stage is locked while it processes an execution.

Failed execution —If an execution fails, it stops and does not completely traverse the pipeline. The pipeline status changes to Failed and the stage that was processing the execution is unlocked. A failed execution can be retried or replaced by a more recent execution.

### Creating Review stage in pipeline
In order to be able to do manual approval of the pipeline, we need an additional stage to the pipeline stages, hence the need to add the `Review Stage`. The steps to do that are highlighted below:

1. Open the [AWS CodePipeline console](https://console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-west-2).
2. You should see the pipeline we created in Module 4, which was called **Pipeline-DevOpsGettingStarted**. Select this pipeline.
3. Choose the white **Edit button** near the top of the page.
4. Choose the white **Add stage** button between the Build and Deploy stages.
5. In the Stage name field, enter Review.
6. Choose the orange **Add stage** button.
7. In the Review stage, choose the white **Add action** group button.
8. Under Action name, enter **Manual_Review**.
9. From the Action provider dropdown, select **Manual approval**.
10. Confirm that the optional fields have been left blank.
11. Choose the orange **Done** button.
12. Choose the orange **Save** button at the top of the page.
13. Choose the orange **Save** button to confirm the changes. `You will now see your pipeline with four stages: Source, Build, Review, and Deploy.`

![Manual approval](https://github.com/user-attachments/assets/778a1ca6-b9fc-424b-aeae-db3529623bf2)

### Push a new commit to your repo 
**This stage is to basically test how the pipeline deals with code changes.**
1. In your favorite code editor, open the `app.js` file from Module 1.
2. Change the message in Line 5 to whatever you want.
3. Save the file.
4. Commit the change with the following commands:

```
git add app.js
git commit -m "Full pipeline test"``
```
5. Push to github

```
git push
```

### Monitor the pipeline and manually approve the change 

1. Navigate to the AWS CodePipeline console.
2. Select the pipeline named Pipeline-DevOpsGettingStarted. You should see the Source and Build stages switch from blue to green.
3. When the Review stage switches to blue, choose the white Review button.
4. Write an approval comment in the Comments textbox.

![Manual approval](https://github.com/user-attachments/assets/ddd63100-4209-49da-82bd-e8473d9eb171)

5. Choose the orange **Approve** button.
6. Wait for the **Review and Deploy stages** to switch to green.
7. Select the AWS Elastic Beanstalk link in the Deploy stage.
8. A new tab listing your Elastic Beanstalk environments will open.
Select the URL in Domain. You should see a webpage with a white background and the text you had in your most recent GitHub commit. Just like mine below.

![Finale output](https://github.com/user-attachments/assets/97963710-6cf3-4e8f-a975-0c540bbbfc90)



**Congratulations! You have a fully functional continuous delivery pipeline hosted on AWS.**


## Acknowledgments

Parts of this guide were adapted from the [AWS Getting Started Hands-On Guide](https://aws.amazon.com/getting-started/hands-on/create-continuous-delivery-pipeline/?ref=gsrchandson) with which I learnt. The original content is available on the [AWS website](https://aws.amazon.com/).
`



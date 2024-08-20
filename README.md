# Continuous-Delivery-Pipeline-in-AWS
I am about to walk you through how to develop a pipeline to aid smooth logging of changes to this application, which will require manual approval for the changes. We will create the pipeline using AWS CodePipeline, a service that builds, tests, and deploys your code every time there is a code change.

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
2. <span style="color: red;">Please take note to check the region where you're provisioning resources. I had issues later on when I had my service provisioned in different region, So I finally settled for US-West-2.</span>
3. Choose the orange **Create Application button**.
4. Choose **Web server environment** under the Configure environment heading.
   
![image](https://github.com/user-attachments/assets/bd88c26e-1624-46b6-9e13-4eb4f7715834)

4. In the text box under the heading Application name, enter **DevOpsGettingStarted**.

![image](https://github.com/user-attachments/assets/6770f7fe-a8ab-4a0a-8d74-e59e841e9ba5)


5. In the Platform dropdown menu, under the Platform heading, select **Node.js**. Platform branch and Platform version will automatically populate with default selections.
6. Confirm that the radio button next to Sample application under the Application code heading is selected.
7. Confirm that the radio button next to Single instance (free tier eligible) under the Presets heading is selected.
8. Select Next.

 



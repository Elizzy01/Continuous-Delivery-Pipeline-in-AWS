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

- [Step 1 - Setting Up Github REPO](#step-1---setting-up-github-repo)
- [Step 2 - Deploy Web App](#step-2---deploy-web-app)
- [Step 3 - Create Build Project](#step-3---create-build-project)
- [Step 4 - Create Delivery Pipeline](#step-4---create-delivery-pipeline)
- [Step 5 - Finalize Pipeline and Test](#step-5---finalize-pipeline-and-test)

Let's dig deeper into each of the steps..

## Step 1 - Setting Up Github REPO
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

## Step 2 - Deploy Web App

 



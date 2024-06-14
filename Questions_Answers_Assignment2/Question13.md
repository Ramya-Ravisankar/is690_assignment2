
13. **Describe the role of GitHub Actions in your project's CI/CD pipeline. How do you automate testing and deployment using GitHub Actions?**

### Answer
GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows us to automate your build, test, and deployment pipeline. We can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.

GitHub Actions is designed to bring platform-native automation and CI/CD capabilities directly into the GitHub flow to simplify the developer experience.


### Role of GitHub Actions in your project's CI/CD pipeline :
The role of GitHub Actions in our project's CI/CD pipeline is to automate testing of the code to ensure that our application (here fastapi) is functioning as expected and that it is being built and deployed automatically to upload the latest image to DockerHub.Testing is automated once the latest code changes are pushed to the main branch or there is a pull request to merge to the main branch. If testing succeeds, then the next step is to build and deploy the new image to DockerHub. The new image will be deployed to DockerHub if building and deploying is successful.

### 7 Workflow automation features with GitHub Actions:

1)  ## Creating workflows with sensitive data :
Secrets on GitHub are used to securely store sensitive data such as passwords, tokens, and certificates, among other things—and they can be directly referenced in workflows, too. This means you can create and share workflows with collaborators using secrets for secure values without hardcoding those values directly in YAML files.

2) ## Share data between jobs to create more advanced automations :
GitHub Actions supports sharing data between jobs in any workflow as artifacts, which are associated with the workflow run where they are created. This can help simplify the development of YAML workflow files. It also makes it easier to create more complex automations where one workflow informs another via dependencies or conditionals.

3) ## Track our workflows with workflow visualization :
All actions start and end with YAML files—but you can also access real-time workflow visualization graphs to track progress, understand dependencies and conditionals in more complex workflows, and troubleshoot any issues that come up via logs. Plus, workflow visualization graphics are color-coded to quickly show you what actions were successful, which are in progress, and which failed at a particular step.

4) ## Build breakpoints in a workflow with dependencies (or create dependencies between workflows):
GitHub Actions runs multiple commands at the same time by default—but you can leverage the needs keyword to create dependencies between jobs.

5) ## A conditional makes all the difference :
GitHub Actions supports conditionals, which use the if keyword to determine if a step should run in a given workflow. We can use this to build upon dependencies so that if a dependent job fails the workflow can continue running. These can use certain built-in functions for data operations.

6) ## Create different environments for development, staging, and production
GitHub Actions allows you to create environments with secure protection rules and secrets—and then reference those environments in a workflow job to leverage the environment’s protection rules and secrets. Example use cases include requiring a specific person or team to approve workflow jobs that reference an environment or restricting which branches can deploy to a given environment.

7) ## Use contexts to access workflow information
Contexts are a collection of variables used to access information about workflow runs, runner environments, jobs and steps to help derive key information about workflow operations. Contexts use expression syntax like ${{ }}. Most of them can be used at any point in the workflow.

### Automating and deploying workflows with GitHub Actions - Steps :
Add a deploy job to run after a successful test
Define permissions
Choose the deployment environment
Check out the repository with authentication
Set up Node and build the application for deployment
Configure Pages for deployment
Upload artifacts to Pages
Deploy to Pages

[Return back to answer.md](/answer.md)
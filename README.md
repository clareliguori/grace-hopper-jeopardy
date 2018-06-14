# Grace Hopper Jeopardy

## Run locally

Run the following:

    npm install
    npm run webpack

Then open index.html in your browser.

## Deploy on AWS

This application can be deployed to Amazon EC2 using AWS CloudFormation and AWS CodeDeploy.  It can be used as an example application in a workshop, with multiple stacks in the same account.

Create shared resources (create once):
```
aws cloudformation create-stack --stack-name my-workshop-shared-resources --template-body file://cloudformation_templates/shared_resources.yml --capabilities CAPABILITY_NAMED_IAM --parameters ParameterKey=WorkshopName,ParameterValue="my-workshop"

aws cloudformation wait stack-create-complete --stack-name my-workshop-shared-resources
```

Create website resources (can create multiple stacks for a workshop):
```
aws cloudformation create-stack --stack-name my-workshop-jeopardy-1 --template-body file://cloudformation_templates/application.yml --parameters ParameterKey=SharedResourceStack,ParameterValue="my-workshop-shared-resources"

aws cloudformation wait stack-create-complete --stack-name my-workshop-jeopardy-1
```

Go to the CodeDeploy console:
```
aws cloudformation describe-stacks --stack-name my-workshop-shared-resources --query 'Stacks[0].Outputs[?OutputKey==`CodeDeployUrl`].OutputValue' --output text
```

Select the CodeDeploy deployment group created for the new website stack (starts with my-workshop-jeopardy-1), and start a deployment:
* Go to Actions -> Deploy new revision
* Select "My application is stored in GitHub" for the Repository type
* Connect your account to GitHub if you have not already under the "Connect to GitHub" section.
* Enter "clareliguori/grace-hopper-jeopardy" for the repository name, and "52d25ef1715ff2cad71853da67329ce436eb57a0" for the commit ID

Once the deployment completes, go to the application URL:
```
aws cloudformation describe-stacks --stack-name my-workshop-jeopardy-1 --query 'Stacks[0].Outputs[?OutputKey==`Url`].OutputValue' --output text
```


## Credits
* Based on [React Trivia](https://github.com/ccoenraets/react-trivia)
* Grace Hopper clip art by [gingercoons](https://openclipart.org/detail/137533/grace-hopper)

## License

This library is licensed under the MIT License.
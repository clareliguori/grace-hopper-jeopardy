# Grace Hopper Jeopardy

## Run locally

Run the following:

    npm install
    npm run webpack

Then open index.html in your browser.

## Deploy on AWS

* Local bash terminal
* <a href="https://us-west-2.console.aws.amazon.com/cloud9/home?region=us-west-2">Cloud9</a> (Oregon)
* <a href="https://us-west-2.console.aws.amazon.com/cloud9/home?region=us-west-2">Cloud9</a> (Oregon)
* <a href="https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1">CloudFormation</a> (N.Virginia)
* <a href="https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1">CloudFormation</a> (N.Virginia)

This application can be deployed to Amazon EC2 using AWS CloudFormation and AWS CodeDeploy.  It can be used as an example application in a workshop, with multiple stacks in the same account.

Create shared resources (create once):
```
aws cloudformation deploy --stack-name ghc-workshop-shared-resources --template-file cloudformation_templates/shared_resources.yml --capabilities CAPABILITY_NAMED_IAM --parameter-overrides WorkshopName="ghc-workshop"
```

Create website resources (can create multiple stacks for a workshop):
```
aws cloudformation deploy --stack-name ghc-workshop-application-1 --template-file cloudformation_templates/application.yml --parameter-overrides SharedResourceStack="ghc-workshop-shared-resources"
```

Go to the CodeDeploy console:
```
aws cloudformation describe-stacks --stack-name ghc-workshop-shared-resources --query 'Stacks[0].Outputs[?OutputKey==`CodeDeployUrl`].OutputValue' --output text
```

Once the deployment completes, go to the application URL:
```
aws cloudformation describe-stacks --stack-name ghc-workshop-application-1 --query 'Stacks[0].Outputs[?OutputKey==`Url`].OutputValue' --output text
```

Cleanup:
```
Delete S3 objects for CodeSuite before deleting CloudFormation stacks

Delete Stacks:

aws cloudformation delete-stack --stack-name ghc-workshop-shared-resources

aws cloudformation delete-stack --stack-name ghc-workshop-application-1
```

## Credits
* Based on [React Trivia](https://github.com/ccoenraets/react-trivia)
* Grace Hopper clip art by [gingercoons](https://openclipart.org/detail/137533/grace-hopper)
* Based on [grace-hopper-jeopardy](https://github.com/clareliguori/grace-hopper-jeopardy)

## License

This library is licensed under the MIT License.

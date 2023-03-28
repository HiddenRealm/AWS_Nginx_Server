# AWS_Nginx_Server
AWS Cloudformation for a simple Nginx Server

# Parameters
  The Parameters as they are won't work if you tried to recreate this in your account.
  You need to update the VPC ID to be the one in the account your want to re-create this in.

# Run:
  You can either run the cloudformation manually thorugh the CloudFormation service (You will have to fill out the parameters youself). 
  Or you can use the AWS command line to pass boht files through to cloudformation.
  
    aws cloudformation create-stack --stack-name nginx-server --template-body file://james_ford_test_ec2.yaml 
    --parameters file://james_ford_test_ec2_params.json --capabilities CAPABILITY_NAMED_IAM --region eu-west-1

# Possible Improvements:
  As this was just requesting a simple EC2 that loads up an Nginx Server, I didn't want to add a whole load of other services to it as 
  I would be uploading more then one template, some of the services aren't part of the AWS free mode & Some might just be unnesscary depending other services you use.
  However I thought it best to atleast include some thoughts that come to mind when I was creating this
  
  ### Nginx Server
    The website that is currently loaded into Nginx is just a one page site with nothing going on, as well as being written in the userdata of the instance.
    2 easy solutions for this if you wanted a more complex site.
      1. S3 - Using S3 to store the more complicated code, you could instead have the userdata pull the files from S3 & load them into Nginx on launch.
      2. Docker - Using Docker you could have a pre-built container that is set up will Nginx being loaded in with the correct site, that you will just have to pull down the repo in the userdata. I did actually originally have this code do that & still have the docker repo that I was going to use, however then I realised I would have to save my personal dockerhub login on AWS & I didn't feel comfortable doing that.
    
  ### Infrastructure
    A
    
  ### Security
    A
    

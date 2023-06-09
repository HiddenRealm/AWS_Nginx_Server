# AWS_Nginx_Server
AWS Cloudformation for a simple Nginx Server

# Parameters
  The Parameters as they are won't work if you tried to recreate this in your account.
  You need to update the VPC ID to be the one in the account your want to re-create this in.

# Run:
  You can either run the CloudFormation manually through the CloudFormation service (You will have to fill out the parameters yourself). 
  Or you can use the AWS command line to pass both files through to CloudFormation.
  
    aws cloudformation create-stack --stack-name nginx-server --template-body file://james_ford_test_ec2.yaml 
    --parameters file://james_ford_test_ec2_params.json --capabilities CAPABILITY_NAMED_IAM --region eu-west-1

# Possible Improvements:
  As this was just requesting a simple EC2 that loads up an Nginx Server, I didn't want to add a whole load of other services to it as 
  I would be uploading more than one template, some of the services aren't part of the AWS free tier & Some might just be unnecessary depending on other services you use.
  However, I thought it best to at least include some thoughts that come to mind when I was creating this
  
  ### Nginx Server
  The website that is currently loaded into Nginx is just a one-page site with nothing going on, as well as being written by the userdata of the instance.
  Two easy solutions for this if you wanted a more complex site.
  
  1. S3 - Using S3 to store the more complicated code, you could instead have the userdata pull the files from S3 & load them into Nginx on launch.
  2. Docker - Using Docker you could have a pre-built container that is set up with Nginx being loaded in with the correct site, so you will just have to pull down the repo in the userdata. I did actually originally have this code do that & still have the docker repo that I was going to use, however then I realised I would have to save my personal docker hub login on AWS & I didn't feel comfortable doing that.
    
  ### Infrastructure
  The infrastructure that this EC2 site is in currently is non-existent. No room to grow, no real security, no updates & no backups. If you wanted to make this into a full-blown website you will probably want to add some of the following;
    
  1. AutoScaler - Having an auto scaler will allow your environment to spool up more EC2s if the current ones can't keep up with demand. They will also terminate additional EC2s down to a 'minimum' amount depending on current demand.
  2. Load Balancer - The load balancer will allow your network to split traffic over all of the created EC2s that are selected, this works nicely with the auto scaler as new instances can be added to the load balancer automatically.
  3. Schedules - Using CloudWatchs events/EventBridge you can set up schedules that run on timed intervals for several tasks, a good use of this would be to use the events to either backup EC2s or take a snapshot - depending on requirements
  4. Alarms - Using AWS Cloudwatch alarms, you can monitor the EC2s using several built-in metrics i.e. DiskSpace, CPU Utilization & System Recovery. However, you can also install the AWS cloudwatch agent into the instance which will allow you to output more information to the logs. This data can then be used as a custom metric. For example, you could have set up the instance to run 'sudo service nginx status' every 15 minutes & output the result to the agent. Then create an alarm using this metric to monitor if the daemon ever fails.
  5. Systems Manager - Systems manager has a built-in Patching manager which can be set up to monitor and distribute patches to all instances.
    
  ### Security
  There is no real security in this setup a few things that could be used are;
  
  1. Security Groups - Although I did add a 'custom' security group it was generic and only added a port 80 ingress to the default SG that AWS creates. More fleshed-out Security groups and subnets would be needed for a more secure network.
  2. IAM Role - I never created or used an IAM Role for this instance as it currently doesn't need to interact with anything else, however, if this were to be used it would need the appropriate permissions for that.
  3. KMS - I would have used a KMS key for encrypting the instance if it was available in the free tier. If I would turn this into a live fleshed-out site, then I would use KMS Keys all over, one for this instance, one for the S3 buckets, one for the DB etc.

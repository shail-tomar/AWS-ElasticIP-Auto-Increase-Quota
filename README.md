# AWS-ElasticIP-Auto-Increase-Quota
#Functionality
As we know that the Soft limit of Elastic IPs Per region is 5 but we can modify it using Service Quota from console as well as with SDK APIs. 
This script automatically detect the EIP usage in every region and increase the limit before it runs out of quota.

### Process
1. Create one Lambda function having permission in its role which are: 
    a. AmazonEC2FullAccess :- 
        This permission used to count Elastic IPs from Differnet Region.
    b. CloudWatchFullAccess :-
        This permission used to access Clodwatch Logs.
    c. AmazonSNSFullAccess :-
        This permission used to create SNS Topic.
    d. AWSCloudTrail_Access :- 
        This permission used to our lambda can access Cloudtrail.
    e. ServiceQuotasFullAccess :- 
        This permission used to once we reach our threshold Elastic IPs Usage then we can sent request to increase service quota limits. 

    Functionality of Lambda
        a. Lambda will pick region from Cloudwatch Logs for recent "AllocateAddress" API call.
        b. Calculate total number of Elastic IP present on that region.
        c. If number of Elastic IP usage is grater then 80% from applied quota value then it will sent sns for 80% or more usage.
        d. Send Service Quota Increase Request

2. Create CloudTrail with multiple region avaliable and create log groups from CloudTrail. Which will capture all API calls and sent it 
   to Cloudwatch Logs.  

3. Create Log Group Subscription "AllocateAddress" in Log Group which is created by CloudTrail and attach Lambda Function which we 
   created earlier. When ever any user call "AllocateAddress" API it will be captured by Cloudtrail and these logs will move to log group.
   and these log group use to call our lambda function. 

4. Create SNS Topic and Add subscription using Emails for getting notification in Email that we increase Service limit in 
   Particular region. 

5. Once the hole things are implemented then when ever user create Elastic IP in any region our Lambda will triggered using Cloud Logs.

6. Our Lambda will calcualte current usage of Elastic IP's in paticular regoin using CloudWatch Logs.

7. If Current usage is grater then 80% then our Lambda Funciton sent request to Service Quota to increase the limit.

#### You will need:

* An AWS Account with necessary permissions.
* Lambda Function with required permissions to perfrom action on the services.

### Download
Download the Script and save it in your Lambda Function.

### Build the solution
1. Login to AWS Account.
2. Create Lambda Funciton
3. Download Script
4. Save Script in AWS Lambda.

###OUTPUT
1. This Script will check Elasti IP's Usage from particular regions where Elastic IP is created.
2. It Will increase Service Limit based on Condition.

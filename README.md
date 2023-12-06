# Create and Apply S3 Bucket Policies with Conditions to Restrict Specific Bucket Permissions

## Project Objectives:

1.  Configure a bucket policy that will restrict what a user can do within an S3 bucket based upon their IP address.

2.  Configure a bucket policy to only allow the upload of objects to a bucket when server side encryption has been configured for the object.

## Step 1: Login to the AWS console.
login to your AWS account with your credentials.

## Step 2: Creating an Amazon S3 bucket
1. In the AWS Management Console search bar, enter S3, and click the S3 result under Services
2. From the S3 console, click Create bucket.
3. Under General configuration, enter the following values:  
    - Bucket name: Enter a unique name beginning with "unique-bucket-name"
    - AWS Region
4. Make sure to select ACLs Enabled.
5. Leave the Block public access (bucket settings) at the default values
6. To finish creating your bucket, scroll to the bottom, and click on Create bucket.

In this step, we created an Amazon S3 bucket.

## Step 3: Creating a Bucket Policy in Amazon S3 with IP Address Conditions
1. In the Amazon S3 console, select the bucket beginning with "unique-bucket-name" that you created earlier.

2. Click the Permissions tab, then scroll down to the Bucket Policy section and click the Edit button.
From here, you can type in a JSON based policy directly, or use AWS Policy generator.

3. At the top, click Policy generator.
4. Fill out Step 1: Select Policy Type as follows:   
    *  Select Type of Policy: Select S3 Bucket Policy
    
5. Under Step 2: Add Statement(s), enter and select the following:
    
    * Effect: Select the Deny radio button
    * Principal: Enter *
    * Actions: Check PutObject 
    * ARN: You will enter your bucket ARN on Edit bucket policy page
    
6. Still within Step 2: Add Statement(s), click Add Conditions (Optional).fill out follows:
    
    * Condition: Select NotIpAddress
    * Key: Select aws:SourceIp
    * Value: Enter 1.2.3.4 
    
7. Click  Add Condition button when ready to proceed.

8. Click Add Statement, and then Generate Policy when ready to proceed.

9. Select and copy the Policy JSON Document generated for you.

10. Return to your browser tab with the Bucket Policy editor open and paste the JSON into the Policy editor, copy and paste the Bucket ARN into Resource, and append /*.
    
    * Important: Make sure the ARN on the Resource line matches the name of the Amazon S3 bucket you created earlier.

    * Important: Ensure you add the slash and the asterisk at the end of the ARN to have the policy apply to objects in the bucket.
    

11. To save the policy, scroll to the bottom and click Save changes.

Recall that the condition within your policy specifies actions on your S3 bucket from anyone other than Source IP of 1.2.3.4 will be denied. (Clearly your local host IP address is not 1.2.3.4)


### Test your bucket policy:

12. Scroll to the top, click the Objects tab, and click Create folder.

13. Enter a folder name and then at the bottom, click Create folder. 

Because your source IP address is not 1.2.3.4 you will receive an error


14. Click Cancel to exit the form, and then click Upload, and Add files.(Select a few files from your local file system)

15. Scroll to the bottom and click Upload.

Once again, the operation (an upload in this example) failed, as you do not have permission to perform any actions because your IP address does not match the bucket policies condition (source IP 1.2.3.4).


16. To close the Upload form, click Close.

In this Step we learned how to use the Amazon Policy Generator to generate the JSON to create a S3 bucket policy. The policy denied all S3 actions from any source where the IP address was not 1.2.3.4. 

## Step 4: Create a Bucket Policy in S3 with Encryption Conditions

1. From the Amazon S3 console, select the calabs-bucket bucket you created earlier

2. Click the Permissions tab, scroll down to the Bucket policy section and click Edit.

3. Remove the existing policy from the editor:

4. To start generating a new policy, click Policy generator:

5. Fill out Step 1: Select Policy Type as follows:   
    - Select Type of Policy: S3 Bucket Policy
    

6. Fill out Step 2: Add Statement(s) as follows:
      
     * Effect: Select the Deny radio button
     * Principal: Enter *
     * Actions: Select PutObject
     * ARN: You will enter your bucket ARN  on Edit bucket policy page
    

7. Still within Step 2: Add Statement(s), click Add Conditions (Optional) and fill out as follows:
    
    * Condition: Select StringNotEquals
    * Key: Select s3:x-amz-server-side-encryption
    * Value: Enter AES256

    
Conditions allow you to define a greater granularity to your policy to only execute under certain conditions and keys.

8. Click  Add Condition button when ready to proceed.

9. Click Add Statement.
(You have created a bucket policy that will deny access for any object uploads if the objects do not have SSE encryption enabled)

10. In Step 3: Generate Policy, click Generate Policy when ready to proceed.

11. Select and copy the Policy JSON Document generated for you.

12. Return to your browser tab with the Bucket Policy editor open and paste the JSON into the Policy editor, copy and paste the Bucket ARN into Resource, and append /*.

13. To save your policy, scroll to the bottom and click Save changes.

Your S3 bucket now has a bucket policy applied. Recall that the condition within your policy specifies any files added to your bucket without encryption should be denied. 

### Test the bucket policy by attempting an upload of a file without encryption:

14. Click the Objects tab at the top, click Upload, and then Add files.

15. Select (or drag and drop) a file or two from your local file system, then click Upload at the bottom to upload the selected files.
You will see a notification that the upload has failed.  

### Test the bucket policy by attempting an upload of a file with encryption::

16. Click Close, Upload, then Add Files, and select a local file to upload.

This time you will progress through the Upload wizard in order to enable server side encryption before actually uploading the file.

17. With a file selected to be uploaded, move to the Properties section.

18. Scroll down to the Server-side encryption settings section and select Specify an encryption key.

For this test you will use the default Amazon S3 key (SSE-S3) encryption key type.

19. Scroll down to the bottom and click Upload.

This time the file is uploaded to your S3 bucket, because the bucket policy was not violated.

In this Step we enabled a S3 bucket policy that would deny any file uploads to your bucket of any objects that do not have server side encryption enabled. we tested the policy by uploading files that did not have encryption enabled, and others that did. Further, we learned where to look for errors in the S3 console if S3 operations (such as a file upload) fails.






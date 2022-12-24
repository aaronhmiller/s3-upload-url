# s3-upload-url
API Gateway + Lambda + S3 so you can securely have anyone upload a file to you

### To setup:

1. create a new Lambda (Node JS 16)

2. paste the `app.js` code into it

3. create an S3 bucket

4. add the bucket's name as an environment variable to your Lambda named `UploadBucket` 

5. in your Lambda's Permissions, to the default role created for you, add a new inline policy like this:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "LambdaToS3",
               "Effect": "Allow",
               "Action": "s3:PutObject",
               "Resource": "arn:aws:s3:::<YOUR_BUCKET_NAME>/*"
           }
       ]
   }
   ```
6. create a `Function URL` to run the Lambda

7. alternately, create a Trigger of an AWS API Gateway

### To use:

1. Load the Function URL (or API Gateway endpoint trigger)
2. Take the "uploadURL" and use it to upload a file to S3

### Example:

Get upload URL:

`curl <function url> | jq .uploadURL`

Upload the file:

`curl --upload-file <file> <uploadURL>`

or

`http put <uploadURL> @file`

Check S3 for the file (it'll be uniquely named each time).

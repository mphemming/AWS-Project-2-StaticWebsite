# AWS-Project-2-StaticWebsite

## Intro

This is a repository that will contain code and steps for hosting a website using AWS S3.

Project progress can be tracked here: [https://github.com/users/mphemming/projects/2/views/1](https://github.com/users/mphemming/projects/3/views/1)

## Project information

The below sections will contain notes and information useful to replicate the project process.

### IAM user

MFA was enabled for the root user as recommended, and an IAM user 'Michael_developer' with console access and full admin rights was created. This IAM user will be used to work on this project.
The password for this IAM user is stored in Michael's Dashlane account. 

### Regions

I am working within the ap-southeast-2 (Sydney) region. 

## Host a simple website on AWS S3

### Create an S3 bucket to host the website

An S3 bucket was created with the standard option within the ap-southeast-2 (Sydney) region spread across >= 3 AZs. This bucket is called 'aws-project-2-staticwebsite'.

The simple website file 'index.html' was added to the S3 bucket. This simple website displays 'Hello World'. 

### Configure the S3 Bucket for Static Website Hosting

**Enable Static Website Hosting:**

1. Click on the "Properties" tab in your S3 bucket.
2. Scroll down to the "Static website hosting" section and click "Edit".
3. Select "Enable".
4. For the "Index document", enter index.html.
5. Click "Save changes".

**Turn off 'Block public access' (if enabled when creating the S3 bucket):**

1. Click on the "Permissions" tab in your S3 bucket.
2. Click on "Block public accessy" and then "Edit".
3. Uncheck the box for 'Bloack _all_ public access.
4. Click "Save changes".

**Set Bucket Policy for Public Access:**

1. Click on the "Permissions" tab in your S3 bucket.
2. Click on "Bucket Policy" and then "Edit".
3. Add the following bucket policy to allow public read access:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::aws-project-2-staticwebsite/*"
    }
  ]
}
```
4. Click "Save changes".

**Enable Static Website Hosting:**

1. Click on the "Properties" tab in your S3 bucket.
2. Scroll down to the "Static website hosting" section and click "Edit".
3. Select "Enable".
4. For the "Index document", enter index.html.
5. Click "Save changes".

**Access Your Website - get the Website URL:**

1. Go back to the "Properties" tab.
2. In the "Static website hosting" section, you will see the "Bucket website endpoint".
3. In my case, the URL is http://aws-project-2-staticwebsite.s3-website-ap-southeast-2.amazonaws.com
4. Open the provided URL in a web browser to see the "Hello World" page.

## Add user/login capability to access a private webpage


## Update the website to use a BootStrap template

## Notes

The website URL might not work at first relating to http vs https. Changing the URL to 'https' usually works. 



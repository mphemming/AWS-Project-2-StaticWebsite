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


:information_source: **Note that 'index.html' used here was renamed to 'index.html.legacy' in order to setup a new 'index.html' in the next section.**

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


> :information_source: **Note that 'index.html' used here was renamed to 'index.html.legacy' in order to setup a new 'index.html' in the next section.**


## Add user/login capability to access a private webpage

**Create a User Pool using AWS Cognito**

1. create a user pool via the Cognito console with the following settings:

* Users can sign-in using either a username or email address
* The minimum password length is 8 characters, and must contain at least 1 number, and at least one upper and lower case letter.
* The temporary password expires after 7 days
* no multi-factor authentication required (although this is an option if better security needed)
* "forgot your password" option available to users, delivered via email
* disable self-registration option so that only a select few can access information (organised by website admin)
* Allow Cognito to automatically send email messages to verify and confirm (recommended)
* the following attributes are required when a new user is created: birthdate, address, given_name, family_name, phone_number, preferred_username. There is also an option to create custom attributes but I left this blank for now.
* send email with Cognito, but replies are forwarded to my personal email address (temporary)
* user pool name 'StaticWebsite'
* Public app client created called 'StaticWebsite'

For the next steps, this Youtube video was very helpful: https://www.youtube.com/watch?v=8a0vtkWJIA4

2. Upload 3 html scripts to the same S3 bucket 'aws-project-2-staticwebsite': 'index.html', 'logged_in.html', and 'logged_out.html'. These are found in this repo and all 3 will be used for setting this up.
3. I used the Hosted UI for the login interface. To set this up you will need to do the following:
* Navigate to the App client 'StaticWebsite'
* Edit the 'Hosted UI' section to include a callback URL as ```https://aws-project-2-staticwebsite.s3.ap-southeast-2.amazonaws.com/logged_in.html``` and the allowed sign-out URL as ```https://aws-project-2-staticwebsite.s3.ap-southeast-2.amazonaws.com/logged_out.html```.
* Select 'Cognito user pool' as the identity providers
* Select 'Authorization code grant' for OAuth 2.0 grant types
* select 'OpenID' and 'Email' for the OpenID connect scopes
* Add a domain name for the hosted UI. You can call it anything, but it has to be unique. 
4. After saving changes, click on the 'view hosted UI' button to test the login interface.
5. Copy the URL for this hosted UI into the three html scripts uploaded in the S3 bucket, making sure to change some parts of the URL for 'logged_in.html'. 
6. create a user to test the login mechanism. You do this by going to the Cognito user pool and then click on 'create user'. An email will be sent with the username and temporary password, and then once logged in the user will have the opportunity to input required info and change the password. Note that I did not receive an email when I used the same email for the user as used for the user pool email setup. 

## Access data from DynamoDB table on secure webpage

## Update the website to use a BootStrap template

## Notes

The website URL might not work at first relating to http vs https. Changing the URL to 'https' usually works. 
For Cognito, it is better to not use the same email address for the testing user as used when creating the user pool



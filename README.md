# How to


## Setup a domain in AWS
1. Buy a domain and create an AWS account
2. TODO: host the domain in route53

## Create an S3 bucket to host your html
Go to AWS and into S3

Create an S3 bucket with the following settings:
1. Set "Block all public access" to "Off"
2. Under "properties", enable "Static website hosting"
3. Then under "Index document", in the static website settings, write "index.html"

## Cloudfront
Go to AWS and into Cloudfront

Create a distribution
1. Under "Origin domain", select your s3 bucket
2. Choose "Use website endpoint"
3. You can select "Do not enable security protections"

Once the distribution has been created, go to settings and edit it
1. Under "Alternate domain name (CNAME) - optional" set your domain that you bought
2. Under "Default root object - optional" write "index.html"

Take note of the Distribution Id!


## Create the permissions to change the bucket for github actions
Go to AWS and into IAM

Got to users and create a user

Create a policy for the user when asked and copy this one

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::arn:aws:s3:::<name-of-your-bucket>"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::<name-of-your-bucket>/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudfront:CreateInvalidation"
            ],
            "Resource": "*"
        }
    ]
}
```

Then, once the user is created, go to the user and create an access key:
1. In "Access key best practices & alternatives" select "Application running outside AWS"
2. In "Set description tag" write "github-action"
3. In "Retrieve access keys" copy both the access key and the secret to a file for the time being

## Add access for github to your s3 bucket
Go to your github repo -> settings -> secrets and variables -> actions

Now take the
- s3 bucket name
- aws region you created your bucket
- distribution id
- access key
- access secret

and create a secret for each one named:
- S3_BUCKET
- AWS_REGION
- CLOUDFRONT_DISTRIBUTION_ID
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
respectively.



TODO: Add these to github secret

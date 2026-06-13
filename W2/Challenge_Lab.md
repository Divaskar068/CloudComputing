**Dynamic** - sybmit online order - some backend operation exists - you can select preferences
-involves apache https - connect to Db

# Static website:
mostly hosted using github
shows html files as a website on http port - no need for backend or any other services

- If files are smaller than 500kb - better to archive them into one big before trasfer -> cheaper
  more expensive to send many small ones


create rules -> lifecycle configurations

what we did :
rule 1 : Version 30 days IA IA-Infrequent Access

->
Expire current versions of objects - meaning if not accessed for some time will be deleted


Bucket policy :

```
{
    "Version": "2012-10-17",                    
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:Get*"
            ],
            "Resource": [
                "arn:aws:s3:::daru0755-bucket/*"
            ]
        }
    ]
}
```
This is basically permissions for all the services you open in the same bucket 

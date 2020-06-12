# BBB-AWS-S3

This script is written in python, and it keeps migrating your all recorded videos/sessions from Big Blue Button server to your desired s3 bucket. 

# Installation process & AWS Environment Setup
##### Run these all commands line by line
```
$ cd /var/bigbluebutton/published/presentation
$ sudo su
$ export LC_ALL=C
$ apt install python-pip
$ pip install awscli
$ pip install boto3
$ pip install python-magic==0.4.15
$ wget https://raw.githubusercontent.com/rohitcoder/BBB-AWS-S3/master/bbb-s3.py
```
### Get ready with AWS keys, How to get AWS Access Key ID and Secret Access Key ?
1. Follow this video https://youtu.be/665RYobRJDY
2. Note: At this step https://youtu.be/665RYobRJDY?t=101 you have to search "S3 Full Access" and then go ahead.

## Now, Let's setup our AWS CLI
```
$ aws configure
  AWS Access Key ID [None]: PASTE_AWS_KEY_ID_HERE
  AWS Secret Access Key [None]: PASTE_AWS_SECRET_HERE
  AWS region name [None]: Press enter without typing anything
  Default output format [None]: Again, press enter without typing anything
```

![AWS CLI](https://raw.githubusercontent.com/rohitcoder/BBB-AWS-S3/master/Screenshot%202020-06-12%20at%2011.46.12%20PM.png)

Now, Lets edit ```bbb-s3.py```

open ```bbb-s3.py``` with any editor and edit value for BUCKET_NAME, DELETE_SERVER_FILES.

*DELETE_SERVER_FILES* => SET value "True"  if you want to delete recordings from Local EC2 instance after gettting uploaded to s3, default value is False

Now, lets configure our s3 path with BBB
 
```
$ cd /var/bigbluebutton/playback/presentation/2.0/lib
$ vi writing.js
```
Here you need to change value of url to your s3 bucket address and add meetinID variable at end.

![Editing Writing.js](https://raw.githubusercontent.com/rohitcoder/BBB-AWS-S3/master/Screenshot%202020-05-25%20at%2010.56.37%20PM.png)

##### Now, final step.

1. Make sure your bucket is publicly accessible.
2. Open your s3 bucket settings in aws console, and click on Permissions->CORS configuration and add this

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>HEAD</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

Now, you need to setup a cron to automate this upload process
```
$ crontab -e
```
add this to your crontab 

This will run bbb-s3.py after every 5 minutes.
```*/5 * * * * /usr/bin/python /var/bigbluebutton/published/presentation/bbb-s3.py```

It should look like this

![](https://raw.githubusercontent.com/rohitcoder/BBB-AWS-S3/master/Screenshot%202020-05-25%20at%2011.30.47%20PM.png)


Now, you are ready to go, it should work on your side!

For any issues, you can get back to me via LinkedIn or Twitter! 🤟

You can follow me or add me on https://twitter.com/@rohitcoder & https://linkedin.com/in/rohitcoder

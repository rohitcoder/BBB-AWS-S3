# BBB-AWS-S3

This script is written in python, and it keeps migrating your all recorded videos/sessions from Big Blue Button server to your desired s3 bucket. 

# Installation process
##### Run these all commands line by line
```
$ cd /var/bigbluebutton/published/presentation
$ sudo su
$ export LC_ALL=C
$ apt install python-pip
$ pip install boto3
$ pip install python-magic==0.4.15
$ wget https://raw.githubusercontent.com/rohitcoder/BBB-AWS-S3/master/bbb-s3.py
```
Now, Lets edit ```bbb-s3.py```

open ```bbb-s3.py``` with any editor and edit values for ACCESS_KEY, SECRET_KEY, BUCKET_NAME

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

# s3upload

<br>
<div style="text-align:center;"><img src="./logo.svg" width=500></div>
<br>

With this simple program, you can upload files to Amazon Web Services(AWS) S3 using one command. It uploads the files, makes them public, and then prints their URLs.

s3upload uses [Boto 3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) to deal with AWS S3.

## Prerequisites

This program requires these libraries to be installed:

- Boto 3: install with `pip install boto3`.

- requests: install with `pip install requests`.

### AWS Credentials

For s3upload to be able to connect to your AWS account, you need to add your AWS credentials. It is a simple process.

1. Go to [AWS IAM Console](https://console.aws.amazon.com/iam/home). 

2. Create a new user or use an existing user. 

3. Generate a new set of keys for the user.

4. Create a file named `credentials` in the directory `~/.aws/` (Create `~/.aws/` if it's not already there).

5. Put the following in `credentials` file:

```
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```
You're done!

## Usage

```
s3up LOCAL_DIRECTORY BUCKET_NAME [-d DEST_DIRECTORY] [-ext FILES_EXTENSIONS]
```

where:

- LOCAL_DIRECTORY is the path to the local directory
which contains the local files to be transferred.

- BUCKET_NAME is the name of the destination S3 bucket.

- DEST_DIRECTORY (optional) is the path inside the destination
bucket that the files need to be transferred to. Note that
it should not start with '/'. If it is not specified, 
files will be uploaded to the main bucket directory.

- FILES_EXTENSION (optional) is the extensions of the files in
LOCAL_DIRECTORY that need to be transfered. Extensions
should be separated by ',' only. If FILES_EXTENSION is
not specified, all files in the directory are uploaded
(except files whose names start with '.').

This program uploads files only; folders are ignored.

>Enclose all arguments with quotation marks, as shown
in the example below.

## Example

```
s3up "/Users/abc/xyz/" "bucket_3" -d "2018/Nov/" -ext "png,csv"
```

This will upload all png and csv files in the local directory 'xyz' to the directory '2018/Nov/' inside bucket_3.

## License

My work is licensed under MIT.
Copyright (c) 2018 Ammar Alyousfi

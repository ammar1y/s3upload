# s3upload

<img src="./logo.svg" width=500>

With this simple program, you can upload files to Amazon Web Services(AWS) S3 using one command. It uploads the files, makes them public, and then prints their URLs.

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

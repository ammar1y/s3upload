#! /usr/bin/env python3

"""
This program is used to upload files to AWS S3. It makes
the uploaded files public, and prints their URLs.

Usage: s3up LOCAL_DIRECTORY BUCKET_NAME [-d DEST_DIRECTORY] [-ext FILES_EXTENSIONS]
where:
    LOCAL_DIRECTORY is the path to the local directory
    which contains the local files to be transferred.
    BUCKET_NAME is the name of the destination S3 bucket.
    DEST_DIRECTORY (optional) is the path inside the destination
    bucket that the files need to be transferred to. Note that
    it should not start with '/'. If it is not specified, 
    files will be uploaded to the main bucket directory.
    FILES_EXTENSION (optional) is the extensions of the files in
    LOCAL_DIRECTORY that need to be transfered. Extensions
    should be separated by ',' only. If FILES_EXTENSION is
    not specified, all files in the directory are uploaded
    (except files whose names start with '.').

This program uploads files only; folders are ignored.

Enclose all arguments with quotation marks, as shown
in the example below.

Example:
s3up "/Users/abc/xyz/" "bucket_3" -d "2018/Nov/" -ext "png,csv"
This will upload all png and csv files in the local directory 'xyz' 
to the directory '2018/Nov/' inside bucket_3.
"""

import boto3
import os
import requests
import sys

cmd_args = sys.argv

if len(cmd_args) == 2 and ('-h' in cmd_args or '--help' in cmd_args):
    print(__doc__)
    sys.exit()

# create a resource instance
s3 = boto3.resource('s3')
# the path to the local directory which contains
# the local files to be transferred
local_directory = cmd_args[1]
# the name of the destination bucket on S3
bucket = cmd_args[2]
if len(cmd_args) > 3:
    if '-d' in cmd_args:
        d_index = cmd_args.index('-d')
        # the destination folder in the destination bucket
        dest_directory = cmd_args[d_index + 1]
    else:
        dest_directory = ''
    if '-ext' in cmd_args:
        ext_index = cmd_args.index('-ext')
        extensions = tuple(cmd_args[ext_index + 1].split(','))
        files_list = [
            x for x in os.listdir(local_directory) if (
                not x.startswith(".") and
                os.path.isfile(os.path.join(local_directory, x))
                and x.endswith(extensions))
        ]
    else:
        files_list = [
            x for x in os.listdir(local_directory) if (
                not x.startswith(".") and
                os.path.isfile(os.path.join(local_directory, x)))
        ]
# get the code of the region where the destination
# bucket is stored
bucket_location = s3.meta.client.get_bucket_location(
    Bucket=bucket)['LocationConstraint']
# loop through the desired source files
for f in files_list:
    # get source file path
    src_path = os.path.join(local_directory, f)
    # specify the destination path inside the bucket
    dest_path = os.path.join(dest_directory, f)
    # upload the file and make it public
    s3.meta.client.upload_file(src_path, bucket, dest_path,
                               ExtraArgs={'ACL': 'public-read'})
    print('Uploaded', f, 'with URL:')
    # get the url of the uploaded file. Prepare (Encode) it if necessary
    object_url = "https://s3-{0}.amazonaws.com/{1}/{2}".format(
        bucket_location, bucket, dest_path)
    print(requests.Request('GET', object_url).prepare().url)
    print('=' * 30)

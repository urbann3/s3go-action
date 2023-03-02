# s3go-action
Github Action that can be used for upload or download of files to AWS S3 bucket using concurrency. As it can spin multiple processes the upload and download of the files is much quicker. The original code is written in golang and this repository is containing the binaries which are then called by the Github action. 

## Input parameters

|   Parameter   |       Mode      | Default  |                        Description                     |
|---------------|-----------------|----------|--------------------------------------------------------|
|     `mode`    |                 |  upload  | Operating mode, can be download or upload.
|  `localPath`  |      upload     |    "."   | Path of the local object to upload to S3. 
|  `localPath`  |     download    |    "."   | Path where to download the object from S3.
|    `bucket`   | download/upload |          | S3 bucket name.
|    `prefix`   |      upload     |          | Path of the object being uploaded.
|    `prefix`   |     download    |          | Path of the object being downloaded. 
| `concurrency` | download/upload |    10    | Number of concurrent connections to use.
|  `queueSize`  | download/upload |    100   | Size of the download/upload queue.
|   `exclude`   |      upload     |          | multi-line list of relative paths to exclude for upload.

### Requirements

In order for a proper run, this action require: 
- Environment variable `AWS_REGION` to be set with the value of the region where the S3 bucket is created. 
- Working AWS user with valid credentials and permissions to write to the S3 bucket, we recommend using [GitHub's OIDC provider](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services) to get short-lived credentials needed for your actions. 

### Upload mode examples

Uploading entire directory to S3:

```yaml
- uses: urbann3/s3go-action@v1
  with:
    mode: upload
    bucket: myBucket
    prefix: myBucketPrefix
    concurrency: 50
    queueSize: 1000
```

Upload but exclude some directories:

```yaml
- uses: urbann3/s3go-action@v1
  with:
    mode: upload
    bucket: myBucket
    prefix: myBucketPrefix
    concurrency: 50
    queueSize: 1000
    exclude: |
      .git
      .github
      src
      node_modules
```

Upload specific folder to S3:

```yaml
- uses: urbann3/s3go-action@v1
  with:
    mode: upload
    bucket: myBucket
    prefix: myBucketPrefix
    concurrency: 50
    queueSize: 1000
    localPath: path/to/folder
```

### Download mode examples

Download entire S3 bucket:

```yaml
- uses: urbann3/s3go-action@v1
  with:
    mode: download
    bucket: myBucket
    concurrency: 50
    queueSize: 1000
    localPath: path/to/folder
```

Download specific path only: 
```yaml
- uses: urbann3/s3go-action@v1
  with:
    mode: download
    bucket: myBucket
    prefix: myBucketPrefixToDownload
    concurrency: 50
    queueSize: 1000
    localPath: path/to/save/files
```


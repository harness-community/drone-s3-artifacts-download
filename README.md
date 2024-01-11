# drone-s3-artifacts-download
This plugin is designed to download objects/artifacts from an AWS S3 bucket. The primary goal of this plugin is to use AWS CLI to authenticate into an AWS account and download objects from an S3 bucket.

## Build

Build the binary for different OS/Arch with the following commands:

```bash
env GOOS=linux GOARCH=amd64 go build -o release/linux/amd64/drone-s3-artifacts-download .
env GOOS=linux GOARCH=arm64 go build -o release/linux/arm64/drone-s3-artifacts-download .
env GOOS=windows go build -o release/windows/amd64/drone-s3-artifacts-download.exe   
```


## Docker

Build the Docker image with the following commands. Using the following command, the image can be built for different OS and architecture. 

```
docker build -t DOCKER_ORG/drone-s3-artifacts-download -f PATH_TO_DOCKERFILE
```

## Usage

Use the following command to run the container using docker
```bash
docker run --rm \
-e PLUGIN_AWS_ACCESS_KEY_ID=<"YourAccessKeyId"> \
-e PLUGIN_AWS_SECRET_ACCESS_KEY=<"YourSecretAccessKey"> \
-e PLUGIN_AWS_DEFAULT_REGION=<"YourAWSRegion"> \
-e PLUGIN_AWS_BUCKET_NAME=<"YourS3BucketName"> \
-e PLUGIN_FETCH_DIR=<"YourFetchDirectory"> \
-e PLUGIN_DOWNLOAD_TARGET=<"YourDownloadTarget"> \
DOCKER_ORG/drone-s3-artifacts-download
```

In Harness CI, the following YAML can be used to implement the plugin as a step
```yaml
              - step:
                  type: Plugin
                  name: drone-s3-artifacts-download
                  identifier: drones3artifactsdownload
                  spec:
                    connectorRef: account.harnessImage
                    image: harnesscommunitytest/drone-s3-artifacts-download
                    settings:
                      aws_access_key_id: <+secrets.getValue("awsaccesskeyid")>
                      aws_secret_access_key: <+secrets.getValue("awssecretaccesskey")>
                      aws_default_region: ap-south-1
                      aws_bucket_name: mybucket
                      download_target: /harness
                      fetch_dir: mydir
```

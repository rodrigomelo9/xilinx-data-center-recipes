# Amazon Configurations

## Configuring the AWS CLI

> You will need an *Access Key ID* and a *Secreat Access Key*. Step to get them:
> * Go to the *AWS Management Console*.
> * Expand the `USER@ID` menu and select *security credentials*.
> * Go to *Access keys for CLI, SDK and API* and *Create a new access key*.

```bash
aws configure
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID_HERE
AWS Secret Access Key [None]: YOUR_SECREAT_ACCESS_KEY_HERE
Default region name [None]: YOUR_REGION_HERE
Default output format [None]: json
```

## Creation of a S3 Bucket and Folders

> This step can be donde graphically into the *AWS Management Console*.

```bash
aws s3 mb s3://<unique-bucket-name> --region <region>
aws s3 mb s3://<unique-bucket-name>/dcp
touch FILES_GO_HERE.txt
aws s3 cp FILES_GO_HERE.txt s3://<unique-bucket-name>/dcp/
aws s3 mb s3://<unique-bucket-name>/logs
aws s3 cp FILES_GO_HERE.txt s3://<unique-bucket-name>/logs/
```

> **NOTES:**
> * The trailing '/' is required after the folder name when cp.
> * When the AFI has been created successfully, you are free to delete the generated files on `dcp` and `logs`.

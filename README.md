# Search_Function

An AWS Lambda function that provides search capabilities via Amazon OpenSearch. The function accepts search queries and returns matching documents from an OpenSearch index.

## Project Structure

| File | Description |
|------|-------------|
| `lambda_function.py` | Lambda handler that queries OpenSearch and returns search results |
| `search-function.yaml` | AWS SAM/CloudFormation template defining the Lambda function |
| `buildspec.yml` | AWS CodeBuild build specification for packaging and deploying |

## How It Works

The Lambda function:

1. Receives form-encoded requests with a `searchTerm` parameter (body is base64-encoded)
2. Queries Amazon OpenSearch using a `multi_match` query on these fields: **Title**, **Author**, **Date**, **Body**
3. Returns up to 25 matching documents with **Title**, **Author**, **Date**, and **Summary** fields
4. Uses AWS Signature Version 4 authentication to access OpenSearch

**OpenSearch configuration:**
- Region: `ap-northeast-1`
- Index: `mygoogle`

## Deployment

The project uses AWS CodeBuild for deployment:

1. **Build**: The `buildspec.yml` runs `aws cloudformation package` to bundle the Lambda code and template
2. **Artifacts**: Packaged template (`search-output.yaml`) and source template are uploaded to the S3 bucket `wbt-search-engine-artifacts`

### Requirements

- Python 3.9 (Lambda runtime) / Python 3.10 (CodeBuild)
- Dependencies: `boto3`, `requests`, `requests_aws4auth`

## Lambda Configuration (search-function.yaml)

- **Runtime**: Python 3.9
- **Handler**: `lambda_function.lambda_handler`
- **Memory**: 128 MB
- **Timeout**: 300 seconds
- **IAM Role**: `arn:aws:iam::844028273225:role/lambda_s3`

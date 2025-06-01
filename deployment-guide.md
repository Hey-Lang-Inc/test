# Harness.io Deployment Guide

This guide provides step-by-step instructions for deploying Joe Lang's professional portfolio website using Harness.io and AWS S3.

## Overview

The deployment pipeline automatically:
1. Prepares website files for deployment
2. Uploads files to AWS S3
3. Configures S3 bucket for static website hosting
4. Provides the website URL for access

## Prerequisites

### 1. Harness Account Setup
- Active Harness.io account with Continuous Delivery module
- Organization and project permissions
- Access to create pipelines, connectors, and secrets

### 2. AWS Account Setup
- AWS account with programmatic access
- IAM user with S3 permissions
- Access Key ID and Secret Access Key

### 3. Required AWS IAM Permissions

Create an IAM policy with the following permissions:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "S3BucketManagement",
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:GetBucketWebsite",
                "s3:PutBucketWebsite",
                "s3:PutBucketPolicy",
                "s3:GetBucketPolicy",
                "s3:DeleteBucketPolicy"
            ],
            "Resource": "arn:aws:s3:::joe-lang-portfolio-*"
        },
        {
            "Sid": "S3ObjectManagement", 
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::joe-lang-portfolio-*/*"
        }
    ]
}
```

## Step-by-Step Setup

### Step 1: Configure Harness Secrets

1. **Navigate to Account Settings**
   - Go to Account Settings → Security → Secrets

2. **Create AWS Access Key Secret**
   - Click "New Secret" → "Text"
   - Name: `aws_access_key_id`
   - Description: "AWS Access Key ID for S3 deployment"
   - Secret Value: Your AWS Access Key ID
   - Click "Save"

3. **Create AWS Secret Key Secret**
   - Click "New Secret" → "Text"
   - Name: `aws_secret_access_key`
   - Description: "AWS Secret Access Key for S3 deployment"
   - Secret Value: Your AWS Secret Access Key
   - Click "Save"

### Step 2: Create Harness Project

1. **Create New Project**
   - Go to Projects → "New Project"
   - Name: "Joe Lang Portfolio"
   - Identifier: `joe_lang_portfolio`
   - Description: "Professional portfolio website for Joe Lang"
   - Click "Save"

### Step 3: Set Up AWS Connector

1. **Create AWS Connector**
   - Go to Project Setup → Connectors → "New Connector"
   - Select "AWS"
   - Name: "AWS S3 Connector"
   - Identifier: `aws_s3_connector`

2. **Configure Credentials**
   - Credential Type: "AWS Access Key"
   - Access Key: Select `aws_access_key_id` secret
   - Secret Key: Select `aws_secret_access_key` secret
   - Region: `us-west-2`

3. **Test Connection**
   - Click "Test Connection"
   - Verify successful connection
   - Click "Save"

### Step 4: Create Service

1. **Navigate to Services**
   - Go to Project Setup → Services → "New Service"

2. **Configure Service**
   - Name: "Joe Lang Website Service"
   - Identifier: `joe_lang_website_service`
   - Service Type: "Serverless AWS Lambda" (for static files)

3. **Add Manifest**
   - Manifest Type: "Serverless AWS Lambda"
   - Store Type: "Git"
   - Configure Git connector (if not already set up)
   - Paths: `*.html`, `*.css`, `*.pdf`

### Step 5: Create Environment

1. **Navigate to Environments**
   - Go to Project Setup → Environments → "New Environment"

2. **Configure Environment**
   - Name: "Production"
   - Identifier: `production`
   - Environment Type: "Production"

3. **Add Variables**
   - `aws_region`: `us-west-2`
   - `environment_type`: `production`

### Step 6: Create Infrastructure

1. **Add Infrastructure Definition**
   - In the Production environment
   - Click "New Infrastructure"
   - Name: "AWS S3 Infrastructure"
   - Identifier: `aws_s3_infrastructure`
   - Deployment Type: "Serverless AWS Lambda"

2. **Configure Infrastructure**
   - Connector: Select your AWS S3 Connector
   - Region: `us-west-2`

### Step 7: Import Pipeline

1. **Create New Pipeline**
   - Go to Pipelines → "New Pipeline"
   - Name: "Deploy Joe Lang Website"
   - Setup: "Inline"

2. **Import Pipeline YAML**
   - Switch to YAML view
   - Copy content from `.harness/pipelines/deploy-website.yaml`
   - Paste into the pipeline editor
   - Click "Save"

### Step 8: Configure Pipeline Variables

1. **Set Pipeline Variables**
   - `bucket_name`: Choose a unique S3 bucket name (e.g., `joe-lang-portfolio-2025`)
   - `aws_region`: `us-west-2`

2. **Configure Input Sets** (Optional)
   - Create input sets for different environments
   - Allows for easy switching between staging/production

### Step 9: Run Initial Deployment

1. **Execute Pipeline**
   - Click "Run Pipeline"
   - Provide bucket name when prompted
   - Monitor execution progress

2. **Verify Deployment**
   - Check pipeline logs for any errors
   - Note the S3 website URL in the final step output
   - Test website accessibility

## Post-Deployment Configuration

### Enable Public Access

If the website isn't publicly accessible, configure bucket policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

### Custom Domain (Optional)

1. **Register Domain** (if needed)
2. **Configure Route 53** (or your DNS provider)
3. **Set up CloudFront** for HTTPS and better performance
4. **Update bucket policy** for custom domain

## Troubleshooting

### Common Issues

1. **Permission Denied Errors**
   - Verify AWS credentials are correct
   - Check IAM permissions include all required S3 actions
   - Ensure bucket name is unique globally

2. **Pipeline Execution Failures**
   - Check Harness delegate connectivity
   - Verify AWS connector configuration
   - Review pipeline logs for specific error messages

3. **Website Not Accessible**
   - Confirm S3 website hosting is enabled
   - Check bucket policy allows public read access
   - Verify index.html exists in bucket root

4. **File Upload Issues**
   - Ensure all required files are in repository
   - Check Git connector configuration
   - Verify file paths in service manifest

### Getting Help

- **Harness Documentation**: [docs.harness.io](https://docs.harness.io)
- **AWS S3 Documentation**: [docs.aws.amazon.com/s3](https://docs.aws.amazon.com/s3/)
- **Support**: Contact Joe Lang at josephlang@gmail.com

## Maintenance

### Regular Updates

1. **Content Updates**
   - Modify HTML files as needed
   - Update PDF resume
   - Commit changes to trigger automatic deployment

2. **Pipeline Maintenance**
   - Review and update AWS permissions periodically
   - Monitor deployment logs for issues
   - Update Harness connectors as needed

3. **Security Best Practices**
   - Rotate AWS credentials regularly
   - Review S3 bucket policies
   - Monitor access logs

### Backup Strategy

- **Source Code**: Maintained in Git repository
- **Deployment Configuration**: Stored in `.harness/` directory
- **Website Content**: Automatically backed up in S3 versioning (if enabled)

## Cost Optimization

- **S3 Storage**: Minimal cost for static files
- **Data Transfer**: Monitor bandwidth usage
- **Harness Usage**: Track pipeline execution minutes
- **AWS Resources**: Clean up unused buckets and resources

---

This deployment guide ensures a smooth setup and ongoing maintenance of Joe Lang's professional portfolio website using Harness.io and AWS S3.

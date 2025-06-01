# Joe Lang - Professional Portfolio Website

A comprehensive professional job-seeking website showcasing Joe Lang's career highlights, experience, and achievements in enterprise sales and GTM strategy.

## Website Structure

- **Home** (`index.html`) - Hero section with key stats and highlights
- **About** (`about.html`) - Detailed professional summary and what Joe is looking for
- **Experience** (`experience.html`) - Career achievements and company history
- **Portfolio** (`portfolio.html`) - Case studies, testimonials, and PDF download
- **Contact** (`contact.html`) - Contact information and availability

## Features

- Responsive design optimized for desktop and mobile
- Professional blue and white color scheme
- Downloadable PDF resume
- Working links to Harness.io case studies
- Clean navigation between sections
- SEO-friendly structure with proper meta tags

## Deployment Pipeline

This project includes a complete Harness.io deployment pipeline for AWS S3 static website hosting.

### Pipeline Components

- **Pipeline**: `.harness/pipelines/deploy-website.yaml`
- **AWS Connector**: `.harness/connectors/aws-connector.yaml`
- **Service Definition**: `.harness/services/website-service.yaml`
- **Environment**: `.harness/environments/production.yaml`
- **Project Config**: `harness-config.yaml`

### Prerequisites

1. **Harness Account**: Active Harness.io account with CD module enabled
2. **AWS Account**: AWS account with S3 access
3. **AWS Credentials**: Access Key ID and Secret Access Key with S3 permissions

### Required AWS Permissions

The AWS credentials need the following S3 permissions:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:GetBucketLocation",
                "s3:GetBucketWebsite",
                "s3:PutBucketWebsite",
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::joe-lang-portfolio-*",
                "arn:aws:s3:::joe-lang-portfolio-*/*"
            ]
        }
    ]
}
```

## Setup Instructions

### 1. Configure Harness Secrets

In your Harness account, create the following secrets:

- `aws_access_key_id` - Your AWS Access Key ID
- `aws_secret_access_key` - Your AWS Secret Access Key

### 2. Create S3 Bucket

Create an S3 bucket for hosting (or let the pipeline create it):
```bash
aws s3 mb s3://joe-lang-portfolio-unique-suffix --region us-west-2
```

### 3. Import Pipeline

1. In Harness, create a new project called "Joe Lang Portfolio"
2. Import the pipeline from `.harness/pipelines/deploy-website.yaml`
3. Configure the AWS connector with your credentials
4. Set up the service and environment definitions

### 4. Run Deployment

1. Trigger the pipeline in Harness
2. Provide the S3 bucket name when prompted
3. Monitor the deployment progress
4. Access your website at the S3 website URL

## Local Development

To test the website locally:

```bash
# Serve files locally (requires Python)
python3 -m http.server 8000

# Or use Node.js
npx serve .

# Or use any other static file server
```

Visit `http://localhost:8000` to view the website.

## File Structure

```
.
├── index.html              # Homepage
├── about.html              # About page
├── experience.html         # Experience page
├── portfolio.html          # Portfolio page
├── contact.html            # Contact page
├── styles.css              # Main stylesheet
├── Lang - Career Highlights.pdf  # Resume PDF
├── .harness/               # Harness pipeline configuration
│   ├── pipelines/
│   ├── connectors/
│   ├── services/
│   └── environments/
├── harness-config.yaml     # Project configuration
├── README.md               # This file
└── deployment-guide.md     # Detailed deployment guide
```

## Customization

### Styling
- Modify `styles.css` to change colors, fonts, or layout
- The design uses CSS Grid and Flexbox for responsive layouts
- Color scheme is based on `#007bff` (blue) primary color

### Content
- Update HTML files to modify content
- Replace `Lang - Career Highlights.pdf` with updated resume
- Update case study links in `portfolio.html`

### Pipeline
- Modify `.harness/pipelines/deploy-website.yaml` for different deployment targets
- Update AWS region in pipeline variables
- Add additional deployment steps as needed

## Support

For questions about the website or deployment pipeline, contact:
- Email: josephlang@gmail.com
- LinkedIn: [linkedin.com/in/joemlang](https://linkedin.com/in/joemlang)

## License

© 2025 Joe Lang. All rights reserved.

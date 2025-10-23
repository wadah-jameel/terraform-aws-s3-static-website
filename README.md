# Terraform AWS S3 Static Website

Deploy a static website to AWS S3 using Terraform with optional CloudFront CDN.

## 🏗️ Architecture

```
┌─────────────┐      ┌─────────────┐      ┌──────────────┐
│   Browser   │─────▶│ CloudFront  │─────▶│  S3 Bucket   │
│             │      │ (Optional)  │      │  (Website)   │
└─────────────┘      └─────────────┘      └──────────────┘
```

## ✨ Features

- S3 bucket configured for static website hosting
- Public read access for website content
- Custom error page (404.html)
- Optional CloudFront CDN for global distribution
- HTTPS support with CloudFront
- Fully automated deployment with Terraform

## 📋 Prerequisites

- AWS Account
- [Terraform](https://www.terraform.io/downloads.html) installed (>= 1.0)
- [AWS CLI](https://aws.amazon.com/cli/) configured with credentials
- Basic understanding of AWS S3 and Terraform

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd terraform-aws-s3-static-website
```

### 2. Configure Variables

Copy the example variables file and update with your values:

```bash
cp terraform.tfvars.example terraform.tfvars
```

Edit `terraform.tfvars`:

```hcl
bucket_name = "my-unique-website-bucket-12345"  # Must be globally unique!
aws_region  = "us-east-1"
environment = "dev"
enable_cloudfront = false  # Set to true for CDN
```

### 3. Initialize Terraform

```bash
terraform init
```

### 4. Preview Changes

```bash
terraform plan
```

### 5. Deploy Infrastructure

```bash
terraform apply
```

Type `yes` when prompted to confirm.

### 6. Upload Website Files

After deployment, upload your website files to the S3 bucket:

```bash
# Upload index.html
aws s3 cp website/index.html s3://your-bucket-name/index.html

# Upload error.html
aws s3 cp website/error.html s3://your-bucket-name/error.html

# Or upload entire website folder
aws s3 sync website/ s3://your-bucket-name/
```

### 7. Access Your Website

Terraform will output the website URL:

```bash
terraform output website_url
```

Visit the URL in your browser!

## 📁 Project Structure

```
.
├── main.tf                    # Main Terraform configuration
├── variables.tf               # Input variables
├── outputs.tf                 # Output values
├── terraform.tfvars.example   # Example variables file
├── .gitignore                # Git ignore rules
├── README.md                 # This file
└── website/                  # Website files
    ├── index.html           # Homepage
    └── error.html           # 404 error page
```

## 🔧 Configuration

### Variables

| Variable | Description | Type | Default | Required |
|----------|-------------|------|---------|----------|
| `aws_region` | AWS region | string | us-east-1 | No |
| `bucket_name` | S3 bucket name (globally unique) | string | - | Yes |
| `environment` | Environment name | string | dev | No |
| `enable_cloudfront` | Enable CloudFront CDN | bool | false | No |

### Outputs

| Output | Description |
|--------|-------------|
| `bucket_name` | Name of the S3 bucket |
| `website_endpoint` | S3 website endpoint |
| `website_url` | Full HTTP URL to website |
| `cloudfront_domain_name` | CloudFront domain (if enabled) |
| `cloudfront_url` | Full HTTPS CloudFront URL (if enabled) |

## 💰 Cost Estimate

### Without CloudFront
- **S3 Storage**: ~$0.023 per GB/month
- **S3 Requests**: ~$0.0004 per 1,000 GET requests
- **Data Transfer**: First 100 GB free, then ~$0.09/GB

**Estimated**: $0.50 - $2/month for a small website

### With CloudFront
- **CloudFront Data Transfer**: ~$0.085 per GB
- **CloudFront Requests**: ~$0.01 per 10,000 requests
- Plus S3 costs (reduced due to caching)

**Estimated**: $1 - $5/month for a small website

## 🧹 Cleanup

To destroy all resources and avoid charges:

```bash
# First, empty the S3 bucket
aws s3 rm s3://your-bucket-name --recursive

# Then destroy infrastructure
terraform destroy
```

Type `yes` when prompted.

## 📚 What You'll Learn

- Creating and configuring S3 buckets with Terraform
- Setting up static website hosting on S3
- Managing S3 bucket policies and public access
- Using CloudFront as a CDN (optional)
- Terraform outputs and variables
- AWS best practices for static websites

## 🔐 Security Notes

- This configuration makes the S3 bucket **publicly readable** for website hosting
- Never store sensitive data in a public website bucket
- For production, consider:
  - Enabling CloudFront with custom domain
  - Using AWS WAF for protection
  - Enabling S3 access logging
  - Implementing proper IAM policies

## 🐛 Troubleshooting

### Bucket name already exists
S3 bucket names must be globally unique. If you get an error, try a different bucket name.

### 403 Forbidden error
Make sure the bucket policy is applied and public access block is disabled.

### Website not loading after upload
Wait a few minutes for S3/CloudFront to propagate. Clear your browser cache.

### CloudFront takes long to deploy
CloudFront distributions can take 15-20 minutes to deploy. Be patient!

## 📖 Additional Resources

- [Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS S3 Static Website Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [AWS CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/)

## 📝 License

MIT License - Feel free to use this for learning and personal projects!

## 🤝 Contributing

Issues and pull requests are welcome! This is a learning project.

---

**Happy Learning! 🎉**

If you found this helpful, please give it a ⭐ on GitHub!

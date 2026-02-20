Name: Karthik Prudhivi
G#: G01543977

# SWE 645 – HW1 Deployment (S3 + EC2)

## Project Overview
This project contains a class homepage (index.html) linked to a Student Survey form (survey.html).  
The website is deployed in two ways:
1) Using Amazon S3 Static Website Hosting.
2) Using a web server (Apache) on an Amazon EC2 instance.

---

## URLs

### 1) S3 Static Website URL
- http://hw1-swe645-kprudhivi.s3-website-us-east-1.amazonaws.com

### 2) EC2 Website URL 
- http://44.220.135.246

## Project Files
- index.html — Homepage (Part 1)
- survey.html — Student Survey Form (Part 2)
- styles.css — Styling for the pages
- profile.jpeg — Profile image used on homepage
- error.html — Optional error page for S3/EC2

---

# Part A: Deploy on Amazon S3 (Static Website Hosting)

## Steps to Create and Configure S3 Bucket
1. Create S3 Bucket
   - Go to AWS Console → S3 → Create bucket
   - Provide a globally unique bucket name (example: `hw1-swe645-kprudhivi`)
   - Choose a region (example: `us-east-1`)
   - Create bucket

2. Upload Website Files
   - Open the bucket → Upload
   - Upload:
     - index.html, survey.html, styles.css, profile.jpeg, and error.html
   - Ensure files are uploaded to the bucket root (not inside folders)

3. Enable Static Website Hosting
   - Go to bucket → Properties → Static website hosting → Edit
   - Enable Static website hosting
   - Index document: index.html
   - Error document: error.html
   - Save changes

4. Allow Public Access
   - Bucket → Permissions → Block public access → Edit
   - Uncheck “Block all public access” (or disable the block settings for this bucket)
   - Save changes

5. Add Bucket Policy to Allow Public Read
   - Bucket → Permissions → Bucket policy → Edit
   - Paste and save:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::hw1-swe645-kprudhivi/*"
        }
    ]
}

6. Verify Deployment
Use the Website endpoint shown under:
Properties → Static website hosting → Website endpoint
Confirm:
Homepage loads
Survey link opens survey.html

---------------------

# Part B: Deploy on Amazon EC2 (Apache Web Server)
Steps to Create and Configure EC2 Instance:

1. Launch an EC2 Instance
  AWS Console → EC2 → Launch instance
  AMI: Amazon Linux 2023 (or Amazon Linux 2)
  Instance type: t3.micro (Free Tier)
  Create/select a key pair (download .pem)
  Network: enable Auto-assign public IP

2. Configure Security Group (Inbound Rules)
  Allow SSH:
   Type: SSH, Port: 22, Source: My IP
  Allow HTTP:
   Type: HTTP, Port: 80, Source: Anywhere (0.0.0.0/0)

3. Connect to the EC2 Instance
  From local terminal (Mac/Linux/Git Bash):
	chmod 400 hw1-645.pem
	ssh -i hw1-645.pem ec2-user@44.220.135.246

4. Install and Start Apache
  sudo yum update -y
  sudo yum install httpd -y
  sudo systemctl start httpd
  sudo systemctl enable httpd

5. Upload Website Files to EC2
  Upload using SCP from local machine:
	scp -i hw1-swe645.pem index.html survey.html styles.css error.html profile.jpeg ec2-user@44.220.135.246:/tmp/

6. Verify Deployment
Open in browser:
http://44.220.135.246/
Confirm:
Homepage loads
Survey link opens survey.html




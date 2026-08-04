---
title: "AWS CLI Profile Configuration"
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

To deploy AWS infrastructure from your CLI or deployment scripts, you must configure an IAM profile with appropriate credentials.

### Step 1: IAM User Credentials

Ensure you have an IAM User created in the AWS Management Console with administrator or deployment permissions. Obtain the Access Key ID and Secret Access Key under the user's Security credentials tab.

### Step 2: Configure Named Profile

Run `aws configure` with the `--profile` flag to store credentials securely without altering your default AWS profile:

```bash
aws configure --profile smarthealthcare
```

Enter the required credentials when prompted:

```plaintext
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

### Step 3: Verify AWS Profile Access

Confirm that your CLI can communicate with your AWS account using the newly created profile:

```bash
aws sts get-caller-identity --profile smarthealthcare
```

A successful output will return your UserId, Account, and Arn.
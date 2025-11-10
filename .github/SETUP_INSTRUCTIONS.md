# GitHub Actions Setup Instructions

## Step-by-Step Guide to Configure Secrets

### 1. Go to Your Repository Settings
```
https://github.com/YOUR_USERNAME/pomodoro-app-js/settings/secrets/actions
```

### 2. Add Each Secret (Click "New repository secret" for each)

#### Secret 1: SONARQUBE_URL
- **Name**: `SONARQUBE_URL`
- **Value**: `http://100.25.217.210:9000`
- Click "Add secret"

#### Secret 2: SONAR_TOKEN
- **Name**: `SONAR_TOKEN`
- **Value**: Get from SonarQube
  1. Go to: http://100.25.217.210:9000
  2. Login (default: admin/admin)
  3. Click your profile → My Account → Security
  4. Generate Token → Name it "github-actions" → Generate
  5. Copy the token and paste as secret value

#### Secret 3: NEXUS_USERNAME
- **Name**: `NEXUS_USERNAME`
- **Value**: `admin` (or your Nexus username)

#### Secret 4: NEXUS_PASSWORD
- **Name**: `NEXUS_PASSWORD`
- **Value**: Your Nexus password

#### Secret 5: NGINX_SSH_USER
- **Name**: `NGINX_SSH_USER`
- **Value**: `ubuntu` (or `ec2-user` depending on your AMI)

#### Secret 6: NGINX_SSH_KEY
- **Name**: `NGINX_SSH_KEY`
- **Value**: Your EC2 private key content
  - Open your .pem file
  - Copy entire content including:
    ```
    -----BEGIN RSA PRIVATE KEY-----
    [your key content]
    -----END RSA PRIVATE KEY-----
    ```
  - Paste as secret value

### 3. Verify All Secrets Are Added

You should have 6 secrets configured:
- ✅ SONARQUBE_URL
- ✅ SONAR_TOKEN
- ✅ NEXUS_USERNAME
- ✅ NEXUS_PASSWORD
- ✅ NGINX_SSH_USER
- ✅ NGINX_SSH_KEY

### 4. Trigger the Workflow

After adding all secrets:
1. Go to "Actions" tab in your repository
2. Click "Pomodoro App CI/CD" workflow
3. Click "Run workflow" → "Run workflow"

OR simply push a commit to the main branch:
```bash
git add .
git commit -m "Configure GitHub Actions"
git push origin main
```

## Troubleshooting

### If SonarQube analysis fails:
- Check SONARQUBE_URL is `http://100.25.217.210:9000` (with http://)
- Verify SONAR_TOKEN is valid
- Ensure SonarQube server is running and accessible

### If Nexus upload fails:
- Verify NEXUS_USERNAME and NEXUS_PASSWORD are correct
- Check Nexus is accessible at http://3.85.143.154:8081
- Ensure the `raw-releases` repository exists in Nexus

### If Nginx deployment fails:
- Check NGINX_SSH_USER is correct (usually `ubuntu` or `ec2-user`)
- Verify NGINX_SSH_KEY is the complete private key content
- Ensure SSH access is allowed in security group (port 22)
- Verify Nginx server is running at 3.88.108.148

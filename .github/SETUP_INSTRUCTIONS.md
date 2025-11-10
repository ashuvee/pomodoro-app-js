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
  
  **IMPORTANT - How to copy the key correctly:**
  
  Option A - Using terminal (Linux/Mac):
  ```bash
  cat your-key.pem
  ```
  Copy the ENTIRE output including headers
  
  Option B - Using text editor:
  1. Open your .pem file in a text editor (NOT Word/Notepad++)
  2. Copy EVERYTHING from first line to last line
  3. Must include these lines:
     ```
     -----BEGIN RSA PRIVATE KEY-----
     [multiple lines of key content]
     -----END RSA PRIVATE KEY-----
     ```
     OR
     ```
     -----BEGIN OPENSSH PRIVATE KEY-----
     [multiple lines of key content]
     -----END OPENSSH PRIVATE KEY-----
     ```
  
  **Common Mistakes to Avoid:**
  - ❌ Don't add extra spaces or newlines at start/end
  - ❌ Don't copy only part of the key
  - ❌ Don't use Windows Notepad (it may change line endings)
  - ✅ Use a proper text editor or terminal cat command

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

**Error: "ssh: no key found" or "handshake failed":**
- The SSH key format is incorrect
- Solution:
  1. Delete the current NGINX_SSH_KEY secret
  2. Re-add it using `cat your-key.pem` command
  3. Make sure you copy the ENTIRE key with headers/footers
  4. Ensure no extra spaces at beginning or end

**Error: "Permission denied":**
- Check NGINX_SSH_USER is correct (usually `ubuntu` for Ubuntu, `ec2-user` for Amazon Linux)
- Verify the SSH key matches the server
- Ensure SSH access is allowed in security group (port 22 open)

**Error: "Connection timeout":**
- Verify Nginx server is running at 3.88.108.148
- Check security group allows inbound SSH (port 22) from GitHub IPs
- Ensure the server is in running state (not stopped)

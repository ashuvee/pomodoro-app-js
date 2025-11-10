# GitHub Actions Secrets Configuration

To run the CI/CD pipeline, you need to configure the following secrets in your GitHub repository:

## Repository Secrets
Go to: **Settings → Secrets and variables → Actions → New repository secret**

### Required Secrets:

1. **SONARQUBE_URL**
   - Description: Your SonarQube server URL
   - Value: `http://100.25.217.210:9000`

2. **SONAR_TOKEN**
   - Description: SonarQube authentication token
   - How to get: SonarQube → My Account → Security → Generate Token

3. **NEXUS_USERNAME**
   - Description: Nexus repository username
   - Example: `admin`

4. **NEXUS_PASSWORD**
   - Description: Nexus repository password
   - Example: Your Nexus admin password

5. **NGINX_SSH_USER**
   - Description: SSH username for Nginx server
   - Example: `ubuntu` or `ec2-user`

6. **NGINX_SSH_KEY**
   - Description: Private SSH key for accessing Nginx server
   - How to get: Your EC2 private key (.pem file content)
   - Note: Copy the entire content including `-----BEGIN RSA PRIVATE KEY-----` and `-----END RSA PRIVATE KEY-----`

## How to Add Secrets:

```bash
# In GitHub UI:
1. Go to your repository
2. Click "Settings" tab
3. Click "Secrets and variables" → "Actions"
4. Click "New repository secret"
5. Enter the name and value
6. Click "Add secret"
```

## Verify Configuration:

After adding all secrets, trigger the workflow by:
- Pushing to main branch, or
- Going to Actions → Pomodoro App CI/CD → Run workflow

## Server Configuration:

The workflow uses these configured values:
- **SonarQube URL**: `http://100.25.217.210:9000`
- **Nexus URL**: `http://3.85.143.154:8081`
- **Nginx Server**: `http://3.88.108.148:80`
- **Nexus Repository**: `raw-releases`
- **Nginx Web Root**: `/var/www/html`

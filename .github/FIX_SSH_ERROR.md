# Quick Fix: SSH Authentication Error

## The Error You're Seeing:
```
ssh.ParsePrivateKey: ssh: no key found
ssh: handshake failed: ssh: unable to authenticate
```

## Root Cause:
The `NGINX_SSH_KEY` secret is not properly formatted or is missing content.

## How to Fix (5 minutes):

### Step 1: Get your SSH key content
On your computer where you have the .pem file:

```bash
# Display your key
cat your-nginx-key.pem

# Or copy directly to clipboard (Mac)
cat your-nginx-key.pem | pbcopy

# Or copy directly to clipboard (Linux with xclip)
cat your-nginx-key.pem | xclip -selection clipboard
```

### Step 2: Verify the key looks correct
The output should look like this:
```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA...
[many lines of random characters]
...ending with more characters
-----END RSA PRIVATE KEY-----
```

OR like this:
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEA...
[many lines of random characters]
...ending with more characters
-----END OPENSSH PRIVATE KEY-----
```

### Step 3: Update GitHub Secret

1. Go to: `https://github.com/YOUR_USERNAME/pomodoro-app-js/settings/secrets/actions`

2. Find `NGINX_SSH_KEY` in the list

3. Click the **pencil icon** (Update) or **trash icon** (Delete) then re-add it

4. Paste the ENTIRE key content (from Step 1)
   - Must include `-----BEGIN...-----` line
   - Must include `-----END...-----` line
   - Must include ALL lines in between
   - No extra spaces before or after

5. Click **Update secret** or **Add secret**

### Step 4: Re-run the workflow

1. Go to: **Actions** tab in your repository
2. Click the failed workflow run
3. Click **Re-run all jobs**

## Still Not Working?

### Check your SSH user is correct:
```bash
# Test SSH connection manually
ssh -i your-nginx-key.pem ubuntu@3.88.108.148

# If "ubuntu" doesn't work, try:
ssh -i your-nginx-key.pem ec2-user@3.88.108.148
```

The username that works is what you should use for `NGINX_SSH_USER` secret.

### Verify the key matches your server:
```bash
# Check key fingerprint
ssh-keygen -lf your-nginx-key.pem

# Connect to server and check authorized keys
ssh -i your-nginx-key.pem ubuntu@3.88.108.148 "cat ~/.ssh/authorized_keys"
```

### Check GitHub can reach your server:
- AWS Security Group must allow inbound SSH (port 22)
- You can allow GitHub's IP ranges or allow from anywhere (0.0.0.0/0) for port 22
- Verify the EC2 instance is running (not stopped)

## Alternative: Use Password Authentication

If SSH keys keep failing, you can modify the workflow to use password authentication:

```yaml
- name: 🚀 Deploy to Nginx Server
  uses: appleboy/ssh-action@v1.0.0
  with:
    host: ${{ env.NGINX_SERVER }}
    username: ${{ secrets.NGINX_SSH_USER }}
    password: ${{ secrets.NGINX_SSH_PASSWORD }}  # Add this secret
    # Remove: key: ${{ secrets.NGINX_SSH_KEY }}
```

But SSH keys are more secure and recommended.

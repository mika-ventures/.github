# GitHub Network Access Setup Guide

This document explains how we configured Claude to work with GitHub for the mika-ventures organization.

## Overview

To enable Claude to interact with GitHub repositories (clone, commit, push), we needed to configure network access in Claude's settings and generate a GitHub Personal Access Token.

## Step-by-Step Configuration

### 1. Enable Network Access in Claude

**Location:** Claude Settings → Capabilities → Code execution and file creation

**Steps:**
1. Open Claude settings (click your profile icon)
2. Navigate to **Capabilities**
3. Find **"Code execution and file creation"** section
4. Toggle **"Allow network egress"** to **ON**
5. Under **"Additional allowed domains"**, add:
   - `github.com`
6. Save settings

**Why this is needed:**
- By default, Claude's code execution environment is isolated from the internet
- Enabling network egress allows Claude to make outbound connections
- Adding `github.com` to allowed domains permits Git operations (clone, push, pull)

### 2. Generate GitHub Personal Access Token

**Location:** GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)

**Steps:**
1. Go to https://github.com/settings/tokens
2. Click **"Generate new token"** → **"Generate new token (classic)"**
3. Configure the token:
   - **Note:** `mika-ventures-claude-access`
   - **Expiration:** Choose based on your security preferences (we used 90 days)
   - **Scopes:** Select the following:
     - ✅ `repo` (Full control of private repositories)
     - ✅ `workflow` (Update GitHub Action workflows)
     - ✅ `read:org` (Read org and team membership, read org projects)
4. Click **"Generate token"**
5. **IMPORTANT:** Copy the token immediately and store it securely
   - Token format: `ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`
   - You won't be able to see it again!

**Our Token:** `ghp_****************************` (stored securely, not in version control)

### 3. Configure Git Credentials

**For Claude to use:**
```bash
git config --global user.name "Miguel Porras"
git config --global user.email "miguelporraswork@gmail.com"
```

**When cloning/pushing:**
```bash
# Format: https://TOKEN@github.com/org/repo.git
git clone https://YOUR_TOKEN_HERE@github.com/mika-ventures/.github.git
```

## Verification

To verify the setup works:

```bash
# Test clone
git clone https://YOUR_TOKEN_HERE@github.com/mika-ventures/.github.git

# Test push
cd .github
echo "test" > test.txt
git add test.txt
git commit -m "Test commit"
git push origin main
```

## Security Best Practices

1. **Token Expiration:** Set tokens to expire regularly (30-90 days recommended)
2. **Minimal Scopes:** Only grant necessary permissions
3. **Token Storage:** Never commit tokens to repositories
4. **Rotate Regularly:** Generate new tokens before old ones expire
5. **Revoke Unused:** Remove tokens that are no longer needed

## Troubleshooting

### "CONNECT tunnel failed, response 403"
- **Cause:** Network egress not enabled or domain not allowed
- **Fix:** Verify `github.com` is in "Additional allowed domains"

### "Authentication failed"
- **Cause:** Token is invalid, expired, or has insufficient scopes
- **Fix:** Generate a new token with correct scopes

### "Permission denied"
- **Cause:** Token doesn't have access to the organization
- **Fix:** Ensure you have admin access to mika-ventures org

## What We Accomplished

✅ **Step 1:** Enabled network access in Claude settings  
✅ **Step 2:** Generated GitHub Personal Access Token with proper scopes  
✅ **Step 3:** Configured Git with Miguel's credentials  
✅ **Step 4:** Successfully cloned mika-ventures/.github repository  
✅ **Step 5:** Pushed organization profile README  

## Next Steps

- Create claude-workspace repository for coding standards
- Set up repository templates
- Document coding guidelines in CLAUDE.md
- Create project structure templates

---

**Last Updated:** February 16, 2026  
**Configured By:** Miguel Porras  
**Organization:** mika-ventures

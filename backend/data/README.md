# Data Configuration

This directory contains the personality and context files for your AI Digital Twin.

## Required Files

Create the following files based on the provided examples:

### 1. `facts.json` (Required)
Structured personal information in JSON format.

```bash
cp facts.json.example facts.json
# Edit with your personal information
```

### 2. `linkedin.pdf` (Required)
Export your LinkedIn profile as a PDF and place it here.

**How to get your LinkedIn PDF:**
1. Go to your LinkedIn profile
2. Click "More" → "Save to PDF"
3. Save as `linkedin.pdf` in this directory

### 3. `style.txt` (Required)
Your communication style and mannerisms.

```bash
cp style.txt.example style.txt
# Describe your communication style
```

### 4. `summary.txt` (Required)
Additional context, career highlights, and personal information.

```bash
cp summary.txt.example summary.txt
# Add detailed information about yourself
```

## Security Note

⚠️ **These files contain personal information and are excluded from Git.**

Make sure to:
- Never commit these files to version control
- Keep backups in a secure location
- Update them as your information changes

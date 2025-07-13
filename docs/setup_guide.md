# Setup Guide - Vendor Email Automation

## Prerequisites

Before setting up this automation, ensure you have:

- **n8n instance** (self-hosted or cloud)
- **Google Workspace account** with access to:
  - Gmail
  - Google Sheets
  - Google Drive
- **Basic understanding** of n8n workflows
- **Administrative access** to configure OAuth credentials

## Step 1: Prepare Your Google Sheets

### Sheet 1: Vendor List (Main Data)
Create a Google Sheet with the following columns:
- `First Name` - Contact's first name
- `Last Name` - Contact's last name  
- `Company` - Company name
- `Email` - Contact email address
- `Send Status` - Track email status (initially blank)
- `Time` - Timestamp of when email was sent

### Sheet 2: Email Templates
Create a second sheet in the same workbook with:
- `Subject` - Email subject line
- `Body` - Email body with placeholders `[First Name]` and `[Company]`

**Example template:**
```
Subject: Partnership Opportunity with [Company]
Body: Hi [First Name],

I hope this message finds you well. I've been following [Company]'s work and am impressed by your recent achievements.

I'd love to explore potential collaboration opportunities that could benefit both our organizations.

Would you be open to a brief conversation this week?

Best regards,
[Your Name]
```

## Step 2: Configure Google API Credentials

### Gmail OAuth2 Setup
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable Gmail API
4. Create OAuth2 credentials:
   - Application type: Web application
   - Authorized redirect URIs: Add your n8n webhook URL
5. Download the credentials JSON file
6. In n8n, add new Gmail credentials using the OAuth2 flow

### Google Sheets OAuth2 Setup
1. In the same Google Cloud project, enable Google Sheets API
2. Use the same OAuth2 credentials or create new ones
3. In n8n, add new Google Sheets credentials

### Google Drive OAuth2 Setup (Optional)
1. Enable Google Drive API in Google Cloud Console
2. Configure OAuth2 credentials similar to above
3. Add Google Drive credentials in n8n

## Step 3: Import and Configure the Workflow

### Import the Workflow
1. Download the `workflow.json` file from this repository
2. In n8n, go to **Workflows** → **Import from file**
3. Select the downloaded JSON file
4. Click **Import**

### Configure Node Credentials
Update the following nodes with your credentials:

**Get row(s) in sheet:**
- Credential: Select your Google Sheets account
- Document ID: Your Google Sheets document ID
- Sheet: Select "Sheet1" (or your main data sheet)

**Get row(s) in sheet1:**
- Credential: Select your Google Sheets account  
- Document ID: Same as above
- Sheet: Select "Sheet2" (or your templates sheet)

**Send a message:**
- Credential: Select your Gmail account
- Update sender name to your name

**Append or update row in sheet:**
- Credential: Select your Google Sheets account
- Document ID: Same Google Sheets document
- Sheet: Select "Sheet1"

## Step 4: Customize the Workflow

### Update Personal Information
**Send a message node:**
- Change `senderName` from "Nithin Sankar" to your name
- Adjust any other sender details as needed

### Modify Template Logic (Optional)
In the **Code** node, you can customize:
- Template selection logic
- HTML formatting
- Error handling for missing fields

### Adjust Timing
**Wait** node:
- Current: 1 minute delay between emails
- Recommended: 2-5 minutes for better deliverability
- For high volume: Consider longer delays

## Step 5: Test the Workflow

### Test Setup
1. Add 1-2 test contacts to your Google Sheet
2. Create 1-2 test email templates
3. Set up a test Gmail account (recommended)

### Run Test
1. Click **Execute workflow** on the manual trigger
2. Monitor each node execution
3. Check that:
   - Data is pulled from sheets correctly
   - Templates are selected randomly
   - Emails are personalized properly
   - Status is updated in the sheet

### Verify Results
- Check sent emails in Gmail
- Confirm Google Sheet status updates
- Verify timestamp formatting

## Step 6: Production Deployment

### Data Validation
- Ensure all email addresses are valid
- Check for duplicate contacts
- Verify company names are formatted consistently

### Rate Limiting
- Gmail has sending limits (500 emails/day for free accounts)
- Adjust wait times based on your volume
- Consider spreading campaigns across multiple days

### Monitoring
- Set up error notifications
- Monitor execution logs
- Track delivery rates and responses

## Troubleshooting

### Common Issues

**Authentication Errors:**
- Re-authenticate OAuth2 credentials
- Check API quotas in Google Cloud Console
- Verify redirect URIs are correct

**Template Not Found:**
- Ensure Sheet2 exists and has data
- Check column headers match exactly
- Verify sheet permissions

**Email Not Sending:**
- Check Gmail sending limits
- Verify email format in templates
- Test with a simple template first

**Status Not Updating:**
- Check Google Sheets write permissions
- Verify column mapping in append node
- Test with manual status updates

### Performance Optimization

**For Large Lists:**
- Increase batch processing size
- Implement more sophisticated error handling
- Add retry logic for failed sends

**For Better Deliverability:**
- Randomize send times
- Use multiple email templates
- Implement progressive delays

## Security Considerations

- **Never commit credentials** to version control
- **Use environment variables** for sensitive data
- **Regularly rotate** OAuth2 tokens
- **Monitor for suspicious activity**
- **Follow email compliance** regulations (CAN-SPAM, GDPR)

## Next Steps

Once your automation is running successfully:
1. Monitor performance metrics
2. A/B test different email templates
3. Implement response tracking
4. Scale to larger contact lists
5. Add advanced personalization features

## Support

For issues with this setup:
1. Check n8n community forums
2. Review Google API documentation
3. Test individual nodes in isolation
4. Enable debug logging for troubleshooting

---

**⚠️ Important:** Always test thoroughly with small batches before running large campaigns. Respect recipients' privacy and follow email marketing best practices.
# Setup Guide

## Prerequisites

Before setting up this Order-to-Cash AI Automation Agent, make sure you have:

1. **n8n Account** - Sign up at [n8n.io](https://n8n.io) (Free tier available)
2. **OpenAI API Key** - Get your API key from [OpenAI Platform](https://platform.openai.com/api-keys)
3. **Google Account** - For Google Sheets integration
4. **Google Sheets Setup** - Create a new Google Sheet with the following tabs:
   - `Invoices` - Customer invoice data
   - `Payments` - Payment records
   - `Matches` - Payment-to-invoice matches
   - `Disputes` - Customer dispute records

---

## Step 1: Set Up Google Sheets

### Create Your Spreadsheet

1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new spreadsheet named **"OTC-Automation-Data"**
3. Create four tabs with the exact names: `Invoices`, `Payments`, `Matches`, `Disputes`

### Configure Each Tab

**Invoices Tab Headers:**
```
Invoice_ID | Customer_Name | Amount | Due_Date | Status | Description
```

**Payments Tab Headers:**
```
Payment_ID | Amount | Payment_Date | Reference | Customer_Name
```

**Matches Tab Headers:**
```
Match_ID | Invoice_ID | Payment_ID | Matched_Amount | Confidence_Score | Status | Match_Date
```

**Disputes Tab Headers:**
```
Dispute_ID | Customer_Name | Issue_Type | Priority | Description | Invoice_ID
```

---

## Step 2: Configure n8n

### Import the Workflow

1. Log in to your n8n account
2. Click on **Workflows** → **+ Create New Workflow**
3. Click the **three-dot menu (⋯)** → **Import from File**
4. Upload the `order-to-cash-agent.json` file from this repository

### Set Up Credentials

#### OpenAI Credential

1. In n8n, go to **Credentials** → **+ Add Credential**
2. Search for **OpenAI**
3. Enter your OpenAI API Key
4. Test the connection and save

#### Google Sheets Credential

1. Go to **Credentials** → **+ Add Credential**
2. Search for **Google Sheets OAuth2 API**
3. Click **Connect my account**
4. Authorize n8n to access your Google Sheets
5. Test the connection and save

### Configure Workflow Nodes

1. **Webhook Node**: This is your entry point - copy the webhook URL after activating
2. **Google Sheets Nodes**: Update the spreadsheet ID in each node
   - To find your spreadsheet ID: Open your Google Sheet → Check the URL
   - Format: `https://docs.google.com/spreadsheets/d/{SPREADSHEET_ID}/edit`
3. **AI Nodes**: Verify OpenAI credentials are selected

---

## Step 3: Activate the Workflow

1. Review all nodes and connections
2. Click the **Inactive** toggle in the top-right to activate
3. Copy the webhook URL from the Webhook Trigger node
4. Store this URL securely - you'll use it to trigger the agent

---

## Step 4: Test the Workflow

### Using Sample Data

1. Import the sample CSV files from the `/examples` folder into your Google Sheets
2. Test each action type using the webhook:

**Test Payment Matching:**
```bash
curl -X POST {YOUR_WEBHOOK_URL} \
  -H "Content-Type: application/json" \
  -d '{"action": "payment_match"}'
```

**Test Dispute Classification:**
```bash
curl -X POST {YOUR_WEBHOOK_URL} \
  -H "Content-Type: application/json" \
  -d '{"action": "classify_dispute"}'
```

**Test Collection Email:**
```bash
curl -X POST {YOUR_WEBHOOK_URL} \
  -H "Content-Type: application/json" \
  -d '{"action": "collection_email"}'
```

---

## Troubleshooting

### Common Issues

**Issue**: Webhook returns 404
- **Solution**: Make sure the workflow is activated

**Issue**: "Credential not found" error
- **Solution**: Re-configure credentials and assign them to the correct nodes

**Issue**: Google Sheets not updating
- **Solution**: Check spreadsheet ID and sheet names match exactly

**Issue**: OpenAI API errors
- **Solution**: Verify API key is valid and has sufficient credits

---

## Next Steps

- Customize AI prompts in the AI nodes for your specific use case
- Set up scheduled executions for automated processing
- Integrate with your existing systems via webhooks
- Monitor execution logs in n8n dashboard

## Support

For issues or questions:
- Check the [n8n Community Forum](https://community.n8n.io)
- Review [OpenAI API Documentation](https://platform.openai.com/docs)
- Open an issue in this GitHub repository

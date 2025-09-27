# Azure Function App Configuration Guide
# Cisco Umbrella MSP Portal to Huntress HEC Log Shipping

## Prerequisites

1. **Cisco Umbrella MSP Portal Access**
   - Log in to your Cisco Umbrella MSP dashboard
   - Navigate to Admin > API Keys
   - Create a new API key with appropriate permissions (Read-only access to Investigate and Reports)
   - Note down the API Key and Secret (secret is only shown once)

2. **Huntress HEC Configuration**
   - Log in to your Huntress dashboard
   - Navigate to SIEM > Source Management
   - Add a new Generic HEC source
   - Note down the HEC endpoint URL and token

## API Endpoints (Verified via Postman)

- **Authentication**: `https://api.umbrella.com/auth/v2/token`
- **Reports Base**: `https://api.umbrella.com/reports/v2`
- **Example Log Endpoint**: `https://api.umbrella.com/reports/v2/activity/dns?from=-7days&to=now&limit=100`

## Azure Function App Setup

### 1. Create Function App
- In Azure Portal, create a new Function App
- Choose PowerShell as the runtime stack
- Select appropriate hosting plan (Consumption or Premium)

### 2. Configure Application Settings
Add the following environment variables in your Function App Settings:

```
UMBRELLA_API_KEY = "your_cisco_umbrella_api_key"
UMBRELLA_API_SECRET = "your_cisco_umbrella_api_secret"
HUNTRESS_HEC_URL = "https://hec.huntress.io/services/collector/raw"
HUNTRESS_HEC_TOKEN = "your_huntress_hec_token"
```

### 3. Deploy Function Files
Upload the following files to your Function App:
- `CiscoUmbrellaToHEC-AzureFunction.ps1` (rename to `run.ps1`)
- `function.json`
- `host.json`

### 4. Configure Timer Trigger
The function is configured to run every 5 minutes. To modify the schedule, edit the `schedule` property in `function.json`:
- `"0 */5 * * * *"` = Every 5 minutes
- `"0 0 */1 * * *"` = Every hour
- `"0 0 0 * * *"` = Daily at midnight

## Monitoring and Troubleshooting

### Application Insights
- Enable Application Insights for detailed logging
- Monitor function execution and performance
- Set up alerts for failures

### Common Issues
1. **Authentication Errors**: Verify API credentials are correct
2. **Network Issues**: Ensure Function App can reach external APIs
3. **Rate Limiting**: Adjust timer frequency if hitting API limits
4. **Memory Issues**: Consider upgrading to Premium plan for large log volumes

### Log Analysis
- Check Function App logs for detailed error messages
- Monitor success/error counts in function output
- Use Application Insights queries for trend analysis

## Security Considerations

- Store all sensitive credentials in Function App Settings (encrypted at rest)
- Use managed identity where possible
- Implement proper network security groups
- Regular credential rotation recommended

## Scaling Considerations

- Consumption plan automatically scales based on demand
- Premium plan provides better performance for high-volume scenarios
- Consider implementing batching for large log volumes
- Monitor costs and adjust plan as needed

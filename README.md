# Cisco Umbrella to Huntress HEC Log Shipping

This project provides two versions of a PowerShell script to ship logs from Cisco Umbrella MSP Portal to Huntress using the HEC (HTTP Event Collector) format.

## Project Structure

```
Cisco_to_HEC/
├── CiscoUmbrellaToHEC-Standalone.ps1    # Standalone PowerShell script
├── CiscoUmbrellaToHEC/                   # Azure Functions deployment
│   ├── run.ps1                          # Azure Function entry point
│   ├── function.json                    # Function configuration
│   ├── host.json                        # Host configuration
│   └── .funcignore                      # Exclusions for deployment
├── AzureFunction-SetupGuide.md          # Setup instructions
└── last_run_state.json                  # State tracking file
```

## Scripts

### Standalone Version (`CiscoUmbrellaToHEC-Standalone.ps1`)
- **Purpose**: Run locally on any Windows machine with PowerShell 7+
- **Features**: 
  - Batched HTTP requests (200 events per request)
  - High performance (232+ events/second)
  - Progress indicators
  - Error handling
- **Usage**: `pwsh -ExecutionPolicy Bypass -File "CiscoUmbrellaToHEC-Standalone.ps1"`

### Azure Functions Version (`CiscoUmbrellaToHEC/`)
- **Purpose**: Deploy as an Azure Function for automated execution
- **Features**:
  - Same batched processing as standalone
  - Timer-triggered execution
  - Environment variable configuration
  - Azure Function return format
- **Deployment**: Use Azure Functions Core Tools or VS Code Azure Functions extension

## Configuration

### Required Parameters
- `ApiKey`: Cisco Umbrella API Key
- `ApiSecret`: Cisco Umbrella API Secret  
- `HuntressHecToken`: Huntress HEC Token

### Environment Variables (Azure Functions)
- `UMBRELLA_API_KEY`
- `UMBRELLA_API_SECRET`
- `HUNTRESS_HEC_URL`
- `HUNTRESS_HEC_TOKEN`

## Performance

- **Batched Processing**: 200 events per HTTP request
- **Processing Rate**: 232+ events/second
- **Efficiency**: 7.8x faster than individual event processing
- **Reliability**: Proper error handling and retry logic

## Features

- **Comprehensive Field Mapping**: All Cisco Umbrella fields mapped to Huntress ECS format
- **Flattened Data Structure**: Complex nested objects expanded for better SIEM visibility
- **Multiple Log Types**: DNS, Proxy, Firewall, Intrusion, and IP logs
- **Incremental Processing**: 10-minute lookback window for efficient processing
- **State Management**: Tracks last run time to prevent duplicates

## Setup

See `AzureFunction-SetupGuide.md` for detailed setup instructions for both standalone and Azure Functions deployment.

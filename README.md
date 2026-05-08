# Bolt Food Weather Operations Alert System ⚡🌦️

An automated, highly-scalable weather intelligence pipeline built on **n8n** and **Python** designed for operations managers to anticipate supply drops or delivery delays.

## Quick Start (Docker)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/shumal-web/bolt-weather-alear-automation.git
   cd bolt-weather-automation
   ```

2. **Set up Environment Variables:**
   Copy the example environment file and fill in your keys:
   ```bash
   cp .env.example .env
   ```
   *Note: Ensure you add your Google Gemini API Key and Slack Webhook URL to the `.env` file.*

3. **Start the n8n Container:**
   ```bash
   docker-compose up -d
   ```

4. **Import Workflow:**
   - Open n8n at `http://localhost:5678`
   - Click **Import from File**
   - Select `bolt_weather_workflow.json`
   - Activate the workflow!

## Environment Variables Note
To ensure the n8n expression engine can securely access your `.env` variables, the `docker-compose.yml` explicitly whitelists the required keys using:
`N8N_ENV_VARIABLES_ALLOWED_TO_READ=GEMINI_API_KEY,SLACK_WEBHOOK_URL`


## Architecture

This workflow triggers every 6 hours to:
1. **Fetch:** Pulls live forecast data from the Open-Meteo API for multiple cities (Tallinn, Bucharest, Lisbon, Nairobi).
2. **Analyze (Python):** Uses a native Python `Threshold Engine` to evaluate climate-specific rules (e.g., 2cm of snow in Lisbon is critical, but normal in Tallinn) and peak-hour penalties.
3. **Deduplicate:** Tracks alerts in n8n's persistent static memory to prevent spamming Operations Managers.
4. **Synthesize (Gemini AI):** Uses Google Gemini 2.5 Flash to generate a "military-style", ruthlessly concise 10-second summary and actionable recommendations.
5. **Dispatch:** Delivers the intelligence directly to the operations team via a beautifully formatted Slack Block Kit message.



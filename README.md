# ticker-talker
AI Agentic workflow which provides a summary of potential news or events which could have impacted the stock price that day.

### Background

I created this agentic workflow to automate tracking stocks that I'm interested in and determining potential events that may have caused the positive or negative impact to the stock price.

**Phase 1**: Email analysis

**Backlog**

* Custom Alexa skill to read the analysis to me so I can multitask!
* Custom Alexa skill to easily add or remove stocks to the list.
* Ability to automate backfilling of missing info in the database.

---

### Components needed for this workflow

1. **Airtable**: this will store the list of the stocks of interest and allow the workflow to read/write yesterday's and today's closing stock price.
2. **LLM**: specifically a native web-search or retrieval capable model (e.g. gpt-5-search-api)
3. **Alpha Vantage API key**: In this workflow, I'm using Alpha Vantage, but you could replace this with another financial market data service with API functionality which provides stock closing data.
4. **n8n**
5. **Email account**: I used Gmail in this workflow.

---

### Download
https://github.com/jlau408/ticker-talker

---

### Airtable Set Up

1. From your Airtable home page (after logging in), create a new Airtable base by selecting "Import" from the left hand column.  Select CSV as the source.
2. Select "Stocks Airtable Grid view.csv" as the source to import from.
3. [Optional] If you do not like the name of the Airtable Base and Table, modify it at this time. 
3. Populate (i.e. add/remove rows) the Airtable to contain the list of your companies of interest.  The first column "TickerSymbol" needs to contain the company ticker symbol. 
3. Navigate to https://airtable.com/create/tokens and create a Personal Access Token with the following permissions:
   - data.records:read
   - data.records:write
   - schema.bases:read

---

### Alpha Vantage Set Up

1. Navigate to https://www.alphavantage.co/support/#api-key and register for a free API key from Alpha Vantage.

---

### LLM

1. Select your native web-search or retrieval capable model.  I used OpenAI's gpt-5-search-api model.
2. Create your LLM API key. https://platform.openai.com/api-keys

### n8n

1. GetTickers Workflow:
   - In your n8n instance, create a new workflow and then select "Import from File" . Select the JSON file "GetTickers.json" that you downloaded earlier and save this workflow.
   - Open the Airtable Search Records node and create your Airtable credential.
   - Select the Airtable Base and Table.
   - Save the workflow.

2. Ticker-Talker workflow:
   - In your n8n instance, create a new workflow and then select "Import from File" . Select the JSON file "ticker-talker.json" that you downloaded earlier and save this workflow.
   - Open the "HTTP Request: Alpha Vantage" node, modify the URL to insert your API key.
   - In the Bearer Auth Field for the Bearer Token, insert your API key
   - In the Update Records node, select the Airtable credential you previously created. Select the Airtable Base and Table.
   - Open the "OpenAI Chat Model" and create your credential.
   - In the "Send Message" (Gmail) node, create your credential by linking your Google account (i.e. create a Gmail OAuth2 credential to "Manage drafts and send emails"). Add your email in the "To" field.
   - Save the workflow.
   - Note: This workflow is scheduled to trigger at 2:30pm PT (i.e. after the markets close) If you want to change the time that this triggers, change the Trigger node and modify the time. Also, please note that this workflow is set for Pacific Timezone.

 ---

## References
1. **Airtable**
   - Airtable Personal Access Tokens: https://support.airtable.com/docs/creating-personal-access-tokens
2. **Alpha Vantage**
   - Alpha Vantage API Documentation: https://www.alphavantage.co/documentation/
3. **OpenAI**
   - OpenAI API Key: https://help.openai.com/en/articles/4936850-where-do-i-find-my-openai-api-key
4. **n8n**
   - n8n - Importing a workflow: https://docs.n8n.io/courses/level-one/chapter-6/#exporting-and-importing-workflows\_1
   - n8n - Gmail node: https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.gmail/
   - n8n - Google credentials: https://docs.n8n.io/integrations/builtin/credentials/google/


LinkedIn Daily Post Manager
📋 Overview
The LinkedIn Daily Post Manager is an intelligent workflow automation that helps you maintain a consistent LinkedIn presence by curating, filtering, and creating any content of preference from news articles. This n8n workflow automatically finds relevant news based on your interests, lets you select the best article, generates LinkedIn-optimized posts, and publishes them—all with minimal manual intervention.

✨ Key Features
Smart Article Discovery: Fetches relevant news articles based on topics and keywords from your Google Sheets content calendar

AI-Powered Filtering: Uses Groq's LLM to select the top 3 most relevant articles based on your personal interest notes

Human-in-the-Loop Approval: Sends a formatted email for you to choose which article to post

Professional Content Generation: Creates LinkedIn posts with varying styles (story, tips, insights) while maintaining a consistent educational tone

Automated Publishing: Posts directly to LinkedIn with your selected image

Daily Scheduling: Designed to run once per day based on your content calendar

🏗️ Architecture
Workflow Components
Trigger: Manual execution or schedule trigger

Data Source: Google Sheets (content calendar with topics, keywords, audience, notes)

Query Builder: Code node that constructs a NewsAPI query from topic/keywords with OR logic, phrase quoting, and 190-char limit

Article Search: NewsAPI `/v2/everything` returning 10 articles sorted by relevance

AI Relevance Filter: Groq LLM that scores articles against your interest notes and returns the top 3 indices

Article Linker: Code node that maps LLM-selected indices back to full article objects

Email Formatter: Code node generating an HTML email with article options (title, description, source, URL)

Human Approval: Gmail `sendAndWait` node with custom form fields (Article Number, Content, Image URL)

Response Parser: Code node that validates the user's form submission and extracts selected article + custom content

Content Generation: LLM-powered LinkedIn post creation (hook, body, key message, hashtags)

Post Formatter: Code node that structures LLM output into a ready-to-publish LinkedIn post

Image Fetcher: HTTP Request node that downloads the user-provided image URL before posting

Publishing: LinkedIn API integration with IMAGE media category

Node Flow
text
Google Sheets → Build Query → NewsAPI → Basic LLM Chain (filtering) → Link Response to Articles → Format Email Content → Send message and wait for response → Parse Email Response → Basic LLM Chain1 (post gen) → Format Text for Linkedin → Get Final Image To Post → Create a post

🚀 Setup Instructions
Prerequisites
n8n Account: Self-hosted or cloud instance

API Keys:

NewsAPI (free tier available)

Groq API (for LLM processing)

Google Sheets (service account or OAuth)

LinkedIn (OAuth 2.0)

Gmail (for email notifications)

Step-by-Step Configuration
1. Google Sheets Setup
Create a Google Sheet with the following columns:

Date (yyyy-MM-dd format)

Topic

Keywords (comma-separated)

Audience

Notes (personal interest notes for filtering)

Share the sheet with your Google Service Account

Note the Sheet ID and Sheet Name

2. API Credentials Configuration
NewsAPI
Sign up at newsapi.org

Get your API key

Add as HTTP Header Authentication in the "News Search" node

Groq API
Get API key from console.groq.com

Configure in both Groq Chat Model nodes

Model used: openai/gpt-oss-20b

LinkedIn Integration
Create LinkedIn Developer App

Configure OAuth 2.0 with w_member_social scope

Add credentials to LinkedIn node

Note: The workflow uses person URN w53dHdYuZC - replace with your LinkedIn profile ID

Gmail Setup
Enable Gmail API

Configure OAuth 2.0 in n8n

Test email sending permissions

3. Workflow Import
Copy the provided JSON

In n8n: Workflows → Import from JSON

Update all credentials marked with __rl (replace-list) values

4. Node Configuration Checklist
Google Sheets: Update Document ID and Sheet Name

News Search: Add NewsAPI key in HTTP Header Auth

Groq Chat Model (both): Add Groq API key

LinkedIn: Update Person URN to your profile ID

Gmail: Configure sender email and OAuth

5. Activate the workflow (toggle in n8n) — imported workflows start inactive

📅 Daily Operation
Content Calendar Management
Plan Ahead: Populate your Google Sheet with daily topics

Define Interests: Add detailed notes in the Notes column for better AI filtering

Audience Targeting: Specify target audience for personalized content

Daily Workflow Execution
Trigger: Execute manually or set up schedule trigger

Article Selection: Receive email with 3 article options and 3 form fields

Respond via Form: Fill out the embedded form fields (not a plain reply):

Article Number (1-3) — required

Content — optional, paste your own article text if the LLM summary needs replacement

Image URL — optional, provide a public URL for the post image

Automated Publishing: Workflow handles the rest

🔧 Customization Options
Content Style
The LLM prompt in "Basic LLM Chain1" can be modified to:

Change writing style (professional, casual, academic)

Adjust post length (currently 120-220 words)

Modify hashtag strategy

Vary tone and perspective

Article Filtering
Adjust the AI filtering by:

Modifying the prompt in "Basic LLM Chain"

Changing the number of articles returned (currently top 3)

Adjusting relevance criteria in your interest notes

Query Building
The "Build Query" node:

Takes up to 5 keywords from your sheet, multi-word phrases get double-quoted

Joins all terms with OR (broad search)

Falls back to the Topic column (or `education technology`) if no keywords exist

Enforces a 190-character max to stay within NewsAPI limits

Customizable: change `maxKeywords`, quoting strategy, join operator, or fallback text

🛠️ Technical Details
Nodes Used
n8n Core: Manual Trigger, Code, HTTP Request, Google Sheets, Gmail

LangChain: ChainLlm, LmChatGroq

Community: LinkedIn

Key Code Snippets
Query Builder (Build Query node — constructs NewsAPI query from topic/keywords with OR logic, phrase quoting, 190-char limit)

Email Formatter (Format Email Content node — HTML email with article options and form fields)

Response Parser (Parse Email Response node — validates Article Number [1-3], maps to article index, preserves custom content)

Post Formatter (Format Text for Linkedin node — structures LLM output: hook, body, key message, hashtags)

NewsAPI Request Details
- Endpoint: `/v2/everything`
- Parameters: `q` (built by Query Builder), `language=en`, `pageSize=10`, `searchIn=title,description,content`, `sortBy=relevance`
- Authentication: HTTP Header Auth with your NewsAPI key
🔒 Security & Best Practices
API Security
Use Environment Variables for all API keys

Regularly Rotate LinkedIn and Gmail tokens

Monitor API Usage to avoid rate limits

Data Privacy
Google Sheets contains your content strategy - keep access restricted

Email communications should be secure

LinkedIn posts are public - review before publishing

Error Handling
The workflow includes:

JSON parsing error handling

Input validation for user responses

Fallback queries if no articles match

📊 Monitoring & Maintenance
Success Indicators
Daily email received with article options

LinkedIn posts published consistently

Engagement on posted content

Common Issues & Solutions
Issue	Solution
No articles found	Check NewsAPI quota, verify query terms
LLM filtering fails	Verify Groq API key, check prompt formatting
LinkedIn post fails	Check OAuth token expiration
Email not sent	Verify Gmail permissions, check spam folder
Regular Maintenance
Weekly: Review content calendar for upcoming topics

Monthly: Update interest notes for better filtering

Quarterly: Refresh API tokens

As needed: Adjust LLM prompts based on post performance

📈 Performance Optimization
For Speed
NewsAPI returns 10 articles (configurable)

Groq uses fast gpt-oss-20b model

Parallel processing where possible

For Quality
AI filtering based on detailed interest notes

Human approval step ensures relevance

LLM generates varied content styles

Cost Management
NewsAPI free tier: 100 requests/day

Groq API: Pay-per-use, efficient model selection

Monitor usage in respective dashboards

🤝 Contributing & Support
Getting Help
n8n Documentation: docs.n8n.io

Community Forum: community.n8n.io

GitHub Issues: For bug reports

Custom Development
For custom modifications:

Fork the workflow JSON

Modify nodes as needed

Test thoroughly in development environment

Deploy to production

📄 License & Attribution
This workflow is provided as-is. Ensure you comply with:

NewsAPI terms of service

LinkedIn developer policies

Google API services terms

Groq usage policies

*Built with n8n - The extendable workflow automation tool.*
Last Updated: 24/06/2026
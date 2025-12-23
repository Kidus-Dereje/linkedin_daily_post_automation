# LinkedIn Daily Post Manager

## 📋 Overview

The LinkedIn Daily Post Manager is an intelligent workflow automation that helps you maintain a consistent LinkedIn presence by curating, filtering, and creating any content of preference from news articles. This n8n workflow automatically finds relevant news based on your interests, lets you select the best article, generates LinkedIn-optimized posts, and publishes them—all with minimal manual intervention.

## ✨ Key Features

- **Smart Article Discovery**: Fetches relevant news articles based on topics and keywords from your Google Sheets content calendar
- **AI-Powered Filtering**: Uses Groq's LLM to select the top 3 most relevant articles based on your personal interest notes
- **Human-in-the-Loop Approval**: Sends a formatted email for you to choose which article to post
- **Professional Content Generation**: Creates LinkedIn posts with varying styles (story, tips, insights) while maintaining a consistent educational tone
- **Automated Publishing**: Posts directly to LinkedIn with your selected image
- **Daily Scheduling**: Designed to run once per day based on your content calendar

## 🏗️ Architecture

### Workflow Components

1. **Trigger**: Manual execution or schedule trigger
2. **Data Source**: Google Sheets (content calendar with topics, keywords, audience, notes)
3. **Article Search**: NewsAPI for fetching relevant articles
4. **AI Filtering**: Groq LLM for relevance scoring
5. **Human Approval**: Email interface for article selection
6. **Content Creation**: LLM-powered LinkedIn post generation
7. **Publishing**: LinkedIn API integration with image support

### Node Flow
Google Sheets → Query Builder → NewsAPI → AI Filtering → Email Selection → Content Generation → LinkedIn Post


## 🚀 Setup Instructions

### Prerequisites

1. **n8n Account**: Self-hosted or cloud instance
2. **API Keys**:
   - NewsAPI (free tier available)
   - Groq API (for LLM processing)
   - Google Sheets (service account or OAuth)
   - LinkedIn (OAuth 2.0)
   - Gmail (for email notifications)

### Step-by-Step Configuration

#### 1. Google Sheets Setup

Create a Google Sheet with the following columns:

- `Date` (yyyy-MM-dd format)
- `Topic`
- `Keywords` (comma-separated)
- `Audience`
- `Notes` (personal interest notes for filtering)

Share the sheet with your Google Service Account

Note the Sheet ID and Sheet Name

#### 2. API Credentials Configuration

##### NewsAPI

1. Sign up at [newsapi.org](https://newsapi.org)
2. Get your API key
3. Add as HTTP Header Authentication in the "News Search" node

##### Groq API

1. Get API key from [console.groq.com](https://console.groq.com)
2. Configure in both Groq Chat Model nodes
3. Model used: `openai/gpt-oss-20b`

##### LinkedIn Integration

1. Create LinkedIn Developer App
2. Configure OAuth 2.0 with `w_member_social` scope
3. Add credentials to LinkedIn node
4. **Note**: The workflow uses person URN `w53dHdYuZC` - replace with your LinkedIn profile ID

##### Gmail Setup

1. Enable Gmail API
2. Configure OAuth 2.0 in n8n
3. Test email sending permissions

#### 3. Workflow Import

1. Copy the provided JSON
2. In n8n: Workflows → Import from JSON
3. Update all credentials marked with `__rl` (replace-list) values

#### 4. Node Configuration Checklist

- [ ] Google Sheets: Update Document ID and Sheet Name
- [ ] News Search: Add NewsAPI key in HTTP Header Auth
- [ ] Groq Chat Model (both): Add Groq API key
- [ ] LinkedIn: Update Person URN to your profile ID
- [ ] Gmail: Configure sender email and OAuth

## 📅 Daily Operation

### Content Calendar Management

1. **Plan Ahead**: Populate your Google Sheet with daily topics
2. **Define Interests**: Add detailed notes in the Notes column for better AI filtering
3. **Audience Targeting**: Specify target audience for personalized content

### Daily Workflow Execution

1. **Trigger**: Execute manually or set up schedule trigger
2. **Article Selection**: Receive email with 3 article options
3. **Provide Content**: Reply with:
   - Article number (1-3)
   - Optional: Custom content/text
   - Optional: Image URL for the post
4. **Automated Publishing**: Workflow handles the rest

## 🔧 Customization Options

### Content Style

The LLM prompt in "Basic LLM Chain1" can be modified to:

- Change writing style (professional, casual, academic)
- Adjust post length (currently 120-220 words)
- Modify hashtag strategy
- Vary tone and perspective

### Article Filtering

Adjust the AI filtering by:

- Modifying the prompt in "Basic LLM Chain"
- Changing the number of articles returned (currently top 3)
- Adjusting relevance criteria in your interest notes

### Query Building

The "Build Query" node can be modified to:

- Change number of keywords (currently 5 max)
- Adjust search query logic
- Modify news sources or languages

## 🛠️ Technical Details

### Nodes Used

- **n8n Core**: Manual Trigger, Code, HTTP Request, Google Sheets, Gmail
- **LangChain**: ChainLlm, LmChatGroq
- **Community**: LinkedIn

### Key Code Snippets

#### Query Builder (`Build Query` node)
// Builds NewsAPI query from topics and keywords

// Handles quotes, OR logic, and character limits


#### Email Formatter (`Format Email Content` node)
// Creates HTML email with article options

// Includes titles, descriptions, and source information


#### LinkedIn Formatter (`Format Text for Linkedin` node)
// Structures LLM output into LinkedIn post format

// Combines hook, body, key message, and hashtags


## 🔒 Security & Best Practices

### API Security

1. **Use Environment Variables** for all API keys
2. **Regularly Rotate** LinkedIn and Gmail tokens
3. **Monitor API Usage** to avoid rate limits

### Data Privacy

- Google Sheets contains your content strategy - keep access restricted
- Email communications should be secure
- LinkedIn posts are public - review before publishing

### Error Handling

The workflow includes:

- JSON parsing error handling
- Input validation for user responses
- Fallback queries if no articles match

## 📊 Monitoring & Maintenance

### Success Indicators

- Daily email received with article options
- LinkedIn posts published consistently
- Engagement on posted content

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| No articles found | Check NewsAPI quota, verify query terms |
| LLM filtering fails | Verify Groq API key, check prompt formatting |
| LinkedIn post fails | Check OAuth token expiration |
| Email not sent | Verify Gmail permissions, check spam folder |

### Regular Maintenance

1. **Weekly**: Review content calendar for upcoming topics
2. **Monthly**: Update interest notes for better filtering
3. **Quarterly**: Refresh API tokens
4. **As needed**: Adjust LLM prompts based on post performance

## 📈 Performance Optimization

### For Speed

- NewsAPI returns 10 articles (configurable)
- Groq uses fast `gpt-oss-20b` model
- Parallel processing where possible

### For Quality

- AI filtering based on detailed interest notes
- Human approval step ensures relevance
- LLM generates varied content styles

### Cost Management

- NewsAPI free tier: 100 requests/day
- Groq API: Pay-per-use, efficient model selection
- Monitor usage in respective dashboards

## 🤝 Contributing & Support

### Getting Help

1. **n8n Documentation**: [docs.n8n.io](https://docs.n8n.io)
2. **Community Forum**: [community.n8n.io](https://community.n8n.io)
3. **GitHub Issues**: For bug reports

### Custom Development

For custom modifications:

1. Fork the workflow JSON
2. Modify nodes as needed
3. Test thoroughly in development environment
4. Deploy to production

## 📄 License & Attribution

This workflow is provided as-is. Ensure you comply with:

- NewsAPI terms of service
- LinkedIn developer policies
- Google API services terms
- Groq usage policies

---

*Built with n8n - The extendable workflow automation tool.*  
*Last Updated: 23/12/2025*

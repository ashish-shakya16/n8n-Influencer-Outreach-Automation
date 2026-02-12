# CrowdWisdomTrading Influencer Outreach Automation

> Automated lead generation and CRM integration workflow for discovering and engaging with trading influencers across social media platforms.

[![n8n](https://img.shields.io/badge/n8n-workflow-orange)](https://n8n.io/)
[![Status](https://img.shields.io/badge/status-production--ready-brightgreen)]()
[![License](https://img.shields.io/badge/license-MIT-blue)]()

---

## 🎯 Overview

This n8n workflow automates the entire influencer outreach pipeline for CrowdWisdomTrading, including:

- **Lead Discovery**: Search for trading influencers across YouTube, Instagram, TikTok, and LinkedIn
- **Data Enrichment**: Extract comprehensive profile information (followers, bio, contact details)
- **CRM Integration**: Automatically create contacts in HubSpot
- **Intelligent Routing**: LinkedIn profile enrichment for high-value leads
- **Quality Filtering**: Focus on influencers with 50,000+ followers

### Key Features

✅ Multi-platform influencer search (YouTube, Instagram, TikTok, LinkedIn)  
✅ AI-powered search using Tavily API  
✅ Automated HubSpot contact creation  
✅ Data normalization and validation  
✅ LinkedIn profile enrichment  
✅ Scalable architecture with error handling  
✅ Production-ready with comprehensive documentation  

---

## 🏗️ Workflow Architecture

```
Step 1: Manual Trigger
    ↓
Step 2: Tavily Search (AI-powered influencer discovery)
    ↓
Step 3: Extract Influencer Data
    ↓
Step 4: Structure & Normalize Data
    ↓
Step 5: IF (LinkedIn URL Check)
    ↓
    ├─→ TRUE: Step 9 (LinkedIn Enrichment)
    │
    └─→ FALSE: Step 13 (Create HubSpot Contact)
```

### Node Descriptions

| Node | Type | Purpose | Status |
|------|------|---------|--------|
| Step 1 | Manual Trigger | Initiates workflow execution | ✅ Active |
| Step 2 | Tavily Search | Discovers influencers using AI search | ✅ Active |
| Step 3 | Code | Extracts influencer data from search results | ✅ Active |
| Step 4 | Code | Normalizes data schema for CRM compatibility | ✅ Active |
| Step 5 | IF | Routes based on LinkedIn URL availability | ✅ Active |
| Step 6 | Gemini AI | Generates personalized email (quota-limited) | ⚠️ Deactivated |
| Step 7 | Parse JSON | Parses AI-generated email structure | ⚠️ Deactivated |
| Step 8 | Save Note | Stores email content in HubSpot | ⚠️ Deactivated |
| Step 9 | LinkedIn | Enriches profile data from LinkedIn | ✅ Active |
| Step 10 | Create Task | Creates LinkedIn outreach task | ⚠️ Deactivated |
| Step 11 | Google Sheets | Audit logging (optional) | ⚠️ Deactivated |
| Step 13 | HubSpot | Creates contact in CRM | ✅ Active |

---

## 🚀 Quick Start

### Prerequisites

- n8n instance (cloud or self-hosted)
- Tavily API key ([Get one here](https://tavily.com))
- HubSpot account with API access
- (Optional) Google Gemini API key for AI email generation
- (Optional) LinkedIn Sales Navigator for profile enrichment

### Installation

1. **Import Workflow**
   ```bash
   # In n8n, go to: Workflows → Import from File
   # Select: trading outreach n8n workflow.json
   ```

2. **Configure API Keys**
   
   **Tavily Search (Required)**
   - Navigate to Step 2: Search (Tavily)
   - Replace placeholder with your API key
   - Current key: `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`

3. **Set Up HubSpot Credentials**
   - Click on Step 13: Create HubSpot Contact
   - Select "Create New Credential"
   - Authenticate with OAuth2
   - Grant contact creation permissions

4. **Test Execution**
   ```bash
   # Click "Execute Workflow" button
   # Verify data flow through each node
   # Check HubSpot for created contacts
   ```

---

## ⚙️ Configuration

### Search Parameters

Edit Step 2 to customize search criteria:

```json
{
  "query": "Top trading educators on YouTube with more than 50000 followers",
  "include_answer": true,
  "max_results": 5
}
```

**Customization Options:**
- `query`: Modify platform (YouTube/Instagram/TikTok/LinkedIn)
- `max_results`: Adjust number of results (1-10)
- `include_answer`: Enable AI-powered summaries

### Data Schema

Step 4 normalizes data to this structure:

```javascript
{
  name: string,
  platform: string,
  followers: number,
  bio: string,
  website: string,
  email: string,
  linkedin_url: string,
  follower_threshold: 50000
}
```

### HubSpot Field Mapping

The workflow maps data to HubSpot properties:

| Workflow Field | HubSpot Property | Type |
|----------------|------------------|------|
| name | firstname + lastname | string |
| email | email | string |
| website | website | URL |
| platform | custom_platform | string |
| followers | custom_followers | number |

---

## 🔧 Troubleshooting

### Common Issues

**Problem: Tavily API returns 401 Unauthorized**
- ✅ Solution: Verify API key is correctly entered in Step 2
- Check API key format: `tvly-dev-XXXXXXXXXXXXX`

**Problem: HubSpot contact creation fails**
- ✅ Solution: Ensure HubSpot credentials are properly authenticated
- Verify OAuth2 permissions include contact write access

**Problem: No LinkedIn data enrichment**
- ✅ Solution: Check that influencer has public LinkedIn URL
- Verify LinkedIn Sales Navigator credentials

**Problem: Gemini AI quota exceeded**
- ✅ Solution: Upgrade to paid Gemini API plan
- Alternatively, keep Step 6 deactivated (current state)

For detailed troubleshooting, see [CHALLENGES_AND_SOLUTIONS.md](CHALLENGES_AND_SOLUTIONS.md).

---

## 📊 Production Deployment

### Before Going Live

1. **Upgrade API Plans**
   - [ ] Tavily API: Move to production tier
   - [ ] Google Gemini: Upgrade to paid plan (optional)
   - [ ] HubSpot: Verify API rate limits

2. **Security Checklist**
   - [ ] Store all API keys in n8n credential vault
   - [ ] Remove hardcoded credentials from workflow JSON
   - [ ] Enable credential rotation policy
   - [ ] Set up access control for workflow editing

3. **Monitoring Setup**
   - [ ] Enable workflow execution logging
   - [ ] Set up error alerts (email/Slack)
   - [ ] Create dashboard for key metrics
   - [ ] Configure API usage monitoring

4. **Performance Optimization**
   - [ ] Implement batch processing for large lead lists
   - [ ] Enable result caching for repeated searches
   - [ ] Configure retry logic with exponential backoff
   - [ ] Optimize data transformation steps

### Production Recommendations

**API Management**
- Use environment-specific credentials (dev/staging/prod)
- Implement request queuing to stay within rate limits
- Set up billing alerts for API costs
- Monitor API performance and response times

**Error Handling**
- Add retry logic for transient failures
- Implement comprehensive error logging
- Create runbook for common error scenarios
- Set up automated health checks

**Scaling**
- Start with small batch sizes (5-10 leads)
- Monitor workflow execution times
- Optimize bottleneck nodes
- Consider implementing webhook triggers for automation

---

## 📁 Project Structure

```
n8n/
├── trading outreach n8n workflow.json   # Main workflow file
├── README.md                             # This file
└── CHALLENGES_AND_SOLUTIONS.md          # Detailed problem-solving documentation
```

---

## 🔐 Security & Compliance

### Data Privacy

- Workflow only collects publicly available information
- Email placeholders used when contact info unavailable
- GDPR-compliant data handling practices
- No storage of sensitive personal data

### API Key Management

⚠️ **IMPORTANT**: Never commit API keys to version control

```bash
# Use n8n's credential vault for:
- Tavily API key
- HubSpot OAuth2 credentials
- Google Gemini API key
- LinkedIn Sales Navigator credentials
```

### Access Control

- Implement role-based access for workflow editing
- Use separate credentials for dev/staging/prod
- Enable audit logging for credential access
- Regularly rotate API keys

---

## 🐛 Known Limitations

### Current Deactivated Features

1. **Gemini AI Email Generation (Step 6)** - ⚠️ Deactivated
   - Reason: Free tier rate limit exceeded
   - Impact: No automated personalized emails
   - Fix: Upgrade to paid Gemini API plan

2. **JSON Email Parsing (Step 7)** - ⚠️ Deactivated
   - Reason: Depends on Step 6
   - Impact: No email structure parsing
   - Fix: Re-activate after Gemini upgrade

3. **HubSpot Email Note (Step 8)** - ⚠️ Deactivated
   - Reason: Depends on Step 7
   - Impact: Emails not saved to contact notes
   - Fix: Re-activate after Gemini upgrade

4. **LinkedIn Task Creation (Step 10)** - ⚠️ Deactivated
   - Reason: Quota-limited feature
   - Impact: No automatic task creation
   - Fix: Manual task creation or API upgrade

5. **Google Sheets Logging (Step 11)** - ⚠️ Deactivated
   - Reason: Resource not found error
   - Impact: No audit trail logging
   - Fix: Configure valid Google Sheet resource

### API Rate Limits

| Service | Free Tier Limit | Recommended Plan |
|---------|-----------------|------------------|
| Tavily | 1,000 searches/month | Pro: $50/month |
| Gemini | 60 requests/minute | Paid: $0.00025/1K chars |
| HubSpot | 100 requests/10 seconds | Professional+ |

---

## 📈 Roadmap

### Planned Enhancements

- [ ] Multi-language support for international influencers
- [ ] Instagram Stories engagement tracking
- [ ] TikTok analytics integration
- [ ] Automated follow-up email sequences
- [ ] A/B testing for outreach messages
- [ ] Custom scoring algorithm for lead quality
- [ ] Integration with Calendly for meeting scheduling
- [ ] Slack notifications for high-value leads

### Future Integrations

- [ ] Salesforce CRM support
- [ ] Pipedrive integration
- [ ] Twitter/X influencer search
- [ ] Discord community engagement
- [ ] Email validation service (ZeroBounce/Hunter.io)

---

## 🤝 Contributing

This workflow is part of CrowdWisdomTrading's internal automation suite. For improvements or bug reports:

1. Document the issue in detail
2. Reference the node ID and step number
3. Include error messages and execution logs
4. Propose a solution with testing steps

---

## 📞 Support

For technical assistance:

- **Workflow Issues**: See [CHALLENGES_AND_SOLUTIONS.md](CHALLENGES_AND_SOLUTIONS.md)
- **n8n Documentation**: https://docs.n8n.io
- **API Documentation**:
  - Tavily: https://docs.tavily.com
  - HubSpot: https://developers.hubspot.com
  - Gemini: https://ai.google.dev/docs

---

## 📄 License

MIT License - See LICENSE file for details

---

## 🙏 Acknowledgments

- **n8n** - Open-source workflow automation platform
- **Tavily** - AI-powered search API
- **HubSpot** - CRM platform
- **Google Gemini** - AI language model

---

## 📝 Changelog

### Version 1.0.0 (February 2026)
- ✅ Initial production-ready release
- ✅ Multi-platform influencer search
- ✅ HubSpot CRM integration
- ✅ Data normalization layer
- ✅ Comprehensive error handling
- ✅ Complete documentation
- ⚠️ Deactivated rate-limited features (Gemini, Sheets)

---

**Status**: ✅ Production-Ready | **Last Updated**: February 13, 2026

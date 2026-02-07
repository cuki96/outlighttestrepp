# Outlight Analytics - AI Agent Skill

**Real-time cryptocurrency token call analytics for AI agents**

Track KOL performance, discover trending tokens, and analyze caller win rates from Telegram and Twitter channels.

---

## 🚀 Quick Deploy (5 minutes)

### Option 1: GitHub Pages (Recommended for testing)

```bash
# 1. Create new public repo on GitHub
# Name: outlight-skill

# 2. Clone and push
git clone <your-repo-url>
cd outlight-skill
cp /path/to/skill.md .
git add skill.md README.md
git commit -m "Add Outlight Analytics skill"
git push

# 3. Enable GitHub Pages
# Go to: Settings → Pages → Source: main branch → Save

# Your skill will be live at:
# https://YOUR_USERNAME.github.io/outlight-skill/skill.md
```

### Option 2: Cloudflare Pages (Recommended for production)

```bash
# 1. Push to GitHub (as above)
# 2. Go to Cloudflare Dashboard
# 3. Pages → Create project → Connect GitHub
# 4. Select repo → Deploy

# Your skill will be live at:
# https://outlight-skill.pages.dev/skill.md
```

---

## 📋 What You Need

To deploy the Outlight skill to your GitHub:

### 1. Files to upload:
- ✅ `skill.md` - Main skill definition (already created)
- ✅ `README.md` - This file

### 2. GitHub repository:
- Create a new **public** repository
- Name suggestion: `outlight-skill` or `outlight-ai-skill`

### 3. After deployment:
- Enable GitHub Pages in repository settings
- Get the public URL: `https://YOUR_USERNAME.github.io/REPO_NAME/skill.md`

---

## 🔗 API Information

**Base URL:** `https://prod.api.sauron.outlight.fun`

**Status:** ✅ Live and operational

**Authentication:** None required (public read-only API)

**Rate Limits:** 
- 100 requests/minute per IP
- 1000 requests/hour per IP

---

## 📝 Submit to AI Platforms

### Meta AI / Moltbook

1. Find submission form on Moltbook platform
2. Submit:
   - **Skill Name:** Outlight Analytics
   - **Skill URL:** `https://YOUR_USERNAME.github.io/outlight-skill/skill.md`
   - **Category:** Trading, Cryptocurrency, Analytics
   - **Description:** Real-time crypto token call analytics from Telegram and Twitter KOLs

### OpenAI ChatGPT

1. Go to https://chat.openai.com
2. Explore GPTs → Create
3. Configure → Actions
4. Import from URL: `https://YOUR_USERNAME.github.io/outlight-skill/skill.md`

### Anthropic Claude

1. Go to https://console.anthropic.com
2. Tools → Add Tool
3. Paste skill URL

---

## 🧪 Testing

### Test API directly:

```bash
# Search for channels
curl "https://prod.api.sauron.outlight.fun/api/channels/search?q=crypto&limit=3"

# Get top channels by win rate
curl "https://prod.api.sauron.outlight.fun/api/channels?sort=win_rate&order=desc&limit=5"

# Get recently called tokens
curl "https://prod.api.sauron.outlight.fun/api/tokens/recent?limit=5"
```

### Test with AI Agent:

**Prompt for ChatGPT/Claude:**
```
Use the Outlight Analytics skill to find the top 5 crypto callers 
from the last 7 days with the highest win rates. Include their 
average gains and number of calls analyzed.
```

**Expected response:**
```
Based on Outlight Analytics data, here are the top 5 callers:

1. @CryptoGems (Telegram)
   - Win Rate: 73%
   - Average Gain: +432%
   - Calls Analyzed: 15

2. @SolanaAlpha (Twitter)
   - Win Rate: 68%
   - Average Gain: +321%
   - Calls Analyzed: 22

...
```

---

## 📊 Available Endpoints

The skill provides access to:

- **Channel Search** - Find KOLs by name/keywords
- **Channel List** - Browse all tracked channels with filters
- **Channel Info** - Detailed performance metrics
- **Channel Calls** - All token calls from a specific channel
- **Token Search** - Find tokens by address/name/symbol
- **Recent Tokens** - Latest token calls
- **Most Called Tokens** - Trending tokens by call frequency
- **Token Details** - Complete token information with call history
- **Statistics** - Aggregate platform statistics

See `skill.md` for complete API documentation.

---

## 🎯 Use Cases for AI Agents

### 1. Discover Top Performers
Find channels with high win rates for reliable trading signals

### 2. Track Trending Tokens
Identify tokens being called frequently in real-time

### 3. Analyze KOL Performance
Review historical performance before following a caller

### 4. Verify Proof-of-Call
Access original message links for transparency

### 5. Research Before Trading
Get comprehensive data about tokens and their callers

---

## 🔒 Security & Rate Limits

- ✅ **Public API** - No authentication required
- ✅ **Read-only** - No write operations
- ✅ **Rate limited** - 100 req/min, 1000 req/hour
- ✅ **Cached** - 2-5 minute cache for performance
- ✅ **CORS enabled** - Works from any origin

---

## 📈 Monitoring (Optional)

Track skill usage by monitoring API requests with User-Agent containing:
- `GPT` (ChatGPT)
- `Claude` (Anthropic)
- `AI-Agent` (Generic AI agents)
- `Moltbook` (Meta AI agents)

---

## 🐛 Troubleshooting

### Issue: 404 Not Found

**Solution:**
- Ensure GitHub Pages is enabled
- Check file is named `skill.md` (lowercase)
- Wait 2-3 minutes after first deployment

### Issue: AI platform rejects skill

**Solution:**
- Verify YAML header format in `skill.md`
- Ensure URL is publicly accessible
- Check that content-type is `text/markdown` or `text/plain`

### Issue: Rate limit errors

**Solution:**
- Respect rate limits: 100/min, 1000/hour
- Implement exponential backoff
- Use cached responses when possible

---

## 📚 Resources

- **Full API Documentation:** https://outlightfun.mintlify.app/
- **Website:** https://outlight.fun/
- **API Base URL:** https://prod.api.sauron.outlight.fun
- **Example Skill:** https://molt.api.cookie.fun/skill.md (Moltbook reference)

---

## 🎉 Launch Checklist

- [ ] Create GitHub repository
- [ ] Upload `skill.md` and `README.md`
- [ ] Enable GitHub Pages
- [ ] Test skill URL is accessible
- [ ] Submit to Moltbook/Meta AI
- [ ] Submit to OpenAI (optional)
- [ ] Submit to Anthropic (optional)
- [ ] Test with real AI agent
- [ ] Monitor usage
- [ ] Share on social media

---

## 💬 Support

Questions or issues?
- **Documentation:** https://outlightfun.mintlify.app/
- **Twitter:** @OutlightAI
- **Website:** https://outlight.fun/

---

## 📄 License

This skill provides access to Outlight's public API. The API is provided as-is for AI agents to make better crypto trading decisions.

**Last Updated:** January 2026  
**Version:** 1.0.0
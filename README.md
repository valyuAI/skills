# Valyu Agent Skills

Agent skills for integrating [Valyu](https://valyu.ai) APIs into AI agents and assistants. 

These skills follow the [Agent Skills](https://agentskills.io) specification, making them compatible with Claude Code, Claude Desktop, and other agent platforms.

## What is Valyu?

Valyu provides a unified API for AI agents to access real-time information across:

- **Web** - General internet content
- **Academic** - arXiv, PubMed, bioRxiv, medRxiv (40M+ papers)
- **Financial** - SEC filings, earnings, stock data, crypto
- **Healthcare** - Clinical trials, drug labels, ChEMBL, DrugBank
- **Economic** - FRED, BLS, World Bank data
- **News** - Real-time news with date/country filtering
- **Predictions** - Polymarket, Kalshi odds
- **Transportation** - UK Rail, ship tracking
- **Patents** - US patent database

## Available Skills

| Skill | Description |
|-------|-------------|
| [valyu-search](valyu-search/valyu-best-practices/SKILL.md) | Complete Valyu API toolkit - search, content extraction, AI answers, deep research |

## Quick Start

### For AI Agents

## Installation

```bash
npx add-skill valyu-network/skills
```

The skill provides instructions for:
- **Search API** - Find information across 25+ data sources
- **Contents API** - Extract clean markdown from URLs
- **Answer API** - Get AI-synthesized answers with citations
- **DeepResearch API** - Generate comprehensive research reports

### For Developers

1. Get a Valyu API key at [platform.valyu.ai](https://platform.valyu.ai) ($10 free credits)

2. Install the SDK:
   ```bash
   npm install valyu-js
   # or
   pip install valyu
   ```

3. Use the API:
   ```typescript
   import Valyu from 'valyu-js';

   const valyu = new Valyu("your-api-key");

   // Search across all sources
   const results = await valyu.search({
     query: "transformer architecture attention mechanism 2024",
     search_type: "all",
     max_num_results: 10
   });

   // Get AI-powered answer
   const answer = await valyu.answer({
     query: "What are the latest developments in quantum computing?"
   });

   // Extract content from URL
   const content = await valyu.contents({
     urls: ["https://example.com/article"],
     summary: "Extract key findings in 3 bullet points"
   });

   // Deep research (async)
   const task = await valyu.deepResearch.create({
     query: "AI chip market competitive landscape 2024",
     model: "standard"  // fast (~5min), standard (~15min), heavy (~90min)
   });
   ```

## Project Structure

```
valyu-agent-skills/
├── README.md
└── valyu-search/
    ├── scripts/                    # CLI tools
    └── best-practices/
        ├── SKILL.md                # Main skill file (start here)
        └── references/
            ├── api-guide.md        # Complete API documentation
            ├── recipes.md          # Index of all 27 recipes
            ├── datasources.md      # Available data sources
            ├── prompting.md        # Query writing best practices
            ├── design-philosophy.md
            ├── search-recipes/     # 14 search patterns
            ├── content-recipes/    # 6 content extraction patterns
            ├── answer-recipes/     # 4 answer generation patterns
            ├── deepresearch-recipes/  # 3 research depth levels
            └── integrations/       # 12 platform guides
```

## API Decision Tree

```
What do you need?

├─ Find information across multiple sources
│  └─ Search API (/v1/search)
│
├─ Extract content from specific URLs
│  └─ Contents API (/v1/contents)
│
├─ Get an AI-synthesized answer with citations
│  └─ Answer API (/v1/answer)
│
├─ Generate a comprehensive research report
│  └─ DeepResearch API (/v1/deepresearch)
│
└─ Discover available data sources
   └─ Datasources API (/v1/datasources)
```

## Recipes

### Search Recipes

| Recipe | Use Case |
|--------|----------|
| [basic-search-all](valyu-search/best-practices/references/search-recipes/basic-search-all.md) | Search across all sources |
| [academic-search](valyu-search/best-practices/references/search-recipes/academic-search.md) | Research papers (arXiv, PubMed) |
| [finance-search](valyu-search/best-practices/references/search-recipes/finance-search.md) | SEC filings, earnings, stocks |
| [news-search](valyu-search/best-practices/references/search-recipes/news-search.md) | Real-time news with filtering |
| [healthcare-and-bio-search](valyu-search/best-practices/references/search-recipes/healthcare-and-bio-search.md) | Clinical trials, drug data |

### Content Recipes

| Recipe | Use Case |
|--------|----------|
| [basic-content-extraction](valyu-search/best-practices/references/content-recipes/basic-content-extraction-from-web.md) | Clean markdown from URLs |
| [structured-extraction](valyu-search/best-practices/references/content-recipes/structured-content-extraction-from-web.md) | Extract data via JSON schema |

### Answer Recipes

| Recipe | Use Case |
|--------|----------|
| [basic-answer](valyu-search/best-practices/references/answer-recipes/basic-answer.md) | AI answers with citations |
| [answer-with-streaming](valyu-search/best-practices/references/answer-recipes/answer-with-streaming.md) | Progressive response generation |

### DeepResearch Recipes

| Recipe | Duration | Use Case |
|--------|----------|----------|
| [fast-research](valyu-search/best-practices/references/deepresearch-recipes/create-a-fast-research-task-and-await-completion.md) | ~5 min | Quick lookups |
| [standard-research](valyu-search/best-practices/references/deepresearch-recipes/create-a-standard-research-task-and-await-completion.md) | ~15 min | Standard research |
| [heavy-research](valyu-search/best-practices/references/deepresearch-recipes/create-a-heavy-research-task-and-await-completion.md) | ~90 min | Comprehensive analysis |

## Integration Guides

| Platform | Guide |
|----------|-------|
| Anthropic Claude | [integrations/anthropic.md](valyu-search/best-practices/references/integrations/anthropic.md) |
| OpenAI | [integrations/openai.md](valyu-search/best-practices/references/integrations/openai.md) |
| Vercel AI SDK | [integrations/vercel-ai-sdk.md](valyu-search/best-practices/references/integrations/vercel-ai-sdk.md) |
| LangChain | [integrations/langchain.md](valyu-search/best-practices/references/integrations/langchain.md) |
| LlamaIndex | [integrations/llamaindex.md](valyu-search/best-practices/references/integrations/llamaindex.md) |
| MCP Server | [integrations/mcp-server.md](valyu-search/best-practices/references/integrations/mcp-server.md) |

## Query Best Practices

**Keep queries under 400 characters. Be specific, not verbose.**

```
BAD:  "I want to know about AI"
GOOD: "transformer attention mechanism survey 2024"

BAD:  "Apple financial information"
GOOD: "Apple revenue growth Q4 2024 earnings SEC filing"
```

**Split complex requests into multiple queries:**

```
# Don't do this
"Tesla stock, products, and news"

# Do this instead
Query 1: "Tesla stock performance Q4 2024"
Query 2: "Tesla Cybertruck production updates"
Query 3: "Tesla FSD autonomous driving progress"
```

See [prompting.md](valyu-search/best-practices/references/prompting.md) for the complete guide.

## Links

- [Valyu Platform](https://platform.valyu.ai) - Get API key
- [Valyu Documentation](https://docs.valyu.ai) - Official docs
- [Agent Skills Specification](https://agentskills.io) - Skill format spec
- [valyu-js](https://www.npmjs.com/package/valyu-js) - TypeScript/JavaScript SDK
- [valyu (Python)](https://pypi.org/project/valyu/) - Python SDK

## License

MIT

This repository contains all of my work completed during the **Wayfair AI Agent Engineering for Business Intelligence Externship**.

I designed and built multiple AI agents using **n8n**, integrating LLMs, web scraping, structured data processing, prompt engineering, and automated report generation.

- Consumer Trend Discovery AI Agent - Discovering emerging consumer trends from multiple sources and produces structured trend reports
- Competitor Monitoring AI Agent - Monitoring competitor products
- AI Insights & Content Strategy Agent - Generating marketing content
- Dashboard Builder Agent - Producing executive dashboards & automating business reporting



## Consumer Trend Discovery AI Agent:

STAGE 1: INPUT & ROUTING
Parses user input to extract
- Category (Area Rug, Outdoor Rug, Hallway Runner, Shag Rug)
- Optional focus keyword

STAGE 2A: AMAZON PRODUCTS
Single API call to FastAPI server (/api/products/amazon)
The server returns upto 10 pre-scraped Amazon products for the selected category
If the API returns 0 products (rare), the workflow short-circuits with a clear error message

STAGE 2B: EXTERN API + IMAGE ANALYSIS
Calls FastAPI server for social data:
- Instagram trends (with images)
- Pinterest boards (with images)
- Blog Posts (text-only)

Gemini Vision analyzes social images:
- Extracts colors, patterns, styles
- Identifies textures and moods
- Provides visual trend summary

Handles API failures

STAGE 3: AI PROCESSING
Structuring the Scraped Data with AI Agents
1. Judge Product Category (OpenRouter AI Agent) - Filters products to match selected category
2. Standardize Details (OpenRouter AI Agent) - Normalizes size, color, material, pattern, style, price
3. Identify Top Trends (OpenRouter AI Agent) - Finalizes 2-3 style micro-segments

STAGE 4: IMAGE GENERATION
Generating Trend Images from AI Micro-Segments
For each micro-segment:
1. Generate Image Prompt (OpenRouter / DeepSeek free)
2. Clean Prompt
3. Hugging Face model
4. Encode to base64 for HTML embedding

STAGE 5-6 MERGE & REPORT
Merge all data into single structure
Sequential Mistral models (mistral-large-latest) generates and writes the report sections:

STAGE 7: OUTPUT
- HTML cleanup & validation
- Section presence check
- Create a downloadable HTML file



## Competitor Monitoring Agent

STAGE 1: INPUT
Parses chat message to detect the rug category and optional focus keyword.
Supported categories: Area Rug, Outdoor Rug, Hallway Runner, Shag Rug

STAGE 2: WAYFAIR PRODUCTS
Fetches up to 10 Wayfair products for the selected category.

STAGE 3: AMAZON PRODUCTS
Fetches up to 10 Amazon products for the selected category.

STAGE 4: WALMART PRODUCTS
Fetches up to 10 Walmart products for the selected category.

STAGE 5: AI ANALYSIS & REPORT GENERATION
Six AI Agents work sequentially to write each section of the report:
- Executive Summary - Top line findings (Mistral)
- Amazon Analysis - Amazon specific insights (OpenRouter)
- Comparison - head to head retailer comparison (Mistral)
- Pricing & Whitespace - pricing gaps and opportunities (Mistral)
- Recommendations - strategic next steps (Mistral)
- Supplier Identification - third party suppliers worth tracking (OpenRouter)

Each section runs one after the other with a short pause for rate limit safety.

STAGE 6: OUTPUT
The Assemble HTML node combines all sections into a single report



## Content Strategy Generator


## Dashboard Builder Agent

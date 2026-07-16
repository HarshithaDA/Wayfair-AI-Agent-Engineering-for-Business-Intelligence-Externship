I designed and built multiple AI agents using **n8n**, integrating LLMs, web scraping, structured data processing, prompt engineering, and automated report generation.

- Consumer Trend Discovery AI Agent:
Discovers emerging consumer design trends by analyzing products, social media, and market data to generate AI-powered market intelligence reports.

- Competitor Monitoring AI Agent:
Collects and compares competitor product data across major retailers to get pricing strategies, market gaps, and actionable business insights.

- AI Insights & Content Agent:
Transforms market trends and competitor intelligence into personalized, Wayfair-branded marketing strategies and content recommendations using AI.

- Dashboard Builder Agent:
Combines trend and competitor reports into a single interactive Business Intelligence dashboard for executive decision-making and market analysis.

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
Sequential Mistral models (mistral-large-latest) generates and writes the report sections

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
STAGE 1: INPUT COLLECTION
- Upload Consumer Trend Discovery (P2) and Competitor Monitoring (P3) HTML reports
- Select the product category for content generation
- Validate uploaded input files

STAGE 2: DATA EXTRACTION
- Extract text from uploaded HTML reports
- Convert reports into plain text for AI processing
- Prepare structured input for the LLM

STAGE 3: AI CONTENT GENERATION
- Analyze trend and competitor insights using Mistral (mistral-large-latest)
- Generate a structured content strategy
- Produce marketing copy, messaging, and recommendations

STAGE 4: CONTENT PARSING
- Parse the AI-generated JSON response
- Validate output structure
- Handle formatting or parsing errors

STAGE 5: HTML REPORT GENERATION
- Organize generated content into readable sections
- Apply consistent formatting and branding

Enhancements Implemented for highly personalized content: 

Brand Voice Optimization
- Redesigned the AI system prompt to reflect Wayfair's marketing voice
- Added tone and style guidelines for warm, conversational copy
- Included Wayfair copy examples to improve content quality

Enhanced User Inputs
- Added Target Audience input
- Added Seasonal Context input
- Added Content Focus selection
  
## Dashboard Builder Agent

STAGE 1: INPUT COLLECTION
- Select the product category
- Upload the Consumer Trend Discovery and Competitor Monitoring HTML reports
- Validate user inputs and start the dashboard generation workflow

STAGE 2: DASHBOARD TEMPLATE
- Prepare the dashboard layout for data population

STAGE 3: REPORT EXTRACTION
- Extract the uploaded HTML reports
- Identify and separate each report automatically
- Prepare the report contents for parsing

STAGE 4: DATA PARSING
- Parse both reports using JavaScript pattern matching
- Extract key business metrics, market trends, pricing, suppliers, segments, and recommendations
- Convert extracted information into structured datasets

STAGE 5: DASHBOARD GENERATION
- Merge trend and competitor insights into a unified dashboard
- Replace template placeholders with real business data
- Generate tables, summaries, and recommendation sections
- Build the final interactive HTML dashboard

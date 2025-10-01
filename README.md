# Automated Blog-to-Landing-Page Workflows (n8n)

This repository contains a collection of **n8n workflows** that automate the process of researching blog content, enhancing it with AI, generating images, and publishing a landing page. The goal is to showcase how multiple services can be orchestrated in n8n to create an end-to-end automated content pipeline.

All workflows are **self-hosted**:
- **n8n** runs in a Docker container thru Docker compose.
- **PostgreSQL** is self-hosted in Docker as the main database.
- **ngrok** is used to expose the local n8n instance for secure external API access and trigger webhooks.
- 
---

## 🚀 Workflows Overview

### **Workflow 1: Blog Research**
- **Tools Used**: [SerpAPI](https://serpapi.com/), [Google Search API], [Gemini Chat Model], PostgreSQL
- **Steps**:
  1. Search the web for blog content via Google using SerpAPI.
  2. Use the Gemini chat model to analyze and extract the search results
  3. Create a table in PostgreSQL for:
     - Headlines (Array)
     - Main content  
     - Structure (JSONB)  
     - Enhanced output  
     - Featured image (Array)
     - Generated HTML 

---

### **Workflow 2: Content Enhancement**
- **Tools Used**: [Gemini Chat Model], PostgreSQL
- **Steps**:
  1. Take the extracted content from Workflow 1.
  2. Use Gemini to further expand and improve the content.
  3. Store the enhanced output in PostgreSQL.

---

### **Workflow 3: Image Generation**
- **Tools Used**: [Stable Horde](https://stablehorde.net/) (free image generation), Supabase, PostgreSQL
- **Steps**:
  1. Generate **three images** (1 featured + 2 supporting) using prompts derived from Workflow 1’s output.  
  2. Upload images to Supabase for hosting with stable URLs.  
  3. Store the image URLs in PostgreSQL.  

---

### **Workflow 4: Landing Page Generation**
- **Tools Used**: [Gemini Chat Model]
- **Steps**:
  1. Generate an HTML landing page with inline CSS and JS.
  2. Use Workflow 2 (enhanced content) and Workflow 3 (image URLs) as part of the AI prompt.
  3. Store the generated landing page in PostgreSQL.

---

### **Workflow 5: Delivery**
- **Tools Used**: [SMTP Email Node]
- **Steps**:
  1. Convert the generated HTML into an `.html` file.  
  2. Send it to the user via email with SMTP.  

---

## 🛠️ Tools & Services Used
- **n8n (Docker)** → Workflow automation engine, self-hosted  
- **PostgreSQL (Docker)** → Central self-hosted database for structured data  
- **ngrok** → Exposes the local n8n instance securely for API access  
- **SerpAPI** → Google Search API for blog research  
- **Gemini Chat Model** → AI-based content extraction & enhancement  
- **Stable Horde** → Free image generation (text-to-image)  
- **Supabase** → File hosting & image storage with stable URLs  
- **SMTP** → Email delivery of generated HTML pages  

---

## ⚠️ Limitations
- **AI Weakness**: Currently using Gemini as a generic chat model, not specialized AI models for summarization, content generation, or web extraction.  
- **Image Generation**: Stable Horde is free but produces lower-quality images compared to commercial alternatives (e.g., MidJourney, DALL·E).  
- **Styling**: Generated landing pages are simple HTML/CSS with limited design sophistication.
- **Infrastructure**: Self-hosted setup (Docker + ngrok + PostgreSQL) is lightweight but not production-ready or scalable.  
- **Scalability**: Workflows are experimental and may not handle production-level loads.  

---

## 📸 Workflow Previews
Below is a screenshot of one of the workflows in n8n:  

![Workflow Screenshot](/Screenshots/workflow_screenshot.png)  

---

## 🌐 Example Output
Here’s an example of the generated HTML landing page:  

Github Hosted: https://bubblipathic.github.io/Blog-to-Landing-Page-Using-n8n/landing-page

![Generated Landing Page](/Screenshots/html_1.png)   
![Generated Landing Page](/Screenshots/html_2.png)  
![Generated Landing Page](/Screenshots/html_3.png)  

---

## 📩 How to Use
1. Clone this repository.  
2. Import the JSON workflow(s) into your **self-hosted n8n instance** (`Workflows → Import from File`).  
3. Run your stack with Docker Compose (n8n + PostgreSQL).  
4. Configure ngrok to expose your n8n instance to the web.  
5. Add your credentials (SerpAPI key, Supabase, PostgreSQL, SMTP).  
6. Run the workflows in order.  

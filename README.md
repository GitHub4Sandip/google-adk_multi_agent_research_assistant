# Research Assistant Multi-Agent System

A sophisticated multi-agent research assistant built with the Google ADK (Agent Development Kit) that orchestrates specialized agents to conduct comprehensive research on any topic and generate detailed reports.

## Overview

This project demonstrates a **controller-subordinate agent architecture** where:
- A **root controller agent** orchestrates research tasks
- **Specialist agents** handle specific research domains (Wikipedia, arXiv, Google Search)
- A **writer agent** compiles findings into a structured report

The system leverages the `gemini-3.5-flash` model to coordinate research workflows and synthesize information from multiple sources into coherent, high-quality reports.

## Architecture

### Agent Components

1. **Wikipedia Researcher Agent** (`wikipedia_researcher`)
   - Searches Wikipedia for general background information and overviews
   - Handles disambiguation and missing pages gracefully
   - Provides foundational context for research topics

2. **arXiv Researcher Agent** (`arxiv_researcher`)
   - Searches academic papers from the arXiv repository
   - Returns top 2 most relevant papers sorted by relevance
   - Includes paper titles, summaries, and URLs

3. **Web Searcher Agent** (`web_searcher`)
   - Performs Google web searches for current and supplementary information
   - Captures up-to-date content beyond Wikipedia and academic papers
   - Bypasses multi-tool limits for comprehensive results

4. **Report Writer Agent** (`report_writer`)
   - Appends synthesized research content to output files
   - Creates topic-specific report files (e.g., `black_holes_report.txt`)
   - Handles file encoding and error management

5. **Controller Agent** (`root_agent`)
   - Orchestrates the entire research workflow
   - Calls all three specialist agents in parallel
   - Synthesizes information from all sources
   - Delegates report writing to the writer agent

### Workflow

```
User Query
    ↓
Controller Agent
    ├─→ Wikipedia Researcher (background info)
    ├─→ arXiv Researcher (academic papers)
    ├─→ Web Searcher (current information)
    └─→ Synthesize Results
        ↓
    Report Writer (save to file)
```

## Installation

### Prerequisites
- Python 3.13 or higher
- Google API Key (for Gemini models and Google Search)

### Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd research_assistant_multi_agent
   ```

2. **Create a virtual environment** (optional but recommended)
   ```bash
   python -m venv .venv
   .venv\Scripts\activate  # On Windows
   # source .venv/bin/activate  # On macOS/Linux
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   # or using uv (faster):
   uv sync
   ```

### Configuration

1. **Set up environment variables**
   - Create a `.env` file in the project root (already included but needs your API key):
   ```
   GOOGLE_GENAI_USE_ENTERPRISE=0
   GOOGLE_API_KEY=your_actual_api_key_here
   ```

2. **Get your Google API Key**
   - Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
   - Create a new API key
   - Replace the placeholder in `.env` with your actual key

## Usage

### Basic Research Query

```python
from agent import root_agent

# Define your research query
query = "black holes in astrophysics"

# Invoke the controller agent
response = root_agent.generate(query)

# The agent will:
# 1. Search Wikipedia for general information
# 2. Search arXiv for academic papers
# 3. Search Google for current information
# 4. Synthesize all findings
# 5. Save results to a report file (e.g., black_holes_report.txt)

print(response)
```

### Using Individual Agents

```python
from agent import wikipedia_agent, arxiv_agent, google_search_agent, writer_agent

# Research with Wikipedia
wiki_result = wikipedia_agent.generate("quantum computing")

# Research with arXiv
arxiv_result = arxiv_agent.generate("machine learning")

# Web search
web_result = google_search_agent.generate("AI trends 2025")

# Write to file
writer_result = writer_agent.generate(
    "Summary of research findings...", 
    filename="research_output.txt"
)
```

## Project Structure

```
research_assistant_multi_agent/
├── agent.py                    # Main agent definitions and configurations
├── pyproject.toml              # Project metadata and dependencies
├── .env                        # Environment variables (API keys)
├── .gitignore                  # Git ignore rules
├── .adk/                       # ADK session database
├── machine_learning_report.txt # Example output report
└── README.md                   # This file
```

## Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `google-adk` | >=2.9.2 | Agent Development Kit for building multi-agent systems |
| `arxiv` | >=4.0.1 | Python wrapper for arXiv API |
| `wikipedia` | >=1.4.0 | Wikipedia API wrapper |

See `pyproject.toml` for complete dependency specifications.

## Features

✅ **Multi-Source Research**
   - Combines Wikipedia, academic papers, and web search results

✅ **Specialized Agents**
   - Each agent is optimized for its domain

✅ **Error Handling**
   - Graceful handling of missing pages, disambiguation, and API errors

✅ **Report Generation**
   - Automatic synthesis and file output

✅ **Scalable Architecture**
   - Easy to add new specialist agents

✅ **Google ADK Integration**
   - Leverages modern agent orchestration patterns

## Example Output

The system generates reports like `machine_learning_report.txt` that contain:

- **Wikipedia Summary**: General overview and definitions
- **Academic Papers**: Latest research with links to papers on arXiv
- **Web Search Results**: Current trends and supplementary information
- **Synthesized Summary**: Cohesive narrative combining all sources

## Troubleshooting

### API Key Issues
- Ensure `GOOGLE_API_KEY` is correctly set in `.env`
- Check that the API key has permissions for Gemini and Google Search APIs
- Verify `.env` file is not committed to git (it's in `.gitignore`)

### Wikipedia Disambiguation
- If a query is ambiguous, the agent returns related options
- Try being more specific with your query

### Missing Academic Papers
- arXiv may not have papers on every topic
- The agent will return "No papers found" message

### Rate Limiting
- If you encounter rate limits, add delays between requests or upgrade your API plan

## Contributing

To extend this project:

1. **Add new specialist agents**:
   - Create new agent functions in `agent.py`
   - Define the LlmAgent with appropriate tools
   - Add to controller agent's tool list

2. **Add new tools**:
   - Define tool functions with clear docstrings
   - Handle exceptions gracefully
   - Integrate with appropriate agents

3. **Modify the workflow**:
   - Update the `controller_instruction` in `agent.py`
   - Adjust agent sequencing and synthesis logic

## License

This project is part of the Google ADK learning initiative.

## Resources

- [Google ADK Documentation](https://ai.google.dev/adk)
- [arXiv API Documentation](https://arxiv.org/help/api)
- [Wikipedia API Documentation](https://www.mediawiki.org/wiki/API)
- [Google Search API](https://developers.google.com/custom-search)

## Notes

- This system is designed for research and educational purposes
- Respect rate limits and terms of service for all APIs
- Ensure sensitive information is not included in reports
- The `.adk` directory is auto-generated and should not be manually edited

---

**Version**: 0.1.0  
**Python**: >=3.13  
**Last Updated**: 2025

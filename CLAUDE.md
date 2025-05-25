# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Run Commands

Build the application:
```bash
go build
```

Run the scraper (requires publication name):
```bash
./substackscraper -pub PUBLICATION_NAME -cookie COOKIE_VALUE
```

Run with additional options:
```bash
./substackscraper -pub PUBLICATION_NAME -cookie COOKIE_VALUE -output md -dest ./output -since 2024-01-01
```

## Code Architecture

This is a Go CLI tool that scrapes Substack publications and converts posts to markdown or HTML files.

### Core Components

- `main.go`: Entry point that delegates to the scraper package CLI function
- `scraper/scraper.go`: Main application logic including CLI parsing, HTTP client setup, and post processing
- `scraper/api.go`: Substack API data structures and URL building utilities

### Key Architecture Details

- **API Integration**: Uses undocumented Substack API v1 endpoints (`/api/v1/archive` and `/api/v1/posts/:slug`)
- **Authentication**: Requires `substack.sid` cookie for accessing premium content
- **Rate Limiting**: Built-in 1-second delays between API requests
- **Content Processing**: Uses `html-to-markdown` converter with custom rules for Substack-specific elements:
  - Image links: Extracts actual image URLs from Substack's CDN wrapper URLs
  - Internal links: Converts full Substack URLs to relative markdown links
- **Pagination**: Fetches posts in batches of 50 with offset-based pagination
- **Output Formats**: Supports both HTML and Markdown with frontmatter metadata

### Data Flow

1. Parse CLI arguments and validate required publication name
2. Fetch archive list with pagination until date threshold is reached
3. For each post, fetch full content via slug-based API call
4. Apply HTML-to-markdown conversion with custom rules
5. Write output files with appropriate metadata headers

The scraper handles both free and premium content when provided with valid authentication cookies.
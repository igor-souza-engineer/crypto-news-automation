# Crypto News Automation

Automated crypto news pipeline built with Node.js to fetch, process, filter, and distribute real-time market news from external APIs.

## Overview

Crypto News Automation is a backend-focused automation project designed to consume crypto news from an external REST API, process the incoming data, filter relevant stories, prevent duplicate distribution, and deliver structured news updates automatically.

The project focuses on practical API integration, asynchronous processing, reliability, error handling, retry strategies, and cloud deployment.

A key architectural goal is to keep the application independent from a single news provider, allowing the external API to be replaced without rewriting the core processing logic.

## Project Status

The project is being rebuilt around a provider-independent architecture.

The first technical milestone is selecting and validating a free crypto news API that provides reliable access to recent market news.

Implementation of the final news pipeline should only proceed after the selected provider has been tested and its rate limits, response structure, and usage restrictions are understood.

## Architecture

```text
Crypto News API
      ↓
Node.js
      ↓
Fetch & Validation
      ↓
Normalization
      ↓
Filtering
      ↓
Deduplication
      ↓
Formatting
      ↓
Distribution
```

The external API is treated as a data provider rather than being coupled directly to the rest of the application.

## Core Features

- External crypto news API integration
- Asynchronous news fetching
- JSON response processing
- Data normalization
- Topic and asset filtering
- Duplicate detection
- Automated news distribution
- API error handling
- Retry strategies
- Rate-limit awareness
- Structured logging
- Scheduled execution
- Cloud deployment

## Tech Stack

- Node.js
- JavaScript
- REST APIs
- JSON
- Git
- GitHub
- AWS Lightsail

Additional tools may be introduced only when justified by the final implementation.

## API Provider Strategy

The project is designed to avoid tight coupling with a specific news provider.

The selected API must provide:

- recent crypto news
- structured JSON responses
- stable REST endpoints
- sufficient free usage for a portfolio project
- clear rate limits
- usable source metadata
- article URLs
- publication timestamps

The provider should also allow the application to use article metadata in accordance with its terms of service.

## News Data Model

External API responses are normalized into an internal structure before being processed by the rest of the application.

Example:

```json
{
  "id": "article-id",
  "title": "Bitcoin moves after new market development",
  "summary": "Short provider-supported summary",
  "source": "News Source",
  "url": "https://example.com/article",
  "publishedAt": "2026-10-05T12:00:00Z",
  "assets": ["BTC"],
  "category": "Bitcoin"
}
```

This keeps filtering, formatting, and distribution logic independent from the original provider response.

## How It Works

1. The application requests recent news from the configured API.
2. The API response is validated before processing.
3. Articles are converted into the internal news model.
4. Filtering rules determine which stories are relevant.
5. Previously processed articles are removed through deduplication.
6. Selected stories are formatted for distribution.
7. New items are sent through the configured output channel.
8. Failures are logged and retried when appropriate.

## Filtering

The pipeline can filter news based on criteria such as:

- Bitcoin
- Ethereum
- DeFi
- Stablecoins
- Regulation
- ETFs
- Exchanges
- Macro
- specific assets
- publication time
- source

Filtering rules remain separate from the API integration layer.

## Deduplication

The system prevents the same article from being distributed repeatedly.

Possible identifiers include:

- provider article ID
- normalized URL
- title hash
- source and title combination

Example:

```text
Fetched Article
      ↓
Already Processed?
   ↙        ↘
 Yes        No
  ↓          ↓
Skip      Process
```

## Retry Strategy

Temporary failures should be retried without creating infinite request loops.

Example:

```text
API Request
    ↓
Request Failed
    ↓
Retry 1
    ↓
Wait
    ↓
Retry 2
    ↓
Wait
    ↓
Retry 3
    ↓
Fail Gracefully
```

Retries should be used for temporary failures such as:

- network errors
- timeouts
- HTTP 429
- HTTP 500
- HTTP 502
- HTTP 503

Authentication and invalid-request errors should not be retried indefinitely.

## Error Handling

The application handles failures such as:

- invalid API credentials
- network errors
- request timeouts
- malformed responses
- empty API responses
- provider downtime
- rate limiting
- distribution failures

Errors should include enough context to understand where and why a failure occurred.

## Logging

Important events can be recorded using structured logs.

Examples:

```text
NEWS_FETCH_SUCCESS
NEWS_FETCH_FAILURE
NEWS_PROCESSED
NEWS_SENT
NEWS_SKIPPED_DUPLICATE
RATE_LIMIT_REACHED
DELIVERY_FAILURE
```

Useful log information includes:

- timestamp
- event
- provider
- status
- message

## Automation

The pipeline is designed to execute automatically at controlled intervals.

Example:

```text
Scheduled Execution
        ↓
Fetch News
        ↓
Normalize
        ↓
Filter
        ↓
Deduplicate
        ↓
Distribute
```

The polling interval must respect the limits of the selected API provider.

## Running Locally

### Prerequisites

Make sure you have installed:

- Node.js
- npm
- Git

### Installation

```bash
git clone https://github.com/igor-souza-engineer/crypto-news-automation.git
cd crypto-news-automation
npm install
```

### Environment Variables

Create a local environment file:

```bash
cp .env.example .env
```

Configure the required values according to the selected news provider and distribution channel.

Example:

```env
NEWS_API_KEY=
NEWS_API_BASE_URL=
NEWS_FETCH_INTERVAL=
```

Additional variables may be required depending on the final distribution integration.

Do not commit real credentials to the repository.

### Start the Application

```bash
npm start
```

For development:

```bash
npm run dev
```

## Usage

Once configured, the application runs the automated news pipeline according to the defined schedule.

The typical flow is:

1. Start the Node.js service.
2. The service requests recent crypto news.
3. New articles are normalized and filtered.
4. Duplicate stories are ignored.
5. Relevant articles are formatted.
6. New stories are delivered through the configured output channel.
7. Processing results and failures are logged.

## Testing

Tests should focus on the parts of the system that contain meaningful application logic.

Examples include:

- API response normalization
- filtering
- deduplication
- formatting
- retry behavior
- error mapping

Run the test suite with:

```bash
npm test
```

## Deployment

The application is designed to run continuously on a cloud server such as AWS Lightsail.

A production environment should include:

- Node.js runtime
- environment-based configuration
- process restart strategy
- secure API credentials
- application logging
- controlled polling intervals
- network and firewall configuration

The service should recover gracefully from temporary provider or network failures.

## Reliability

The project applies production-oriented practices such as:

- explicit request timeouts
- bounded retries
- rate-limit awareness
- duplicate prevention
- structured error handling
- provider-independent data normalization
- persistent processing history when required
- safe recovery from temporary failures

## Security

Security considerations include:

- no API keys committed to GitHub
- `.env` excluded from version control
- `.env.example` containing only placeholder values
- credentials stored through environment variables
- external API responses validated before use
- no sensitive configuration included in logs

## Repository Structure

The final structure may evolve as the implementation progresses.

A possible structure is:

```text
crypto-news-automation/
├── src/
│   ├── api/
│   ├── services/
│   ├── filters/
│   ├── formatters/
│   ├── storage/
│   ├── distribution/
│   ├── utils/
│   └── index.js
├── tests/
├── docs/
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

Folders should only be introduced when the implementation actually requires them.

## Project Goals

This project is designed to demonstrate practical experience with:

- Node.js
- JavaScript
- REST API integration
- JSON processing
- asynchronous programming
- external API consumption
- data normalization
- filtering
- deduplication
- automation
- error handling
- retry strategies
- rate-limit management
- logging
- cloud deployment

## License

This project is intended for educational and portfolio purposes.

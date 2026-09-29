# n8n GitHub Discord Automation

An n8n automation workflow that fetches GitHub repositories, enriches them with recent commit data, categorizes them by star count, and sends a scheduled digest to Discord.

## Context

This project was developed as a take-home assessment for an Automation & QA Developer role.

## Workflow

Schedule Trigger  
→ GitHub Search  
→ Top 5  
→ Get Last Commit  
→ Star Threshold  
→ Tag Hot / Tag Rising  
→ Combine  
→ Build Digest  
→ Discord

## APIs Used

### GitHub REST API

The GitHub repository search API is used to find repositories related to the `artificial-intelligence` topic. Results are sorted by stars in descending order.

### GitHub Repository Commits API

The commits endpoint is used as a second API call to enrich each selected repository with its latest commit information.

## Transformation

The workflow keeps the top 5 repositories from the GitHub search results.

Each repository is then reshaped to keep relevant fields such as:

- Repository name
- Star count
- Repository URL
- Latest commit information

## Conditional Logic

Repositories are classified using a star-count threshold of `100000`.

- `HOT` → more than 100,000 stars
- `RISING` → 100,000 stars or fewer

The two branches are then combined before generating the final digest.

## Output

The final digest is generated as a single message and sent to a Discord channel using a Discord Webhook credential stored in n8n.

## Error Handling

The GitHub commit request is configured with an error branch so API failures do not fail silently.

When the API request fails, the error is passed to an `Error Handler` Code node, which creates a readable error message and sends it through the Discord notification path.

## Credentials

The Discord webhook is stored using n8n Credentials rather than hard-coded directly into the workflow.

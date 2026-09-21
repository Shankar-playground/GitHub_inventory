# Fetch GitHub Repository Details (Inventory)

This script generates a comprehensive inventory of all repositories in a GitHub organization with detailed metrics including PRs, issues, branches, releases, and commit information.

## Prerequisites

- Python 3.x
- Required packages: `requests`, `python-dotenv`

Install dependencies:
```bash
pip install requests python-dotenv
```

## Setup

Create a `.env` file with the following variables:
```
GH_PAT=your_github_personal_access_token
GH_ORG=your_organization_name
```

## Usage

Run the script:
```bash
python fetch_repos_details.py
```

The script will:
1. Fetch all repositories in the organization
2. Collect detailed metrics for each repository
3. Export comprehensive inventory to timestamped CSV file

## Features

- ✅ Complete repository inventory with 17+ metrics
- ✅ Pull request counts (open, closed, merged)
- ✅ Issue counts (open, closed)
- ✅ Branch and tag counts
- ✅ Release information
- ✅ Last commit details (date and user)
- ✅ Automatic rate limit handling
- ✅ Connection retry with exponential backoff
- ✅ Error logging to `error_log.txt`
- ✅ Progress reporting during execution

## Output

The script generates `{ORG_NAME}_repo_details_YYYYMMDD_HHMMSS.csv` with the following columns:

| Column | Description |
|--------|-------------|
| `Repo Name` | Repository name |
| `Visibility` | public, private, or internal |
| `Created At` | Repository creation date |
| `Updated At` | Last update timestamp |
| `Last Pushed Date` | Last push timestamp |
| `Repo Size (MB)` | Repository size in megabytes |
| `Primary Language` | Main programming language |
| `Total Open PRs` | Count of open pull requests |
| `Total Closed PRs` | Count of closed (not merged) pull requests |
| `Total Merged PRs` | Count of merged pull requests |
| `Total Open Issues` | Count of open issues |
| `Total Closed Issues` | Count of closed issues |
| `Total Branches` | Number of branches |
| `Total Releases` | Number of releases |
| `Total Tags` | Number of tags |
| `Last Committed Date` | Date of last commit on default branch |
| `Last Committed User` | User who made the last commit |

## Token Permissions

The GitHub Personal Access Token must have:
- `repo` scope - Full repository access
- `read:org` scope - Organization read access

## Important Notes

### Performance
- **This script is comprehensive and may take significant time** for large organizations
- Each repository requires multiple API calls (PRs, issues, branches, tags, commits, releases)
- Expected time: 30-60 seconds per repository
- For 100 repositories, expect 50-100 minutes

### PR Counts Accuracy
- Distinguishes between **closed** (rejected) and **merged** PRs
- Requires additional API call per closed PR to check merge status
- This ensures accurate reporting of PR outcomes

### Rate Limiting
- Automatic rate limit detection and handling
- Script pauses when rate limits are reached
- Includes exponential backoff for connection errors

### Error Handling
- Connection errors are automatically retried (up to 5 attempts)
- Errors logged to `error_log.txt` with timestamps
- Script continues processing remaining repositories on errors

## Example Output

Console output:
```
Processing frontend-app...
Processing backend-api...
Rate limit hit. Sleeping for 3600 seconds...
Processing mobile-app...
Processing data-service...
✅ Done. Output written to my-org_repo_details_20251119_103045.csv
```

## Sample CSV Output

```csv
Repo Name,Visibility,Created At,Updated At,Last Pushed Date,Repo Size (MB),Primary Language,Total Open PRs,Total Closed PRs,Total Merged PRs,Total Open Issues,Total Closed Issues,Total Branches,Total Releases,Total Tags,Last Committed Date,Last Committed User
frontend-app,public,2023-01-15T10:30:00Z,2025-11-18T14:20:00Z,2025-11-19T09:15:00Z,45.32,TypeScript,5,12,156,8,94,12,8,15,2025-11-19T09:15:23Z,john-doe
backend-api,private,2023-03-20T08:45:00Z,2025-11-19T11:30:00Z,2025-11-19T10:22:00Z,128.76,Python,3,8,203,15,142,8,12,24,2025-11-19T10:22:45Z,jane-smith
mobile-app,private,2023-06-10T12:00:00Z,2025-11-17T16:45:00Z,2025-11-18T14:30:00Z,89.54,Swift,2,5,87,4,56,6,5,18,2025-11-18T14:30:12Z,bob-developer
```

## Use Cases

- **Repository audit**: Complete inventory of all organization repositories
- **Activity analysis**: Identify active vs inactive repositories
- **Resource planning**: Track repository sizes and metrics
- **Compliance**: Document repository configurations and activity
- **Migration planning**: Assess repositories before migration
- **Team reporting**: Generate reports on development activity
- **Cleanup**: Identify candidates for archiving or deletion

## Troubleshooting

### Script Running Very Slowly
- Normal for large organizations with many repositories
- Each repo requires 5-10+ API calls
- Consider running during off-peak hours
- Check console for progress updates

### Rate Limit Errors
- Script automatically waits when limits are reached
- GitHub API limit: 5,000 requests/hour
- For very large orgs, may need multiple hours
- Script will resume automatically after wait period

### Connection Errors
- Script includes automatic retry logic
- Check `error_log.txt` for details
- Verify internet connection stability
- GitHub API status: https://www.githubstatus.com/

### Missing Data in CSV
- Check `error_log.txt` for specific repository errors
- Some repositories may have restricted access
- Private repos require appropriate token permissions

### Incomplete PR/Issue Counts
- Ensure token has full `repo` scope
- Some repos may have thousands of PRs/issues (takes longer)
- Check error log for timeout or connection issues

## Error Logging

All errors are logged to `error_log.txt`:
- Failed API calls with status codes
- Connection errors and retries
- Unexpected exceptions
- Repository-specific errors

Review this file if data appears incomplete.

## Performance Optimization Tips

For very large organizations:
1. Run during off-peak hours to avoid rate limits
2. Consider breaking into batches manually
3. Monitor `error_log.txt` during execution
4. Keep terminal/console window open
5. Ensure stable internet connection

## Data Analysis Tips

After exporting, you can analyze the CSV to:
- Identify inactive repositories (old last commit dates)
- Find repositories with high open PR/issue counts
- Compare repository sizes
- Track primary languages across organization
- Identify repositories without releases or tags
- Find repositories with few branches (potential for cleanup)

## Related Information

- Repository API: https://docs.github.com/en/rest/repos/repos
- Pull requests API: https://docs.github.com/en/rest/pulls/pulls
- Issues API: https://docs.github.com/en/rest/issues/issues
- Rate limiting: https://docs.github.com/en/rest/overview/resources-in-the-rest-api#rate-limiting

## Estimated Execution Time

Based on organization size:
- 10 repositories: 5-10 minutes
- 50 repositories: 25-50 minutes
- 100 repositories: 50-100 minutes
- 500 repositories: 4-8 hours

**Note**: Times vary based on repository activity (more PRs/issues = longer processing time).

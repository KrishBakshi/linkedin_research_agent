# LinkedIn Research Results

This directory stores JSON files containing LinkedIn profile data collected by the research agent.

## File Naming Convention

Files are automatically saved with timestamps:

```
linkedin_results_YYYY-MM-DD_HH-MM-SS.json
```

**Example:**
- `linkedin_results_2026-02-06_14-30-45.json` - Saved on Feb 6, 2026 at 2:30:45 PM

## JSON Structure

Each file contains:

```json
{
  "search_metadata": {
    "query": "Boolean search query used",
    "filters": {
      "location": "Japan",
      "connection_level": "2nd"
    },
    "search_date": "2026-02-06T14:30:45Z",
    "total_results": 25
  },
  "profiles": [
    {
      "profile_url": "https://www.linkedin.com/in/username/",
      "linkedin_username": "username",
      "name": "Full Name",
      "headline": "Job Title at Company",
      "location": "City, Country",
      "about": "Profile description...",
      "experiences": [...],
      "educations": [...],
      "skills": [...],
      "contacts": [...]
    }
  ]
}
```

## Usage

The agent automatically:
1. Creates this directory if it doesn't exist
2. Generates timestamped filename
3. Saves all collected profile data in JSON format
4. Reports the saved file path to the user

## Notes

- All JSON files in this directory are git-ignored (see `.gitignore`)
- The directory structure is preserved via `.gitkeep`
- Files are sorted chronologically by timestamp in filename

# Social Media Auto Poster

An n8n workflow that helps automate scheduled social media posting. It checks Airtable for pending posts, waits until each post is due, uses Gemini AI to polish the caption, and routes the content to Instagram, Facebook, or LinkedIn through Buffer's API.

The idea is simple: manage your post schedule in Airtable and let n8n handle the repetitive steps.

## What It Does

- Checks Airtable for records with a `Pending` status.
- Runs every 15 minutes and checks whether a post's scheduled date and time have arrived.
- Uses Gemini AI to rewrite the caption for the selected platform and add relevant hashtags.
- Routes the post according to the `Platform` field.
- Sends the post request to Buffer for Instagram, Facebook, or LinkedIn.
- Includes the Airtable image URL as a media asset for Instagram, Facebook, and LinkedIn posts.
- Sets the result to `Posted` or `Failed` and updates the Airtable record.

## Workflow Overview

```text
Schedule Trigger (every 15 minutes)
        ↓
Get Pending Posts (Airtable)
        ↓
Check If Due Now
        ↓
Generate Caption (Gemini AI)
        ↓
Extract AI Caption
        ↓
Route By Platform
   ┌────┼─────────┐
 Instagram      Facebook      LinkedIn
   └────┼─────────┘
        ↓
Check Post Success
        ↓
Mark as Posted / Mark as Failed
        ↓
Update Airtable Status
```

## Tools & Services

- **n8n** — workflow automation
- **Airtable** — stores post content, schedule, platform, and status
- **Google Gemini API** — rewrites captions
- **Buffer API** — receives publishing requests for connected social channels

## Airtable Fields

The workflow expects Airtable records with these fields:

| Field | Purpose |
|---|---|
| `Platform` | Target platform: `Instagram`, `Facebook`, or `LinkedIn` |
| `Date` | Scheduled posting date |
| `Time` | Scheduled posting time |
| `Caption` | Original caption to be improved by Gemini |
| `Image URL` | Public image URL for Instagram and Facebook posts |
| `Status` | Workflow filters for `Pending` and updates the result |
| `Error Details` | Available for recording error information |
| `Success` | Boolean result field |
| `Response Data` | Available for storing the API response |

Make sure the field names and formats match what the workflow expects.

## Setup

1. Download or clone this repository.
2. Open n8n and import `Social_Media_Auto_Poster.json`.
3. Create an Airtable base and table with the fields listed above.
4. In the Airtable nodes, replace the placeholder base and table IDs, then connect your Airtable credentials.
5. Add your Gemini API key to the Gemini HTTP Request node.
6. Configure Buffer HTTP Header Auth credentials and replace the placeholder Instagram, Facebook, and LinkedIn channel IDs with your own Buffer channel IDs.
7. Confirm your Buffer channels are connected and that the post formats and media meet each platform's requirements.
8. Test the workflow with a sample Airtable record before activating it.

## Example Airtable Record

| Platform | Date | Time | Caption | Image URL | Status |
|---|---|---|---|---|---|
| Instagram | 2026-12-01 | 10:30 | Launching our new project today! | https://example.com/image.jpg | Pending |

Use a valid, publicly accessible image URL for image-based posts. This is only an example; replace it with your own content and media.

## Important Notes

- The workflow is configured to check for due posts every 15 minutes, so publishing may not happen at the exact scheduled minute.
- The JSON contains placeholder API keys, Airtable IDs, Buffer channel IDs, and credential references. Add your own values in n8n; do not commit secrets to GitHub.
- The Buffer publishing requests are HTTP Request nodes and are marked in the workflow as placeholder calls. Verify the current Buffer API schema, channel IDs, and response format before relying on live posting.
- The Instagram, Facebook, and LinkedIn request bodies are configured to include an image asset from Airtable's `Image URL` field. Confirm that Buffer accepts the image asset format and LinkedIn metadata for your connected channel before relying on live posting.
- Test the success/failure condition and Airtable status updates with real API responses. The exported workflow should be treated as a starting point that needs credentials and integration testing.
- The workflow is exported as inactive. Activate it only after successful testing.

## Repository Contents

```text
.
├── Social_Media_Auto_Poster.json
└── README.md
```


# Trustpilot Data Extraction & Automation (n8n Workflow)

A robust n8n workflow designed to automatically extract company reviews from Trustpilot, process the data, store it in Google Sheets, and send email notifications. This is ideal for automated review monitoring and sentiment analysis.

## 📋 Overview

This workflow performs the following operations in sequence:
1.  **Scheduled Trigger**: Activates the workflow daily at 1:00 PM (13:00).
2.  **API Data Fetching**: Retrieves the latest company reviews from the Trustpilot API via RapidAPI.
3.  **Data Processing**: Formats the raw JSON API response into a clean, simplified structure.
4.  **Data Storage**: Appends the processed review data to a specified Google Sheet.
5.  **File Export**: Converts the dataset into a downloadable file (e.g., CSV).
6.  **Email Notification**: Sends a styled HTML email confirmation upon successful completion.

## ⚙️ Workflow Configuration

### Nodes Breakdown
| Node Name | Type | Purpose |
| :--- | :--- | :--- |
| `Daily Scheduler` | Schedule Trigger | Initiates the workflow daily at the set time. |
| `Fetch Trustpilot Reviews` | HTTP Request | Calls the Trustpilot API to fetch review data. |
| `Format Reviews Data` | Code | Processes the raw API data into a structured format. |
| `Save Reviews to Google Sheets` | Google Sheets | Writes the formatted data to a Google Sheet. |
| `Export Reviews File` | Convert to File | Creates a file from the data for download/backup. |
| `Email Notification` | Email Send | Sends a notification email upon successful run. |

### API & Service Configuration
-   **Trustpilot API (via RapidAPI)**
    -   **Endpoint**: `https://trustpilot-company-and-reviews-data.p.rapidapi.com/company-reviews`
    -   **Query Parameters**: Configured for `gossby.com`, any posting date, and `en-US` locale.
    -   **Authentication**: Requires `x-rapidapi-key` and `x-rapidapi-host` headers.
-   **Google Sheets Integration**
    -   **Document ID**: `1UrEy6MY7H-5UDcYSHdhlj4t8-y5MGSPlDKiUpKDbqfo`
    -   **Operation**: `Append` data to the existing sheet.
    -   **Columns**: `text`, `rating`, `date`, `consumer_name`
-   **Email Notification**
    -   **SMTP**: Configured with credentials for sending.
    -   **From**: `rupak.stat16@gmail.com`
    -   **To**: `rupak.aideveloper@gmail.com`
    -   **Subject**: `Trustpilot data has been extracted!`

## 🗂️ Extracted Data Structure

The workflow processes and stores the following review information:
| Column | Description | Type |
| :--- | :--- | :--- |
| `text` | The content of the review. | `string` |
| `rating` | The star rating given by the user. | `string` |
| `date` | The timestamp of when the review was posted. | `string` |
| `consumer_name` | The display name of the reviewer. | `string` |

## 🔐 Authentication & Credentials

This workflow requires the following credentials to be set up in n8n:
1.  **RapidAPI Account**: A valid API key for the Trustpilot endpoint.
2.  **Google Service Account**: OAuth 2.0 credentials for the Google Sheets node with write access to the target spreadsheet.
3.  **SMTP Credentials**: Login details for an SMTP email server (e.g., Gmail, SendGrid) to send notifications.

*Note: The credentials in the exported JSON are for reference only and must be replaced with your own valid credentials in n8n.*

## 🚀 Installation & Usage

### Prerequisites
-   An [n8n](https://n8n.io/) instance (cloud, self-hosted, or desktop app).
-   The required service accounts and API keys mentioned above.

### Setup Steps
1.  **Import the Workflow**:
    -   Copy the JSON from `dataextractfrom_trustpilot.json`.
    -   In your n8n instance, go to the workflows page.
    -   Click the dropdown menu next to the "+ New Workflow" button and select "Import from JSON".
    -   Paste the JSON content and click "Import".

2.  **Configure Credentials**:
    -   For each node with a credential warning (Google Sheets, Email, HTTP Request), click on the node and set up the required credentials using your own accounts.

3.  **Customize Parameters (Optional)**:
    -   Change the `company_domain` in the "Fetch Trustpilot Reviews" node to monitor a different company.
    -   Update the Google Sheets `documentId` to point to your own spreadsheet.
    -   Modify the email recipients and content in the "Email Notification" node.

4.  **Activate the Workflow**:
    -   Toggle the workflow from `Inactive` to `Active`.
    -   The workflow will now run automatically based on its schedule (daily at 1:00 PM).

## 🔧 Customization

-   **Schedule**: Adjust the `interval` in the "Daily Scheduler" node to change the trigger time or frequency.
-   **Data Points**: Modify the Code node (`Format Reviews Data`) to extract different or additional fields from the Trustpilot API response.
-   **Destination**: The workflow can be easily extended to save data to other destinations like a database (PostgreSQL, MySQL), another SaaS app, or a webhook.

## 📄 File Contents

-   `dataextractfrom_trustpilot.json`: The exported n8n workflow JSON file.

## 📝 Notes

-   The workflow is currently in an `"active": false` state upon import. Remember to activate it.
-   Ensure your n8n instance has internet access to call the Trustpilot API and Google Sheets.
-   The Google Sheets node uses `autoMapInputData`, so the column names in the sheet must match the JSON properties (`text`, `rating`, etc.) for automatic mapping to work. The included JSON defines the schema to help with this.

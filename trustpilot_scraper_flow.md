# Trustpilot Review Scraper Flow

``` mermaid
flowchart TD
    A[Schedule Trigger] --> B[HTTP Request - Fetch Trustpilot Reviews]
    B --> C[Code Node - Extract & Transform Data]
    C --> D[Append Row - Google Sheet]
    D --> E[Convert to File - Export Sheet as CSV/XLSX]
    E --> F[Send Email - Notify with File Attached]
```

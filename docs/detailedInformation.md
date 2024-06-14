### Flow

The DependAssist script automates the process of creating and managing JIRA tickets based on Dependabot alerts from GitHub repositories. This section provides a detailed description of the script flow for different test cases.

#### 1. Load Configuration

The script begins by loading the configuration file (`my.json`) and the `.env` file for credentials. The `.env` file should contain your JIRA API key, JIRA username, and GitHub token.

#### 2. Fetch Repositories

The script reads the list of repositories to be processed from the `repos.txt` file.

#### 3. Fetch Dependabot Alerts

For each repository listed in repos.txt, the script fetches open Dependabot alerts from the GitHub API. This step involves retrieving information about potential vulnerabilities in the dependencies used by the repository. The cutoff_days parameter is used to filter these alerts based on their creation date, ensuring that only recent alerts are considered. The cutoff_days parameter in the my.json configuration file is used to filter alerts. This parameter specifies the number of days to look back from the current date to consider alerts as recent. Only alerts created within this time frame are processed.

#### 4. Check for Existing JIRA Tickets

The script checks JIRA for existing tickets to avoid creating duplicate tickets for the same alert. This is done using a JIRA Query Language (JQL) query that searches for tickets with a summary that matches the alert's summary and also checks the description for the specific Dependabot alert link. This ensures that even if multiple alerts exist for the same package, duplicate tickets are not created.

The script parses the response from the JIRA API to check if any tickets match the alert's summary. If a matching ticket is found, it further checks the description of each issue to see if it contains the specific Dependabot alert link. By checking the description for the alert link, the script ensures that even if multiple alerts exist for the same package, a new ticket is created only if the alert link is not already present in an existing ticket's description.


 - **Summary Example**:
     - If the summary of the alert is "Vulnerable Library - repo1 - package1", the JQL query will search for tickets in the "SECURITY" project with statuses "In Progress", "Open", or "Pending Review" and with summaries containing "Vulnerable Library - repo1 - package1". Additionally, the script will check the description of these tickets for the specific alert link.


#### 5. Create or Update JIRA Tickets

If no existing ticket is found, the script creates a new JIRA ticket with the relevant details. It formats the description using JIRA's Wiki Markup.

#### 6. Process Tickets

The script processes tickets based on their type and the configuration settings.

- **Development Dependencies**: 
    - Handle the creation of JIRA tickets for development dependencies.
    - Add a comment indicating that the vulnerable code is not used in production.
    - Move the ticket through the workflow states specified for development dependencies.
    - Optionally dismiss the alert in GitHub.
- **Risk Accepted**: 
    - Handle the transition for risk-accepted tickets.
    - Add a comment indicating that no patch is available.
    - Move the ticket through the workflow states specified for risk-accepted dependencies.
    - Optionally link the ticket to an existing risk-acceptance ticket.
    - Optionally dismiss the alert in GitHub.
- **Normal Workflow**: 
    - Move the ticket through the workflow states specified for runtime dependencies.

### Key Features

DependAssist offers several key features to streamline the management of Dependabot alerts and JIRA tickets.

#### Automatic Team Assignment

The script can automatically assign the appropriate team to each JIRA ticket based on the repository. This is done using a `team_mapping.json` file that maps repositories to JIRA team IDs.

**Example `team_mapping.json`:**
```json
{
  "repo1": "10050",
  "repo2": ["10051", "10052"],
  "repo3": "10053",
  "repo4": ["10054", "10055"]
}
```

#### Custom Fields

The script supports custom fields in JIRA tickets. Custom field IDs and their values are defined in the `my.json` configuration file. The script dynamically determines if a field allows multiple values.

**Example Custom Fields Configuration:**
```json
"custom_fields": {
  "customfield_10036": "10029",
  "customfield_10037": ["10030", "10031"]
}
```

#### Automatic Severity Assignment

If `auto_severity` is enabled, the script calculates the severity of each issue based on CVSS score, EPSS score, and KEV status. The appropriate severity level is then assigned to the custom severity field. It is recommended to have four categories for severity otherwise script will fail. For more details on severity calculations please check [Severity Calculation](Severitycalculation.md) 

**Example Severity Fields Configuration:**
```json
"severity_fields": {
  "customfield_10036": {
    "values": {
      "P4": "10029",
      "P3": "10028",
      "P2": "10027",
      "P1": "10026"
    }
  }
}
```

#### Handling Development Dependencies

The script has special handling for development dependencies, including creating tickets and adding comments to indicate that the vulnerable code is not used in production.


#### Dismissing Alerts

The script can automatically dismiss Dependabot alerts in GitHub if configured to do so. This feature helps manage and reduce noise from alerts that do not require immediate attention.

- **Configuration:**
    - **dismiss_no_patch**: Set to `true` to automatically dismiss alerts with no available patches.

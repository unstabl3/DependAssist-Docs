## Frequently Asked Questions (FAQ)

### 1. What is DependAssist?

**DependAssist** is a script designed to automate the creation and management of JIRA tickets based on Dependabot alerts from GitHub repositories. It helps streamline the process of handling vulnerabilities in dependencies by creating JIRA tickets and processing them according to predefined workflows.

### 2. How do I install DependAssist?

You can install DependAssist by cloning the repository from GitHub and setting up the necessary dependencies. For detailed installation instructions, refer to the [Installation Guide](Installation.md).

### 3. How do I configure DependAssist?

DependAssist uses a configuration file (`my.json`) to define various settings and behaviors. You need to provide values for keys such as GitHub organization name, JIRA server URL, project key, issue types, workflow states, custom fields, and more. Refer to the [Advanced Configuration](Advanced_configuration.md) for detailed descriptions of each configuration key.

### 4. How do I store sensitive tokens for DependAssist?

You can store sensitive tokens (JIRA API key, JIRA username, and GitHub token) in a `.env` file or directly in environment variables. For detailed instructions on storing credentials, refer to the [Sensitive Tokens](Sensitive_tokens.md) guide.

### 5. What are cutoff_days, and how should I set it?

The `cutoff_days` parameter specifies the number of days to look back for Dependabot alerts. It filters alerts based on their creation date, ensuring that only recent alerts are considered. Set this value based on your organization's security policies and the frequency of dependency updates. A typical value might be between 3 and 5 days. If you are running this first time and want to cover all older alerts then give 365 or higher days.

### 6. How does DependAssist check for existing JIRA tickets?

DependAssist uses a JQL query to search for existing JIRA tickets with summaries matching the alert's summary. It also checks the description of the tickets for the specific Dependabot alert link to avoid creating duplicates. Refer to the [Flow](detailedInformation.md) for detailed steps.

### 7. How does automatic team assignment work?

DependAssist can automatically assign the appropriate team to each JIRA ticket based on the repository. This is done using a `team_mapping.json` file that maps repositories to JIRA team IDs. For details on setting this up, refer to the [Advanced Configuration](Advanced_configuration.md).

### 8. What is the purpose of custom fields in JIRA tickets?

Custom fields allow you to include additional information in JIRA tickets that may be required by your organization. DependAssist supports custom fields and allows you to define them in the `my.json` configuration file. Refer to the [Advanced Configuration](Advanced_configuration.md) for more information.

### 9. How does automatic severity assignment work?

If `auto_severity` is enabled, DependAssist calculates the severity of each issue based on CVSS score, EPSS score, and KEV status. The appropriate severity level is then assigned to the custom severity field. For more details, refer to the [Severity-calculation](Severitycalculation.md).

### 10. How does DependAssist handle development dependencies?

DependAssist has special handling for development dependencies, including creating tickets and adding comments to indicate that the vulnerable code is not used in production. It also moves the ticket through the specified workflow states for development dependencies.

### 11. Can DependAssist dismiss alerts in GitHub?

Yes, DependAssist can automatically dismiss Dependabot alerts in GitHub if configured to do so. This helps manage and reduce noise from alerts that do not require immediate attention. Set the `dismiss_no_patch` key in the `my.json` file to `true` to enable this feature.

### 12. What should I do if I encounter issues with DependAssist?

If you encounter any issues, ensure that all required fields in the `my.json` file are correctly configured and that the `.env` file contains valid credentials. For further assistance, open an issue on the [GitHub repository](https://github.com/unstabl3/DependAssist/issues).

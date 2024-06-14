This script requires working JIRA and GitHub API keys to function properly.

#### Creating API Keys

Before storing credentials, make sure you have created the necessary API keys with the minimum required access.

- [Creating JIRA API Key](Creating_JIRA_API_Key.md)
- [Creating GitHub API Key](Creating_GitHub_API_Key.md)

#### Storing the Credentials

The script uses the `dotenv` Python library to fetch credentials stored in a `.env` file. Alternatively, you can store these credentials directly in environment variables.

##### Using a .env File

1. Create a `.env` file in the `DependAssist` directory.
2. Save the following content inside the file with your valid credentials:

```plaintext
JIRA_APIKEY=your_jira_api_key_here
JIRA_USERNAME=your_jira_username_here
GITHUB_TOKEN=your_github_token_here
```

##### Saving Credentials in Environment Variables

You can also set the credentials directly in your environment variables. Here’s how you can do it:

###### On Linux/macOS

Add the following lines to your `.bashrc`, `.zshrc`, or equivalent shell configuration file:

```bash
export JIRA_APIKEY=your_jira_api_key_here
export JIRA_USERNAME=your_jira_username_here
export GITHUB_TOKEN=your_github_token_here
```

After adding the lines, reload the shell configuration:

```bash
source ~/.bashrc   # or source ~/.zshrc
```

###### On Windows

Use the following commands in Command Prompt to set environment variables:

```cmd
setx JIRA_APIKEY "your_jira_api_key_here"
setx JIRA_USERNAME "your_jira_username_here"
setx GITHUB_TOKEN "your_github_token_here"
```

You will need to restart your Command Prompt or computer for the changes to take effect.

By following these steps, you can securely store and access your credentials, ensuring that the `DependAssist` script can authenticate with JIRA and GitHub APIs.

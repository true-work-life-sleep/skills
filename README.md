# Prepare install skill requirements

```
npx skills@latest add mattpocock/skills
npx skills add affaan-m/ECC --skill jira-integration
```


# Set env in your local machine

[Create API token](https://id.atlassian.com/manage-profile/security/api-tokens) jira & confluence</br>

Linux/Mac
```
$ echo 'export JIRA_URL="<email>"'  >> ~/.zshrc
$ echo 'export JIRA_EMAIL="<email>"'  >> ~/.zshrc
$ echo 'export JIRA_API_TOKEN="<token>"'  >> ~/.zshrc
$ source ~/.zshrc
```</br>

Windows
```
$ setx JIRA_URL "<url>"
$ setx JIRA_EMAIL "<email>"
$ setx JIRA_API_TOKEN "<token>"
```</br></br>

# How to use slash command
```
$ cd /path/to/project
$ open kiro-cil, claude, gemini, codex, opencode, etc
$ /dev-fast <url jira or url confluence>
```
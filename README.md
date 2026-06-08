# Prepare install skill requirements

```
npx skills@latest add mattpocock/skills
npx skills add affaan-m/ECC --skill jira-integration
```


# Set env in your local machine

[Create API token](https://id.atlassian.com/manage-profile/security/api-tokens) jira & confluence</br>
```
$ echo 'export JIRA_URL="<email>"'  >> ~/.zshrc
$ echo 'export JIRA_EMAIL="<email>"'  >> ~/.zshrc
$ echo 'export JIRA_API_TOKEN="<token>"'  >> ~/.zshrc
$ source ~/.zshrc
```
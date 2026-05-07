---
name: download-github-issues
description: Downloads the full GitHub Projects backlog and writes it to project_mgmt/github-issues.json
disable-model-invocation: true
---

# Download GitHub Issues

Fetch the full GitHub Projects backlog and write it to `project_mgmt/github-issues.json`.

## Steps

### Step 1: Create output directory if needed

```bash
mkdir -p project_mgmt
```

### Step 2: Detect org and project number

```bash
ORG=$(gh repo view --json owner -q .owner.login)
```

If `$ARGUMENTS` was provided, use it as the project number. Otherwise ask the user: "What is your GitHub Projects project number?" (visible in the project URL: `github.com/orgs/ORG/projects/NUMBER`).

```bash
PROJECT_NUMBER=$ARGUMENTS  # or the number provided by the user
```

### Step 3: Fetch the project board via GraphQL

```bash
gh api graphql -F org="$ORG" -F projectNumber="$PROJECT_NUMBER" -f query='
query($org: String!, $projectNumber: Int!) {
  organization(login: $org) {
    projectV2(number: $projectNumber) {
      title
      items(first: 100) {
        nodes {
          id
          status: fieldValueByName(name: "Status") {
            ... on ProjectV2ItemFieldSingleSelectValue { name }
          }
          content {
            ... on Issue {
              number
              title
              body
              url
              state
              labels(first: 20) { nodes { name } }
              assignees(first: 10) { nodes { login } }
              comments(first: 100) {
                nodes {
                  author { login }
                  body
                  createdAt
                }
              }
            }
          }
        }
      }
    }
  }
}
' > project_mgmt/github-issues.json
```

### Step 4: Confirm the download

```bash
python3 -c "import json; nodes = json.load(open('project_mgmt/github-issues.json'))['data']['organization']['projectV2']['items']['nodes']; print(f'Saved {len(nodes)} issues to project_mgmt/github-issues.json')"
```

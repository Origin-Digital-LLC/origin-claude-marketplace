---
name: download-github-issues
description: Downloads the full GitHub Projects backlog for Nimble Insurance and writes it to project_mgmt/github-issues.json
---

# Download GitHub Issues

Fetch the full GitHub Projects backlog and write it to `project_mgmt/github-issues.json`.

## Steps

### Step 1: Create output directory if needed

```bash
mkdir -p project_mgmt
```

### Step 2: Fetch the project board via GraphQL

```bash
gh api graphql -f query='
query {
  organization(login: "Nimble-Insurance") {
    projectV2(number: 1) {
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

### Step 3: Confirm the download

```bash
python3 -c "import json; nodes = json.load(open('project_mgmt/github-issues.json'))['data']['organization']['projectV2']['items']['nodes']; print(f'Saved {len(nodes)} issues to project_mgmt/github-issues.json')"
```

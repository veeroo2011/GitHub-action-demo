# GitHub-action-demo
first repo for GitHub action for learning purposes 

How to generate and upload report after unit test pass
To generate report, we need to add one more dependency pytest cov or pytest cove

below snap of workflow of CodeQL scan tools
  codeql:
    name: CodeQL security Scan # use to scan the repo for secuirity vulnerabilities
    runs-on: ubuntu-latest
    permissions:
      actions: read
      contents: read
      security-events: write # Required to upload the results of the codeql scan analysis results
    strategy: # Required to run the codeql analysis
      matrix:
        language: ['python']
        
Q. Why Permissions Are NeededBy default, 
   1. GitHub Actions jobs run with a broad set of permissions that could allow a compromised workflow to alter your repository. 
   2. To enforce security, GitHub recommends setting explicit, restricted access—known as the Principle of Least Privilege.This specific configuration blocks all unmentioned actions and grants only the exact access required for a security scanner to read your code and post vulnerabilities.

Q. To Whom You Are Giving Permission?
You are giving these permissions to the GITHUB_TOKEN, which is a temporary security credential generated automatically by GitHub for this specific job execution.

The actions and steps running inside this codeql job (such as the official GitHub CodeQL action blocks) will use this token to authenticate back into your repository.

Detailed Breakdown of Each PermissionHere is exactly why the GITHUB_TOKEN needs each permission to complete the scan:
contents: read
Why: The CodeQL engine cannot scan code it cannot see. This allows the runner to download (git clone) your repository files into the Ubuntu environment.

security-events: write
Why: After scanning, CodeQL generates a file containing vulnerabilities. It needs "write" access to upload this data back to GitHub so it can display alerts under your repository's Security tab.

actions: read
Why: CodeQL often references previous workflow runs or optimization caches to speed up analysis. This allows it to look up that automation data safely without changing anything.

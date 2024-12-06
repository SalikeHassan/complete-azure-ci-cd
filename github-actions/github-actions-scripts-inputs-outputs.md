<img width="955" alt="image" src="https://github.com/user-attachments/assets/5b5322c3-8feb-48a4-b91f-e236c9e56fa8">

```yml
name: Script and I/O Example

on:
  workflow_dispatch: # Allows manual triggering of the workflow
    inputs:
      issue_title:
        description: 'Title of the GitHub issue to create'
        required: true
      issue_body:
        description: 'Body of the GitHub issue'
        required: false

jobs:
  run-scripts:
    runs-on: ubuntu-latest # Uses a Linux runner

    steps:
      # Step 1: Inline Bash Script
      - name: Inline Script Example
        run: |
          echo "This is an inline script example"
          echo "The current directory is: $(pwd)"

      # Step 2: External Script Execution
      - name: Run External Script
        run: ./scripts/sample_script.sh # Runs a script stored in the repository
        shell: bash # Specifies the shell type

      # Step 3: Using github-script to Interact with GitHub API
      - name: Create GitHub Issue
        id: create_issue # Assigns an ID to reference outputs
        uses: actions/github-script@v6 # Executes custom JavaScript using GitHub API
        with:
          script: |
            // Log workflow context for debugging
            console.log(context);
            // Create an issue in the repository
            const issue = await github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: inputs.issue_title, // Takes input value for the issue title
              body: inputs.issue_body || 'Default issue body' // Default value if input not provided
            });
            // Return issue URL as output
            return { result: issue.data.html_url };

      # Step 4: Using Outputs from Previous Step
      - name: Log Issue URL
        run: echo "Issue created: ${{ steps.create_issue.outputs.result }}"
```

<img width="805" alt="image" src="https://github.com/user-attachments/assets/be58b279-ea23-48d2-aeab-8927d12d1e29">

<img width="1047" alt="image" src="https://github.com/user-attachments/assets/a511837c-e559-4215-b5b4-3e7054d0e823">

<img width="1070" alt="image" src="https://github.com/user-attachments/assets/18fc1b7f-219f-44c5-80e4-0718eb0eccca">

```yaml
uses: actions/github-script@v6
with:
  script: |
    const issue = await github.rest.issues.create({
      owner: context.repo.owner,
      repo: context.repo.repo,
      title: 'New Issue',
    });
```

<img width="630" alt="image" src="https://github.com/user-attachments/assets/e561148d-0ba2-46ed-8958-0bc4f5b3449b">

<img width="1048" alt="image" src="https://github.com/user-attachments/assets/3e9f614c-d15c-4233-b1f8-062d32febf1c">

<img width="1100" alt="image" src="https://github.com/user-attachments/assets/a2d13946-6a04-4385-a189-ab7acf72fd4e">

<img width="958" alt="image" src="https://github.com/user-attachments/assets/2d8ac14b-f237-4472-b2d8-5816a2ff3b9e">

<img width="945" alt="image" src="https://github.com/user-attachments/assets/22d3eba6-1efe-444c-95c1-f646bf061007">






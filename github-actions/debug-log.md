<img width="1031" alt="image" src="https://github.com/user-attachments/assets/21e9a103-176e-4783-a71a-35b126c8aa45">

```yaml
name: Debugging Example Workflow

on:
  workflow_dispatch: # Allows manual trigger of the workflow

jobs:
  debug-job:
    runs-on: ubuntu-latest
    env:
      ACTIONS_RUNNER_DEBUG: ${{ secrets.ACTIONS_RUNNER_DEBUG }} # Secret for runner debugging
      ACTIONS_STEP_DEBUG: ${{ secrets.ACTIONS_STEP_DEBUG }}     # Secret for step debugging
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Run Debug Command
        run: echo "Running with advanced debugging enabled"
```

<img width="1053" alt="image" src="https://github.com/user-attachments/assets/5a0ec98b-c911-4219-baae-9501383a978f">

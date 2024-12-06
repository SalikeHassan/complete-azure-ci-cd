<img width="961" alt="image" src="https://github.com/user-attachments/assets/7aba5e3e-8556-4e70-a81c-afb0de1a73b1">

```yml
steps:
  - name: Show Event Details
    run: echo "Triggered by event: ${{ github.event_name }}"
  - name: Show Runner OS
    run: echo "Runner OS: ${{ runner.os }}"
```

<img width="1036" alt="image" src="https://github.com/user-attachments/assets/1dbb1475-5320-4533-9ab0-f6528797a2d5">

```yml
env:
  BUILD_CONFIG: release

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      NODE_VERSION: 16
    steps:
      - name: Install Node.js
        run: echo "Installing Node.js ${{ env.NODE_VERSION }}"
```

<img width="1078" alt="image" src="https://github.com/user-attachments/assets/47409724-eeb0-4705-9e94-58e0c7f9743a">

```yml
steps:
  - name: Conditional Step
    if: ${{ github.ref == 'refs/heads/main' }}
    run: echo "This runs only on the main branch"
```

<img width="1079" alt="image" src="https://github.com/user-attachments/assets/47034ed2-5d81-49e4-835f-86ef76d80e49">

### Example

```yaml
name: Advanced Workflow Example with Variables

on:
  workflow_dispatch:
    inputs:
      should_fail:
        description: 'Simulate failure (yes/no)'
        required: true

env: # Workflow-level environment variables
  GLOBAL_VAR: "This is a workflow-level variable"
  NODE_ENV: production

jobs:
  build:
    runs-on: ubuntu-latest
    env: # Job-level environment variables
      JOB_VAR: "This is a job-level variable"
      NODE_ENV: ${{ env.NODE_ENV }} # Accessing workflow-level variable

    steps:
      - name: Access Workflow and Job Variables
        env: # Step-level environment variables
          STEP_VAR: "This is a step-level variable"
        run: |
          echo "Workflow Variable: ${{ env.GLOBAL_VAR }}"
          echo "Job Variable: ${{ env.JOB_VAR }}"
          echo "Step Variable: $STEP_VAR"
          echo "Node.js environment: ${{ env.NODE_ENV }}"

      - name: Conditional Step
        if: ${{ github.event.inputs.should_fail == 'yes' }}
        env: # Step-level variable for this specific step
          FAILURE_MESSAGE: "Simulating failure as per input"
        run: |
          echo "$FAILURE_MESSAGE"
          exit 1

      - name: Check Failure
        if: ${{ failure() }}
        run: echo "A step failed. Handling failure."

      - name: Success Step
        if: ${{ success() }}
        run: echo "All steps succeeded"

      - name: Always Run
        if: ${{ always() }}
        run: echo "Cleanup regardless of success or failure"

  deploy:
    runs-on: ubuntu-latest
    needs: build
    if: ${{ always() }}
    env: # Job-level environment variables for deploy job
      DEPLOY_ENV: "production"
    steps:
      - name: Access Deploy Job Variable
        run: echo "Deploy environment: ${{ env.DEPLOY_ENV }}"
      - name: Deployment
        run: echo "Deploying after cleanup"
```

<img width="819" alt="image" src="https://github.com/user-attachments/assets/a9aeb83f-a94b-41b8-ace6-6669f7b01331">

<img width="1031" alt="image" src="https://github.com/user-attachments/assets/343dd21a-f9b8-4df4-bde1-ca7e6a2bfc55">

<img width="1024" alt="image" src="https://github.com/user-attachments/assets/03476518-8bde-4a50-88dc-aec11d6ebca5">

<img width="956" alt="image" src="https://github.com/user-attachments/assets/a4b60dea-0a9e-47aa-aeee-742f102551fb">

<img width="981" alt="image" src="https://github.com/user-attachments/assets/37805fa4-bd0c-438e-bfe7-114618905da7">

<img width="996" alt="image" src="https://github.com/user-attachments/assets/bf27d25e-9e1c-4cb5-8975-dcb0701bd43b">

<img width="1014" alt="image" src="https://github.com/user-attachments/assets/b3020251-f9bc-4f2f-b612-a60e877b1543">

<img width="1036" alt="image" src="https://github.com/user-attachments/assets/8e2a69b2-7e64-4fc6-b3fa-54653e0c04ee">






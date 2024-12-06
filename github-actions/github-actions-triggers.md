<img width="960" alt="image" src="https://github.com/user-attachments/assets/4e924cb5-54ce-43e8-8dee-3fd5c9a168fc">

```yaml
Trigger on Pull Requests
name: Pull Request Validation

on:
  pull_request:
    branches:
      - main
    types:
      - opened
      - closed

jobs:
  validate_pr:
    runs-on: ubuntu-latest
    steps:
      - name: Run Tests
        run: echo "Testing pull request on branch ${{ github.ref }}"
```
<img width="1033" alt="image" src="https://github.com/user-attachments/assets/24b18dcc-269f-488b-8dc3-ffc8d21cff3b">

```yaml
name: Nightly Build

on:
  schedule:
    - cron: '0 2 * * *' # Every day at 2 AM

jobs:
  build_job:
    runs-on: ubuntu-latest
    steps:
      - name: Build Application
        run: echo "Building application at 2 AM"
```

#### Note: https://crontab.guru

<img width="829" alt="image" src="https://github.com/user-attachments/assets/831b2225-03d4-47d7-a7cf-99edde37e98c">

```yaml
name: Manual Deployment

on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment Environment'
        required: true
        default: 'production'

jobs:
  deploy_job:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        run: echo "Deploying to ${{ github.event.inputs.environment }} environment"
```
<img width="977" alt="image" src="https://github.com/user-attachments/assets/893d0053-c9fc-40e0-9218-cf81125c629b">

```yaml
name: External Trigger Workflow

on:
  repository_dispatch:
    types: [external_trigger]

jobs:
  process_data:
    runs-on: ubuntu-latest
    steps:
      - name: Process Payload
        run: echo "Received data: ${{ github.event.client_payload.data }}"
```

<img width="995" alt="image" src="https://github.com/user-attachments/assets/d529f68e-cc06-462c-bbc2-a818d6203100">

```json
{
  "event_type": "external_trigger",
  "client_payload": {
    "data": "Sample payload"
  }
}
```
<img width="943" alt="image" src="https://github.com/user-attachments/assets/b434c0f2-763f-4be5-93ec-44c9bc1fe0f7">














        

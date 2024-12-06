<img width="1036" alt="image" src="https://github.com/user-attachments/assets/65a894af-61a3-42dc-978a-94a5ec26255b">

```yaml
# Matrix for .NET on Multiple OS and Versions

name: .NET Matrix Example

on:
  workflow_dispatch: # Allows manual workflow trigger

jobs:
  build:
    runs-on: ${{ matrix.os }} # Dynamically set OS based on the matrix
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest] # Define OS options
        dotnet-version: [3.1, 5.0] # Define .NET versions
    steps:
      # Checkout code
      - name: Checkout Code
        uses: actions/checkout@v3

      # Set up .NET environment
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: ${{ matrix.dotnet-version }} # Use .NET version from the matrix

      # Build the application
      - name: Build Application
        run: dotnet build

# This workflow runs the .NET build job on Ubuntu and Windows with .NET 3.1 and 5.0, creating four jobs in total.
```

```yaml
# Matrix with continue-on-error

name: Matrix with Continue-on-Error

on:
  push:

jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]
        dotnet-version: [3.1, 5.0]
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: ${{ matrix.dotnet-version }}

      - name: Build Application
        run: dotnet build
        continue-on-error: ${{ matrix.dotnet-version == '3.1' }} # Ignore errors for .NET 3.1

# If the job running .NET 3.1 fails, it will not fail the entire workflow because of continue-on-error.
```

```yaml
#  Matrix with include and exclude

name: Matrix with Include and Exclude

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]
        dotnet-version: [3.1, 5.0]
        include: # Add a specific combination
          - os: ubuntu-latest
            dotnet-version: 6.0
        exclude: # Exclude a specific combination
          - os: ubuntu-latest
            dotnet-version: 3.1
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: ${{ matrix.dotnet-version }}

      - name: Build Application
        run: dotnet build

	#	Include: Adds an extra job for Ubuntu with .NET 6.0.
	#	Exclude: Removes the combination of Ubuntu with .NET 3.1.
```

<img width="1066" alt="image" src="https://github.com/user-attachments/assets/db4063ef-c83d-4a6d-8259-2ec3db0710c6">

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    dotnet-version: [3.1, 5.0]
    exclude:
      - os: ubuntu-latest
        dotnet-version: 3.1
```

<img width="1166" alt="image" src="https://github.com/user-attachments/assets/90866262-f3df-4283-81ba-52735306e575">

```yaml

strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    dotnet-version: [5.0]
    include:
      - os: ubuntu-latest
        dotnet-version: 6.0

key: ${{ matrix.os }}-${{ hashFiles('**/package.json') }}
```
<img width="1018" alt="image" src="https://github.com/user-attachments/assets/26270a50-f63f-4fee-ba2f-42d60d85c7b9">











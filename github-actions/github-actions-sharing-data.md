<img width="1037" alt="image" src="https://github.com/user-attachments/assets/4e64c8af-f8d1-4f23-9e55-6867ba977629">

```yaml
Using Output Variables

name: Output Variable Example

on:
  push:

jobs:
  generate-output:
    runs-on: ubuntu-latest
    outputs:
      url: ${{ steps.set-output.outputs.result }} # Define job-level output variable
    steps:
      - id: set-output # Step to set the output
        run: echo "::set-output name=result::https://example.com" # Setting output value

  use-output:
    runs-on: ubuntu-latest
    needs: generate-output # Depend on the job that generates the output
    steps:
      - name: Use Output
        run: echo "Accessed Output: ${{ needs.generate-output.outputs.url }}" # Access job output
```

```yaml
Artifact

name: Artifact Example

on:
  push:

jobs:
  create-artifact:
    runs-on: ubuntu-latest
    steps:
      - name: Generate Artifact File
        run: echo "This is an artifact file" > artifact.txt # Create a file

      - name: Upload Artifact
        uses: actions/upload-artifact@v3 # Upload the artifact
        with:
          name: example-artifact # Artifact name
          path: artifact.txt # File to upload

  use-artifact:
    runs-on: ubuntu-latest
    needs: create-artifact
    steps:
      - name: Download Artifact
        uses: actions/download-artifact@v3 # Download the artifact
        with:
          name: example-artifact # Name of the artifact to download

      - name: Read Artifact
        run: cat artifact.txt # Display artifact content
```

<img width="1045" alt="image" src="https://github.com/user-attachments/assets/44ef0319-c2c3-4de2-8a9a-ef6a08916392">


```yaml
Caching

name: Caching Example

on:
  push:

jobs:
  cache-example:
    runs-on: ubuntu-latest
    steps:
      - name: Restore Cache
        uses: actions/cache@v3
        with:
          path: node_modules # Directory to cache
          key: ${{ runner.os }}-node-modules-${{ hashFiles('**/package-lock.json') }} # Cache key
          restore-keys: |
            ${{ runner.os }}-node-modules- # Fallback key prefix

      - name: Install Dependencies
        run: npm install # Install dependencies

      - name: Save Cache
        if: ${{ always() }} # Ensure cache is saved even if other steps fail
        uses: actions/cache@v3
        with:
          path: node_modules
          key: ${{ runner.os }}-node-modules-${{ hashFiles('**/package-lock.json') }}
```

<img width="988" alt="image" src="https://github.com/user-attachments/assets/8c200cf4-9941-4d65-add6-580382532a26">

<img width="1055" alt="image" src="https://github.com/user-attachments/assets/fc882ec5-0597-41db-b7fb-32a1746a7814">

```yaml
- uses: actions/upload-artifact@v3
  with:
    name: my-artifact
    path: ./file.txt
```

<img width="985" alt="image" src="https://github.com/user-attachments/assets/599d1f0d-9740-4a1f-bf4d-e952599ec802">

<img width="1032" alt="image" src="https://github.com/user-attachments/assets/63b7670b-c28b-44b5-8bb2-39d38fbefd23">









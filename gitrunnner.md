# GitRunners

Check Git version:

```bash
git --version
````

Useful links:

* [Build and test code with GitHub Actions](https://docs.github.com/en/actions/tutorials/build-and-test-code)
* [Add self-hosted runners](https://docs.github.com/en/actions/how-tos/manage-runners/self-hosted-runners/add-runners)

---

## Step 1. Create a self-hosted runner in GitHub

1. Go to your repository on GitHub → **Settings → Actions → Runners → Add runner**.
2. Choose **Linux**.
3. Follow the commands GitHub provides:
4. https://github.com/naaamlas/SecDevOpsDemo/settings/actions/runners/new
5. 

```bash

# Create a folder
$ mkdir actions-runner && cd actions-runner# Download the latest runner package
$ curl -o actions-runner-linux-x64-2.329.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.329.0/actions-runner-linux-x64-2.329.0.tar.gz# Optional: Validate the hash
$ echo "194f1e1e4bd02f80b7e9633fc546084d8d4e19f3928a324d512ea53430102e1d  actions-runner-linux-x64-2.329.0.tar.gz" | shasum -a 256 -c# Extract the installer
$ tar xzf ./actions-runner-linux-x64-2.329.0.tar.gz

---

## Step 2. Configure the runner


Now go to 
Repo > Settings > Actions > Runner > choose OS and run config... 

```bash
./config.sh --url https://github.com/naaamlas/SecDevOpsDemo --token <TOKEN>
```

---

## Step 3. Run the runner

Run interactively:

```bash
./run.sh
```

Or as a service to run continuously:

```bash
sudo ./svc.sh install
sudo ./svc.sh start
```

---

## Step 4. Create a GitHub Actions workflow

Create the workflow folder and file:

```bash
mkdir -p .github/workflows
touch .github/workflows/build.yml
nano .github/workflows/build.yml
```

Example workflow YAML:

```yaml
name: Linux Build

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build:
    # This job runs on our self-hosted Linux runner labeled 'build'
    runs-on: [self-hosted, linux, build] # specify runner labels

    steps:
      # Checkout the repository
      - uses: actions/checkout@v3

      # Set up Python environment
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: 3.11

      # Install dependencies
      - name: Install dependencies
        run: pip install -r requirements.txt

      # Run tests
      - name: Run tests
        run: pytest
```

---

## Step 5. Commit and push workflow

```bash
git add .github/workflows/build.yml
git commit -m "Add self-hosted build workflow"
git push
```

Once pushed, GitHub Actions will detect the workflow and run it on your self-hosted runner.

## Adding git runner 
sudo adduser gh_runner
sudo su - gh_runner

## codeql sast workflow. 
CodeQL SAST workflow:

nano .github/workflows/codeql.yml
nano .github/workflows/codeql.yml

name: CodeQL

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  analyze:
    runs-on: self-hosted

    permissions:
      contents: read
      security-events: write

    steps:
      - uses: actions/checkout@v3
      - name: Initialize CodeQL
        uses: github/codeql-action/init@v2
        with:
          languages: python

      - name: Build
        run: |
          echo "Build step here if needed"

      - name: Analyze
        uses: github/codeql-action/analyze@v2



nano .github/workflows/security.yml


name: Secure Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  sast:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v3
      - run: semgrep --config auto

  sca:
    runs-on: self-hosted
    steps:
      - run: trivy fs .

  dast:
    runs-on: self-hosted
    needs: [ sast, sca ]
    steps:
      - run: docker run -t owasp/zap2docker-stable zap-baseline.py -t http://yourapp.local

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
mkdir actions-runner && cd actions-runner
curl -o actions-runner-linux-x64-<version>.tar.gz -L https://github.com/actions/runner/releases/download/v<version>/actions-runner-linux-x64-<version>.tar.gz
tar xzf ./actions-runner-linux-x64-<version>.tar.gz
```

---

## Step 2. Configure the runner

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

```

---

If you want, I can also make a **more visual version with tips, notes, and recommended labels for self-hosted runners** to make it like a mini handbook for your team. Do you want me to do that?
```

# CI/CD Integration

Integrate Guardrail into your CI/CD pipeline to catch issues before they reach production.

## GitHub Actions

### Basic Setup

```yaml
# .github/workflows/guardrail.yml
name: Guardrail

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  guardrail:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Dependencies
        run: npm ci

      - name: Guardrail Gate
        run: npx guardrail gate
```

### With SARIF Upload

```yaml
name: Guardrail + Code Scanning

on: [push, pull_request]

jobs:
  guardrail:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      
      - run: npm ci
      
      - name: Guardrail Gate
        run: npx guardrail gate --sarif
        continue-on-error: true
      
      - name: Upload SARIF
        uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: .guardrail/results.sarif
```

### Full Pipeline

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test

  guardrail:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npx guardrail gate --policy=strict

  deploy:
    runs-on: ubuntu-latest
    needs: [test, guardrail]
    if: github.ref == 'refs/heads/main'
    steps:
      - run: echo "Deploy to production"
```

---

## GitLab CI

```yaml
# .gitlab-ci.yml
stages:
  - test
  - security
  - deploy

guardrail:
  stage: security
  image: node:20
  script:
    - npm ci
    - npx guardrail gate
  artifacts:
    paths:
      - .guardrail/
    reports:
      codequality: .guardrail/results.json
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH == "main"
```

---

## CircleCI

```yaml
# .circleci/config.yml
version: 2.1

jobs:
  guardrail:
    docker:
      - image: cimg/node:20.11
    steps:
      - checkout
      - run: npm ci
      - run: npx guardrail gate
      - store_artifacts:
          path: .guardrail

workflows:
  main:
    jobs:
      - guardrail
```

---

## Azure DevOps

```yaml
# azure-pipelines.yml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: NodeTool@0
    inputs:
      versionSpec: '20.x'
  
  - script: npm ci
    displayName: 'Install dependencies'
  
  - script: npx guardrail gate
    displayName: 'Guardrail Gate'
  
  - publish: .guardrail
    artifact: guardrail-report
    condition: always()
```

---

## Jenkins

```groovy
// Jenkinsfile
pipeline {
    agent {
        docker { image 'node:20' }
    }
    
    stages {
        stage('Install') {
            steps {
                sh 'npm ci'
            }
        }
        
        stage('Guardrail') {
            steps {
                sh 'npx guardrail gate'
            }
            post {
                always {
                    archiveArtifacts artifacts: '.guardrail/**'
                }
            }
        }
    }
}
```

---

## Best Practices

### 1. Run on PRs

Always run Guardrail on pull requests:

```yaml
on:
  pull_request:
    branches: [main, develop]
```

### 2. Use Strict Policy for Production

```bash
# Development
guardrail gate --policy=lenient

# Production branches
guardrail gate --policy=strict
```

### 3. Cache Dependencies

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

### 4. Upload Reports

Always save reports as artifacts for debugging:

```yaml
- uses: actions/upload-artifact@v4
  if: always()
  with:
    name: guardrail-report
    path: .guardrail/
```

### 5. Block Merges

Configure branch protection to require Guardrail to pass.

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `GUARDRAIL_API_KEY` | API key for premium features |
| `CI` | Auto-detected in most CI systems |

```yaml
env:
  GUARDRAIL_API_KEY: ${{ secrets.GUARDRAIL_API_KEY }}
```

---

**Full documentation:** [getguardrail.io/docs/ci-cd](https://getguardrail.io/docs/ci-cd)

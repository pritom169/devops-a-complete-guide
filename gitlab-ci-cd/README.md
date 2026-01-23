# GitLab CI/CD

## What is CI/CD?

CI/CD stands for **Continuous Integration** and **Continuous Delivery/Deployment**. It is a methodology that enables development teams to deliver code changes more frequently and reliably through automation.

---

## Continuous Integration (CI)

### Definition

Continuous Integration is a development practice where developers frequently merge their code changes into a shared repository—typically multiple times per day. Each integration is automatically verified by building the application and running automated tests.

### Core Principles

1. **Single Source Repository**: All developers commit to a shared mainline (usually `main` or `master` branch)
2. **Automated Builds**: Every commit triggers an automated build process
3. **Self-Testing Builds**: The build process includes automated tests to validate code quality
4. **Fast Feedback**: Developers receive immediate feedback on their changes
5. **Trunk-Based Development**: Short-lived feature branches merged frequently to reduce integration complexity

### What Happens in a CI Pipeline?

```
Developer Pushes Code
        │
        ▼
┌───────────────────┐
│   Code Checkout   │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│  Install Dependencies  │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│   Static Analysis │  ← Linting, Code Style, SAST
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│    Unit Tests     │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│  Integration Tests│
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│   Build Artifact  │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│  Security Scans   │  ← Dependency scanning, Container scanning
└───────────────────┘
```

### Industry Best Practices for CI (2024-2025)

| Practice | Description |
|----------|-------------|
| **Shift-Left Testing** | Run tests as early as possible in the pipeline |
| **Parallel Test Execution** | Split test suites to run concurrently for faster feedback |
| **Caching** | Cache dependencies and build artifacts to speed up pipelines |
| **Ephemeral Build Environments** | Use containerized, reproducible build environments |
| **Security as Code** | Integrate SAST, DAST, and dependency scanning into every build |
| **Pipeline as Code** | Define CI configuration in version-controlled files (`.gitlab-ci.yml`) |

### Benefits of CI

- **Early Bug Detection**: Issues are caught within minutes of being introduced
- **Reduced Integration Risk**: Small, frequent integrations are easier to manage than large merges
- **Improved Code Quality**: Automated quality gates enforce standards
- **Faster Development Cycles**: Developers spend less time debugging integration issues
- **Increased Confidence**: Teams can release more frequently with confidence

---

## Continuous Delivery (CD)

### Definition

Continuous Delivery is an extension of CI that ensures code is always in a deployable state. After passing all automated tests, the application can be released to production at any time with the push of a button.

### Key Characteristics

- Every change that passes automated tests is **potentially releasable**
- Deployment to production requires **manual approval**
- Release decisions are **business decisions**, not technical ones
- Maintains a **deployment pipeline** that can deliver to any environment

---

## Continuous Deployment

### Definition

Continuous Deployment takes Continuous Delivery one step further—every change that passes all stages of the production pipeline is **automatically released to production** without human intervention.

### CI vs CD vs CD: The Spectrum

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                         │
│  Continuous Integration                                                 │
│  ├── Automated builds                                                   │
│  ├── Automated testing                                                  │
│  └── Merge to main branch                                               │
│                                                                         │
│      Continuous Delivery                                                │
│      ├── Everything in CI, plus:                                        │
│      ├── Automated deployment to staging                                │
│      ├── Manual approval for production                                 │
│      └── One-click production deployments                               │
│                                                                         │
│          Continuous Deployment                                          │
│          ├── Everything in Continuous Delivery, plus:                   │
│          └── Automated production deployments                           │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Modern CD Pipeline Architecture

### Typical CD Pipeline Stages

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Build   │───▶│   Test   │───▶│  Stage   │───▶│  Review  │───▶│  Prod    │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
                                     │               │
                                     ▼               ▼
                               ┌──────────┐    ┌──────────┐
                               │ Smoke    │    │ Manual   │
                               │ Tests    │    │ Approval │
                               └──────────┘    └──────────┘
```

### Deployment Strategies (Industry Standard)

| Strategy | Description | Use Case |
|----------|-------------|----------|
| **Blue-Green** | Maintain two identical environments; switch traffic instantly | Zero-downtime deployments |
| **Canary** | Gradually roll out changes to a small subset of users first | Risk mitigation for critical services |
| **Rolling** | Incrementally update instances in a deployment group | Kubernetes deployments, minimal resource overhead |
| **Feature Flags** | Deploy code with features disabled, enable progressively | A/B testing, gradual rollouts |
| **Shadow/Dark Launch** | Deploy to production but don't serve real traffic | Testing with production data patterns |

### GitLab CI/CD Core Concepts

A typical GitLab CI/CD workflow follows this sequence:

```mermaid
graph LR
    A[Code Changes<br/>Merged] --> B[Git Push to<br/>Remote Repository]
    B --> C[CI Pipeline<br/>Triggered]
    C --> D[run_tests]
    D --> E[build_image]
    E --> F[push_image]

    style A fill:#e1f5ff
    style B fill:#e1f5ff
    style C fill:#fff4e1
    style D fill:#e8f5e9
    style E fill:#e8f5e9
    style F fill:#e8f5e9
```

**Pipeline Configuration**

GitLab CI/CD pipelines are defined using a declarative YAML configuration file (`.gitlab-ci.yml`) stored in the repository root. This file specifies:

- **Stages**: The sequential phases of the pipeline (build, test, deploy)
- **Jobs**: Individual tasks that execute within each stage
- **Variables**: Environment-specific configuration values
- **Rules**: Conditions that determine when jobs run
- **Artifacts**: Files passed between pipeline stages

This "Pipeline as Code" approach ensures that CI/CD configuration is version-controlled, reviewable, and reproducible across environments.

## Jobs (Basic Building Blocks of Pipeline)

A **job** is the fundamental execution unit in GitLab CI/CD. Each job runs in an isolated container, executes a defined set of commands, and reports a pass/fail status. Jobs are the building blocks that, when combined, form your entire CI/CD pipeline.

### Job Structure and Configuration

GitLab CI/CD pipelines are configured by placing a `.gitlab-ci.yml` file in the repository root. GitLab automatically detects this file and executes the defined pipeline on each commit.

```yaml
run_tests:
  before_script:
    - echo "Preparing test data ..."
  script:
    - echo "Running tests..." # Changes made to the pipeline
  after_script:
    - echo "Clearing temporary files..."

build_image:
  script:
    - echo "Building the docker image..."
    - echo "Tagging the docker image"

push_image:
  script:
    - echo "Logging into docker registry..."
    - echo "Pushing docker image to registry.."
```

### Job Execution Lifecycle

```mermaid
graph TD
    subgraph run_tests
        A1[before_script<br/>Preparing test data] --> A2[script<br/>Running tests]
        A2 --> A3[after_script<br/>Clearing temporary files]
    end

    subgraph build_image
        B1[script<br/>Building the docker image] --> B2[script<br/>Tagging the docker image]
    end

    subgraph push_image
        C1[script<br/>Logging into docker registry] --> C2[script<br/>Pushing docker image to registry]
    end

    run_tests --> build_image
    build_image --> push_image

    style A1 fill:#fff4e1
    style A2 fill:#e8f5e9
    style A3 fill:#e1f5ff
    style B1 fill:#e8f5e9
    style B2 fill:#e8f5e9
    style C1 fill:#e8f5e9
    style C2 fill:#e8f5e9
```

### Pipeline as Code

By storing CI/CD configuration in `.gitlab-ci.yml`, the pipeline definition becomes version-controlled alongside the application code. This follows the **Infrastructure as Code (IaC)** philosophy—enabling code reviews for pipeline changes, maintaining audit history, and ensuring reproducibility across environments.

Each commit triggers a full pipeline execution, automatically validating all changes against the defined quality gates.

### Stages: Grouping and Ordering Jobs

Jobs within the same stage execute in parallel, while stages themselves run sequentially. This allows pipelines to enforce execution order where dependencies exist.

In the current configuration, `run_tests`, `build_image`, and `push_image` execute in parallel by default. However, the logical dependency requires sequential execution:

1. **Test** — Validate code quality before building
2. **Build** — Create the Docker image after tests pass
3. **Push** — Upload the image to the registry only after a successful build

To enforce sequential execution and logical grouping, GitLab CI/CD provides **stages**. Stages allow you to organize related jobs together and define explicit execution order across the pipeline. If any job fails, subsequent jobs are skipped and the pipeline terminates immediately.

The following configuration demonstrates how to assign jobs to stages:

```yaml
stages:
  - test
  - build
  - deploy

run_unit_tests:
  stage: test
  before_script:
    - echo "Preparing test data ..."
  script:
    - echo "Running unit tests..."
  after_script:
    - echo "Clearing temporary files..."

run_lint_tests:
  stage: test
  before_script:
    - echo "Preparing test data ..."
  script:
    - echo "Running lint tests..."
  after_script:
    - echo "Clearing temporary files..."

build_image:
  stage: build
  script:
    - echo "Building the docker image..."
    - echo "Tagging the docker image"

push_image:
  stage: build
  script:
    - echo "Logging into docker registry..."
    - echo "Pushing docker image to registry.."

deploy_image:
  stage: deploy
  script:
    - echo "Deploying new docker image to dev server..."
```

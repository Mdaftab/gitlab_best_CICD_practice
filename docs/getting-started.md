# Getting Started with GitLab CI/CD

This guide will help you set up and understand the basics of GitLab CI/CD pipelines.

## Prerequisites

- A GitLab account (GitLab.com or self-hosted GitLab instance)
- A project repository in GitLab
- Basic understanding of YAML syntax
- Access to a GitLab Runner (shared runners are available on GitLab.com)

## Setting Up Your First Pipeline

### 1. Create a `.gitlab-ci.yml` File

The first step is to create a `.gitlab-ci.yml` file in the root of your repository. This file defines your CI/CD pipeline configuration.

```yaml
# Basic .gitlab-ci.yml example
stages:
  - test
  - build
  - deploy

test_job:
  stage: test
  script:
    - echo "Running tests..."
    - echo "Tests completed successfully!"

build_job:
  stage: build
  script:
    - echo "Building application..."
    - echo "Build completed successfully!"
  artifacts:
    paths:
      - build/

deploy_job:
  stage: deploy
  script:
    - echo "Deploying application..."
    - echo "Deployment completed successfully!"
  environment:
    name: production
  only:
    - main
```

### 2. Commit and Push

Once you've created the `.gitlab-ci.yml` file, commit and push it to your repository:

```bash
git add .gitlab-ci.yml
git commit -m "Add GitLab CI/CD configuration"
git push
```

### 3. View Your Pipeline

After pushing the file, GitLab will automatically detect it and start running your pipeline:

1. Navigate to your project in GitLab
2. Go to **CI/CD > Pipelines** to see your pipeline running
3. Click on the pipeline to view details and job statuses

## Key Concepts

### Pipeline Structure

A GitLab CI/CD pipeline consists of:

- **Stages**: Groups of jobs that run in sequence
- **Jobs**: Individual units of work that run scripts
- **Scripts**: Commands that are executed in jobs

### Job Execution

Jobs are executed by GitLab Runners, which are agents that run your jobs and send the results back to GitLab.

### Artifacts

Artifacts are files generated during the pipeline execution that can be passed between jobs and downloaded after pipeline completion.

### Environments

Environments define where your application is deployed, such as development, staging, or production.

## Next Steps

- Explore the [Pipeline Architecture](architecture/overview.md) to understand how pipelines work
- Learn about different [Pipeline Stages](architecture/stages.md) and their purposes
- Check out the [Examples](../examples/) section for practical implementations

## Additional Resources

- [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)
- [GitLab CI/CD YAML Syntax Reference](https://docs.gitlab.com/ee/ci/yaml/)
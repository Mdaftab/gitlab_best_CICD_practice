# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains comprehensive guides, examples, and best practices for implementing GitLab CI/CD pipelines. It's designed to help DevOps and SRE professionals create efficient and effective CI/CD workflows using GitLab's features.

## Repository Structure

The repository is organized as follows:

- `docs/`: Documentation on GitLab CI/CD concepts and best practices
- `examples/`: Example pipeline configurations for different stages and use cases
  - `test/`: Testing stage examples (unit, integration, e2e)
  - `build/`: Build stage examples (Docker, packages, artifacts)
  - `deploy/`: Deployment examples (environments, Kubernetes, serverless)
  - `secure/`: Security scanning examples (SAST, DAST, dependency scanning)
  - `templates/`: Reusable pipeline templates
- `troubleshooting/`: Guides for diagnosing and fixing common pipeline issues

## GitLab CI/CD Concepts

When working with GitLab CI/CD configurations, be aware of these key concepts:

1. **Pipeline Configuration**: `.gitlab-ci.yml` files define the pipeline structure and behavior
2. **Stages**: Groups of jobs that run in sequence (e.g., test, build, deploy, secure)
3. **Jobs**: Individual units of work that run scripts
4. **Runners**: Agents that execute the jobs
5. **Artifacts**: Files generated during jobs that can be passed between stages
6. **Caching**: Mechanism to store dependencies between pipeline runs
7. **Variables**: Key-value pairs that can be used throughout the pipeline
8. **Rules**: Conditions that determine when jobs run
9. **Templates**: Reusable pipeline configurations
10. **Environments**: Deployment targets with tracking and controls

## Best Practices

When making changes to pipeline configurations:

1. Use GitLab's built-in CI/CD validation tools
2. Implement proper caching strategies for faster builds
3. Organize pipelines with clear stage progression
4. Utilize GitLab's security scanning features
5. Implement proper environment management
6. Use variables for configuration flexibility
7. Follow YAML best practices for readability
8. Consider pipeline performance impacts
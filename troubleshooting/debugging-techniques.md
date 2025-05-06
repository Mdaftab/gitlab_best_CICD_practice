# GitLab CI/CD Debugging Techniques

This guide provides effective techniques for debugging GitLab CI/CD pipelines to quickly identify and resolve issues.

## Pipeline Debugging Strategies

### 1. Enable Debug Logging

Add the `CI_DEBUG_TRACE` variable to your pipeline or job to enable detailed debug output:

```yaml
variables:
  CI_DEBUG_TRACE: "true"
```

For specific jobs only:

```yaml
my_job:
  variables:
    CI_DEBUG_TRACE: "true"
  script:
    - echo "Debugging enabled for this job"
```

### 2. Use Interactive Jobs

When investigating complex issues, create an interactive job that allows you to explore the environment:

```yaml
debug_environment:
  stage: debug
  when: manual
  script:
    - echo "Starting interactive debug session"
    - sleep 3600  # Keeps the job running for 1 hour
    - echo "Debug session ended"
  rules:
    - when: manual
      allow_failure: true
```

Then use the Terminal button in the job log to connect to the running environment.

### 3. Step-by-Step Execution

Break complex scripts into smaller steps with explicit output:

```yaml
debug_job:
  script:
    - echo "Step 1: Checking dependencies"
    - ls -la
    - echo "Step 2: Verifying environment"
    - env | sort
    - echo "Step 3: Running the command"
    - some-command || echo "Command failed with exit code $?"
```

### 4. Add Debug Checkpoints

Insert checkpoints in your scripts to identify where failures occur:

```yaml
build_job:
  script:
    - echo "CHECKPOINT 1: Starting build"
    - mkdir -p build
    - echo "CHECKPOINT 2: Installing dependencies"
    - npm ci
    - echo "CHECKPOINT 3: Running build"
    - npm run build
    - echo "CHECKPOINT 4: Build completed"
```

## System Information Gathering

### 1. Environment Inspection

Create a dedicated job or add commands to inspect the execution environment:

```yaml
inspect_environment:
  script:
    - echo "System Information:"
    - uname -a
    - cat /etc/os-release
    - echo "CPU Information:"
    - cat /proc/cpuinfo | grep "model name" | head -1
    - echo "Memory Information:"
    - free -h
    - echo "Disk Space:"
    - df -h
    - echo "Network Configuration:"
    - ip addr show
    - echo "Environment Variables:"
    - env | sort
    - echo "Installed Packages:"
    - if command -v dpkg > /dev/null; then dpkg -l | head -20; elif command -v rpm > /dev/null; then rpm -qa | head -20; fi
    - echo "Current Directory Contents:"
    - ls -la
```

### 2. Check Runner Status

When debugging runner issues, gather information about the runners:

```yaml
check_runner:
  script:
    - echo "Runner information:"
    - cat /etc/gitlab-runner/config.toml || echo "Config not accessible"
    - gitlab-runner --version || echo "Runner command not available"
    - echo "Docker information (if applicable):"
    - docker info || echo "Docker not available"
    - echo "System resources:"
    - top -b -n 1 | head -20
```

## Docker Container Debugging

### 1. Docker Image Inspection

When debugging Docker image issues:

```yaml
inspect_docker_image:
  image: ${CI_REGISTRY_IMAGE}:${CI_COMMIT_REF_SLUG}
  script:
    - echo "Docker image information:"
    - cat /etc/os-release
    - echo "Installed packages:"
    - if command -v dpkg > /dev/null; then dpkg -l | head -20; elif command -v rpm > /dev/null; then rpm -qa | head -20; fi
    - echo "Image environment variables:"
    - env | sort
    - echo "Filesystem layout:"
    - find / -type d -maxdepth 3 | sort
```

### 2. Docker Build Debugging

For Docker build issues:

```yaml
debug_docker_build:
  script:
    - echo "Building with verbose output"
    - docker build --progress=plain --no-cache -t debug-image .
    - echo "Inspecting built image"
    - docker history debug-image
    - docker run --rm debug-image ls -la
    - docker run --rm debug-image env
```

## Network and Dependency Debugging

### 1. Network Connectivity Tests

Test network connectivity to external services:

```yaml
network_debug:
  script:
    - echo "Testing DNS resolution:"
    - nslookup gitlab.com
    - echo "Testing HTTP connectivity:"
    - curl -v --connect-timeout 10 https://gitlab.com
    - echo "Testing registry connectivity:"
    - curl -v --connect-timeout 10 https://registry.hub.docker.com
    - echo "Network route:"
    - traceroute gitlab.com || echo "Traceroute not available"
```

### 2. Dependency Resolution Debugging

For dependency issues:

```yaml
dependency_debug:
  script:
    - echo "NPM registry connectivity:"
    - npm config get registry
    - curl -v --connect-timeout 10 https://registry.npmjs.org
    - echo "Package-lock.json analysis:"
    - jq '.dependencies' package-lock.json
    - echo "Dependency installation with verbose logging:"
    - npm ci --verbose
```

## Advanced Techniques

### 1. Script Export and Review

Export scripts for later analysis:

```yaml
export_scripts:
  script:
    - echo "Exporting job scripts to artifact"
    - mkdir -p debug-output
    - gitlab-ci-local-scripts > debug-output/generated-scripts.sh || echo "Command not available"
    - env > debug-output/environment.txt
  artifacts:
    paths:
      - debug-output/
    expire_in: 1 week
```

### 2. Performance Profiling

Identify performance bottlenecks:

```yaml
performance_debug:
  script:
    - echo "Starting performance profiling"
    - START_TIME=$(date +%s)
    - time npm ci
    - DEPS_TIME=$(date +%s)
    - echo "Dependencies installation took $((DEPS_TIME - START_TIME)) seconds"
    - time npm run build
    - BUILD_TIME=$(date +%s)
    - echo "Build process took $((BUILD_TIME - DEPS_TIME)) seconds"
    - time npm test
    - TEST_TIME=$(date +%s)
    - echo "Tests took $((TEST_TIME - BUILD_TIME)) seconds"
    - echo "Total execution time: $((TEST_TIME - START_TIME)) seconds"
```

### 3. Create Debug Reports

Generate comprehensive debug reports:

```yaml
generate_debug_report:
  script:
    - echo "Generating debug report"
    - mkdir -p debug-report
    - echo "## System Information" > debug-report/report.md
    - uname -a >> debug-report/report.md
    - echo "## Environment Variables" >> debug-report/report.md
    - env | sort >> debug-report/report.md
    - echo "## Installed Packages" >> debug-report/report.md
    - if command -v dpkg > /dev/null; then dpkg -l >> debug-report/packages.txt; elif command -v rpm > /dev/null; then rpm -qa >> debug-report/packages.txt; fi
    - echo "## Disk Space" >> debug-report/report.md
    - df -h >> debug-report/report.md
    - echo "## Repository Information" >> debug-report/report.md
    - git log -n 5 --pretty=format:"%h - %an, %ar : %s" >> debug-report/report.md
  artifacts:
    paths:
      - debug-report/
    expire_in: 1 week
```

## Debugging Specific Components

### 1. GitLab Runner Issues

```yaml
runner_debug:
  script:
    - echo "Runner configuration:"
    - gitlab-runner --version || echo "Runner command not available"
    - echo "Runner service status:"
    - systemctl status gitlab-runner || echo "Service command not available"
    - echo "Runner logs:"
    - tail -n 100 /var/log/gitlab-runner/gitlab-runner.log || echo "Logs not accessible"
```

### 2. Cache and Artifacts Issues

```yaml
cache_debug:
  script:
    - echo "Cache debugging"
    - echo "Current cache key: ${CI_COMMIT_REF_SLUG}"
    - echo "Cache paths:"
    - for path in $(echo "$CACHE_PATHS" | tr ',' ' '); do
    -   echo "Path: $path"
    -   ls -la "$path" || echo "Path does not exist"
    - done
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
      - .npm/
```

### 3. Docker Registry Problems

```yaml
registry_debug:
  script:
    - echo "Docker registry debugging"
    - echo "Testing registry authentication:"
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - echo "Listing available tags:"
    - curl --header "PRIVATE-TOKEN: $GITLAB_API_TOKEN" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/registry/repositories"
    - echo "Testing image pull:"
    - docker pull $CI_REGISTRY_IMAGE:latest || echo "Failed to pull latest image"
```

## Best Practices for Debugging

1. **Start simple**: Begin with the most basic issue that could cause the problem
2. **Isolate the problem**: Create minimal reproduction cases
3. **Add visibility**: Insert echo statements or export variables to see what's happening
4. **Review logs carefully**: Pipeline logs contain valuable information
5. **Use CI lint**: Validate your YAML configuration
6. **Test locally**: Use tools like `gitlab-runner exec` to run jobs locally
7. **Seek help appropriately**: Include relevant logs and configuration when asking for help
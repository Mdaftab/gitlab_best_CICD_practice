# Common GitLab CI/CD Pipeline Errors and Solutions

This guide covers the most common errors encountered in GitLab CI/CD pipelines and provides practical solutions for resolving them.

## Job Execution Errors

### 1. Runner Connection Issues

**Error:**
```
Preparing the "docker+machine" executor
ERROR: Job failed (system failure): prepare environment: Error response from daemon: Get "https://registry-1.docker.io/v2/": dial tcp: lookup registry-1.docker.io: no such host
```

**Solutions:**
- Check runner's network connectivity
- Verify Docker daemon is running
- Configure DNS settings properly
- Use self-hosted runners if external connectivity is restricted

### 2. Resource Limitations

**Error:**
```
ERROR: Job failed: execution took longer than 1h0m0s seconds
```

**Solutions:**
- Increase job timeout in project settings
- Optimize job execution to reduce runtime
- Split long-running jobs into smaller parts
- Use more powerful runners

### 3. Exit Code Errors

**Error:**
```
$ npm test
...
Exited with code 1
```

**Solutions:**
- Check script logic
- Examine test failures
- Add debugging output
- Review logs for specific error messages

## Configuration Errors

### 1. YAML Syntax Issues

**Error:**
```
Found errors in your .gitlab-ci.yml:
jobs:build config should implement a script: or a trigger: keyword
```

**Solutions:**
- Validate YAML syntax using CI Lint tool
- Check indentation and spacing
- Use a YAML linter
- Review GitLab CI/CD syntax documentation

### 2. Invalid References

**Error:**
```
Job failed: reference not found: template
```

**Solutions:**
- Verify referenced templates exist
- Check the path to included files
- Ensure anchor names are defined before use
- Validate variable references

### 3. Permission Issues

**Error:**
```
ERROR: Job failed: failed to pull image "registry.example.com/image:latest" with specified auth: 
denied: access forbidden
```

**Solutions:**
- Verify registry credentials
- Check deployment tokens
- Set CI_JOB_TOKEN_SCOPE_ENABLED variable
- Configure proper permissions in project settings

## Docker and Container Errors

### 1. Image Not Found

**Error:**
```
ERROR: Job failed: failed to pull image "node:16": 
pulling image: Error response from daemon: manifest for node:16 not found: manifest unknown
```

**Solutions:**
- Verify image name and tag
- Check registry availability
- Use image digest instead of tag
- Switch to a different registry or mirror

### 2. Docker-in-Docker Issues

**Error:**
```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. 
Is the docker daemon running?
```

**Solutions:**
- Use Docker-in-Docker service
- Configure DOCKER_HOST and TLS settings
- Use Docker socket binding (when appropriate)
- Check runner privileges

### 3. Registry Authentication

**Error:**
```
ERROR: authentication required
```

**Solutions:**
- Ensure CI_REGISTRY_USER and CI_REGISTRY_PASSWORD are available
- Use proper docker login command
- Configure credentials helper
- Check registry authentication configuration

## Cache and Artifacts Errors

### 1. Cache Retrieval Failure

**Error:**
```
WARNING: Failed to extract cache
```

**Solutions:**
- Check cache key definition
- Verify paths exist before caching
- Ensure runner has enough disk space
- Consider using dependencies instead of cache for critical files

### 2. Artifact Upload Failure

**Error:**
```
ERROR: Uploading artifacts to coordinator... error 
error=couldn't execute POST against 
https://gitlab.example.com/api/v4/jobs/1234/artifacts: 
Unauthorized
```

**Solutions:**
- Check job token permissions
- Verify artifact paths exist
- Reduce artifact size if too large
- Check storage settings in GitLab

## Variable and Environment Errors

### 1. Missing Variables

**Error:**
```
/bin/bash: line 87: $API_TOKEN: ambiguous redirect
```

**Solutions:**
- Define missing variables in project/group settings
- Check variable scope (environment, protected)
- Use default values when appropriate
- Add error handling for missing variables

### 2. Variable Masking Issues

**Error:**
```
WARNING: Failed to mask a secret variable
```

**Solutions:**
- Ensure variable value is at least 8 characters
- Use proper quoting in scripts
- Avoid interpolating masked variables
- Use variable expansion carefully

### 3. Environment Creation Failures

**Error:**
```
ERROR: Failed to create dynamic environment: 
environment name must match the environment name syntax
```

**Solutions:**
- Use valid environment names
- Check environment scope
- Verify deployment platform credentials
- Review environment configuration

## Git and Repository Errors

### 1. Git Fetch Issues

**Error:**
```
fatal: unable to access 'https://gitlab.example.com/group/project.git/': 
The requested URL returned error: 503
```

**Solutions:**
- Check GitLab instance status
- Verify Git LFS settings
- Reduce repository size
- Use shallow cloning

### 2. Submodule Problems

**Error:**
```
fatal: No url found for submodule path 'submodule' in .gitmodules
```

**Solutions:**
- Configure GIT_SUBMODULE_STRATEGY
- Update .gitmodules with correct URLs
- Set up proper authentication for submodules
- Consider alternatives to submodules

## Troubleshooting Techniques

### 1. Debug Mode

Enable debug logging:
```yaml
variables:
  CI_DEBUG_TRACE: "true"
```

### 2. Interactive Debugging

Use interactive debugging when available:
```yaml
job_name:
  script:
    - echo "Starting interactive session"
  when: manual
  interruptible: true
  environment:
    name: debug
```

### 3. Examine Job Logs

- Look for specific error messages
- Check command exit codes
- Verify environment variables
- Review resource usage

### 4. CI Lint Tool

- Access via CI/CD > Pipelines > CI Lint
- Validates configuration syntax
- Provides immediate feedback
- Shows expanded includes and references
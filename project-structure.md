# Project Structure

```
.
├── README.md                     # Project overview and navigation guide
├── CONTRIBUTING.md               # Guidelines for contributors
├── docs/                         # Documentation directory
│   ├── getting-started.md        # Setup and basics
│   ├── architecture/             # Pipeline architecture concepts
│   │   ├── overview.md           # High-level pipeline architecture
│   │   ├── stages.md             # Understanding pipeline stages
│   │   └── workflows.md          # Complex workflow patterns
│   └── best-practices/           # General best practices
│       ├── organization.md       # Organizing pipelines
│       ├── performance.md        # Performance optimization
│       └── maintenance.md        # Maintaining CI/CD pipelines
├── examples/                     # Example implementations
│   ├── test/                     # Testing stage examples
│   │   ├── unit-tests/           # Unit testing examples
│   │   │   ├── README.md         # Explanation of unit test patterns
│   │   │   └── .gitlab-ci.yml    # Example unit test configuration
│   │   ├── integration-tests/    # Integration testing examples
│   │   └── e2e-tests/            # End-to-end testing examples
│   ├── build/                    # Build stage examples
│   │   ├── docker/               # Docker build examples
│   │   ├── packages/             # Package building examples
│   │   └── artifacts/            # Artifact management examples
│   ├── deploy/                   # Deployment examples
│   │   ├── environments/         # Environment configuration examples
│   │   │   ├── development/      # Dev environment setup
│   │   │   ├── staging/          # Staging environment setup
│   │   │   └── production/       # Production environment setup
│   │   ├── kubernetes/           # Kubernetes deployment examples
│   │   ├── serverless/           # Serverless deployment examples
│   │   └── progressive/          # Progressive deployment examples
│   ├── secure/                   # Security scanning examples
│   │   ├── sast/                 # Static application security testing
│   │   ├── dast/                 # Dynamic application security testing
│   │   ├── dependency-scanning/  # Dependency scanning examples
│   │   └── secret-detection/     # Secret detection examples
│   └── templates/                # Reusable pipeline templates
│       ├── language-specific/    # Templates for different languages
│       └── framework-specific/   # Templates for different frameworks
├── troubleshooting/              # Troubleshooting guides
│   ├── common-errors.md          # Common pipeline errors and solutions
│   ├── debugging-techniques.md   # How to debug pipeline issues
│   ├── performance-issues.md     # Diagnosing and fixing performance problems
│   └── checklist.md              # Pre-flight checklist for pipelines
└── resources/                    # Additional resources
    ├── tools/                    # Helpful tools and scripts
    ├── glossary.md               # GitLab CI/CD terminology
    └── further-reading.md        # Links to external resources
```
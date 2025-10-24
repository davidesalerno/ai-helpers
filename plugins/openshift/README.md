# OpenShift Plugin

A collection of Claude Code commands for OpenShift development workflows.

## Installation

### From the Claude Code Plugin Marketplace

1. **Add the marketplace** (if not already added):
   ```bash
   /plugin marketplace add openshift-eng/ai-helpers
   ```

2. **Install the openshift plugin**:
   ```bash
   /plugin install openshift@ai-helpers
   ```

3. **Use the commands**:
   ```bash
   /openshift:bump-deps k8s.io/api
   ```

## Available Commands

### Dependency Bumping

#### `/openshift:bump-deps` - Bump Dependencies

Automates dependency updates in OpenShift projects with comprehensive analysis, testing, and optional Jira ticket and PR creation.

**Basic Usage:**
```bash
# Bump to latest version
/openshift:bump-deps k8s.io/api

# Bump to specific version
/openshift:bump-deps golang.org/x/net v0.20.0

# Bump with Jira ticket
/openshift:bump-deps github.com/spf13/cobra --create-jira

# Bump with Jira ticket and PR
/openshift:bump-deps github.com/prometheus/client_golang --create-jira --create-pr
```

**Supported Dependency Types:**
- Go modules (go.mod)
- npm packages (package.json)
- Container images (Dockerfile)
- Python packages (requirements.txt, pyproject.toml)

**Key Features:**
- Automatic version discovery and compatibility checking
- Changelog and breaking change analysis
- Automated testing (unit, integration, e2e)
- Jira ticket creation with comprehensive details
- Pull request creation with proper formatting
- Handles direct and indirect dependencies
- Security vulnerability detection
- Batch updates for related dependencies

**Arguments:**
- `<dependency>` (required): Package identifier (e.g., `k8s.io/api`, `@types/node`)
- `[version]` (optional): Target version (defaults to latest stable)
- `--create-jira`: Create a Jira ticket for the update
- `--create-pr`: Create a pull request (implies --create-jira)
- `--jira-project <PROJECT>`: Specify Jira project (default: auto-detect)
- `--component <COMPONENT>`: Specify Jira component (default: auto-detect)
- `--skip-tests`: Skip running tests (creates draft PR)
- `--force`: Force update even if tests fail

**Examples:**

1. Simple bump to latest:
   ```bash
   /openshift:bump-deps k8s.io/client-go
   ```

2. Bump with custom Jira project:
   ```bash
   /openshift:bump-deps sigs.k8s.io/controller-runtime --create-jira --jira-project OCPBUGS
   ```

3. Bump container image:
   ```bash
   /openshift:bump-deps registry.access.redhat.com/ubi9/ubi-minimal
   ```

4. Batch update Kubernetes dependencies:
   ```bash
   /openshift:bump-deps "k8s.io/*"
   ```

See [commands/bump-deps.md](commands/bump-deps.md) for full documentation.

### E2E Test Generation

#### `/openshift:new-e2e-test` - Generate E2E Tests

Generate end-to-end tests for OpenShift features.

See [commands/new-e2e-test.md](commands/new-e2e-test.md) for full documentation.

## Development

### Adding New Commands

To add a new command to this plugin:

1. Create a new markdown file in `commands/`:
   ```bash
   touch plugins/openshift/commands/your-command.md
   ```

2. Follow the structure from existing commands (see `commands/bump-deps.md` for reference)

3. Include these sections:
   - Name
   - Synopsis
   - Description
   - Implementation
   - Return Value
   - Examples
   - Arguments
   - Error Handling
   - Notes

4. Test your command:
   ```bash
   /openshift:your-command
   ```

### Plugin Structure

```
plugins/openshift/
├── .claude-plugin/
│   └── marketplace.json          # Plugin metadata
├── commands/
│   ├── bump-deps.md              # Dependency bumping command
│   ├── new-e2e-test.md           # E2E test generation
│   └── ...                        # Additional commands
└── README.md                      # This file
```

## Related Plugins

- **utils** - General utilities including `process-renovate-pr` for processing Renovate PRs
- **jira** - Jira automation and issue management
- **git** - Git workflow automation
- **ci** - OpenShift CI integration

## Contributing

Contributions are welcome! When adding new OpenShift-related commands:

1. Ensure the command is specific to OpenShift development workflows
2. Follow the existing command structure and documentation format
3. Include comprehensive examples and error handling
4. Test with real OpenShift projects
5. Update this README with new command documentation

## License

See [LICENSE](../../LICENSE) for details.

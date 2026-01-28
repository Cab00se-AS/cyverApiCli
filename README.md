# Cyver API CLI

A comprehensive command-line interface tool for interacting with the Cyver API. This CLI provides easy access to manage clients, projects, findings, users, teams, and continuous projects through a well-structured command hierarchy.

## Features

- **Multi-role Support**: Separate command groups for client and pentester operations
- **Flexible Output Formats**: Support for table, JSON, short, and custom output formats
- **Configurable Verbosity**: Multiple verbosity levels for detailed logging (including raw request/response with `-vvv`)
- **Token Management**: Automatic token refresh and secure storage
- **API Version Support**: Support for multiple API versions (v2.2)
- **Interactive Configuration**: Guided setup and configuration management
- **Comprehensive Error Handling**: Structured error management with retry mechanisms
- **Input Validation**: Robust validation with user-friendly error messages
- **Retry Logic**: Automatic retry for transient failures with exponential backoff
- **Findings Import**: Import findings from JSON or Markdown (Obsidian frontmatter) files with field mapping and enum conversion
- **Evidence Management**: Create evidence attached to findings; import evidence from JSON or Markdown files
- **Pentester Labels**: List and filter labels by type (Finding, Client, Project, Assets, All)

## Installation

### From Source
```bash
go install github.com/yourusername/cyverApiCli@latest
```

### From Repository
```bash
git clone https://github.com/yourusername/cyverApiCli.git
cd cyverApiCli
go mod download
go build
```

### Prerequisites
- Go 1.23.0 or later
- Git

## Quick Start

1. **Initialize Configuration**:
```bash
cyverApiCli config init
```

2. **Authenticate**:
```bash
cyverApiCli apiAuth getToken --username your-email@example.com
```

3. **List Available Commands**:
```bash
cyverApiCli --help
```

## Command Tree

```
cyverApiCli
├── apiAuth                    # API Authentication
│   └── getToken              # Get authentication token
├── client                    # Client Operations
│   ├── get-client-info       # Get client information
│   ├── update-client-info    # Update client information
│   ├── list-clients          # List all clients
│   ├── get-projects          # Get client projects
│   ├── get-project-by-id     # Get specific project by ID
│   ├── get-project-request-forms # Get project request forms
│   ├── request-project       # Request a new project
│   ├── get-continuous-projects # Get continuous projects
│   ├── get-continuous-project-by-id # Get specific continuous project
│   ├── get-continuous-project-request-forms # Get continuous project forms
│   ├── request-continuous-project # Request continuous project
│   ├── get-findings          # Get findings
│   ├── get-finding-by-id     # Get specific finding by ID
│   ├── set-finding-status    # Update finding status
│   ├── get-assets            # Get assets
│   ├── create-asset          # Create new asset
│   ├── delete-asset          # Delete asset
│   ├── update-asset          # Update asset
│   ├── get-users             # Get users
│   └── create-user           # Create new user
├── config                    # Configuration Management
│   ├── init                  # Initialize CLI configuration
│   ├── view                  # View current configuration
│   ├── refresh-token         # Refresh access token
│   └── re-auth               # Re-authenticate
├── pentester                 # Pentester Operations
│   ├── clients               # Client Management (Pentester View)
│   │   ├── list              # List pentester clients
│   │   ├── get               # Get specific client
│   │   ├── create            # Create new client
│   │   ├── update            # Update client
│   │   ├── delete            # Delete client
│   │   ├── get-assets        # Get client assets
│   │   ├── create-asset      # Create client asset
│   │   └── update-asset      # Update client asset
│   ├── findings              # Findings Management
│   │   ├── list              # List findings
│   │   ├── get               # Get specific finding
│   │   ├── create            # Create new finding
│   │   ├── update            # Update finding
│   │   ├── delete            # Delete finding
│   │   ├── import            # Import findings (JSON or Markdown)
│   │   └── evidence          # Evidence attached to findings
│   │       ├── create        # Create evidence for a finding
│   │       └── import        # Import evidence from file (JSON or Markdown)
│   ├── labels                # Labels Management
│   │   └── list              # List labels (by type, with pagination)
│   ├── projects              # Projects Management
│   │   ├── list              # List projects
│   │   ├── create            # Create new project
│   │   ├── get               # Get specific project
│   │   ├── delete            # Delete project
│   │   ├── update-status     # Update project status
│   │   ├── set-assets        # Set project assets
│   │   ├── set-users         # Set project users
│   │   ├── set-teams         # Set project teams
│   │   ├── get-checklists    # Get project checklists
│   │   ├── get-compliance-norms # Get compliance norms
│   │   ├── get-report-versions # Get report versions
│   │   ├── get-report        # Get project report
│   │   └── upload-file       # Upload project file
│   ├── users                 # Users Management
│   │   ├── list              # List users
│   │   ├── create            # Create new user
│   │   ├── get               # Get specific user
│   │   ├── update            # Update user
│   │   └── delete            # Delete user
│   ├── teams                 # Teams Management
│   │   ├── list              # List teams
│   │   ├── create            # Create new team
│   │   ├── get               # Get specific team
│   │   ├── update            # Update team
│   │   └── delete            # Delete team
│   └── continuous-projects   # Continuous Projects Management
│       ├── list              # List continuous projects
│       ├── create            # Create continuous project
│       ├── get               # Get specific continuous project
│       ├── delete            # Delete continuous project
│       ├── update-status     # Update continuous project status
│       ├── set-assets        # Set continuous project assets
│       ├── set-users         # Set continuous project users
│       ├── set-teams         # Set continuous project teams
│       ├── get-runs          # Get continuous project runs
│       ├── complete-run      # Complete continuous project run
│       ├── get-report-versions # Get report versions
│       ├── get-report        # Get continuous project report
│       └── upload-file       # Upload file to continuous project
└── help                      # Help about any command
```

## Configuration

### Initial Setup
The CLI uses a YAML configuration file located at `~/.cyverApiCli.yaml`. Initialize it with:

```bash
cyverApiCli config init
```

### Configuration Options
- **API Base URL**: The base URL for the Cyver API
- **API Version**: Supported versions (v2.2)
- **Token Management**: Automatic token refresh and storage
- **Output Format**: Default output format (table, JSON, custom)

### Viewing Configuration
```bash
cyverApiCli config view
```

## Authentication

### Getting a Token
```bash
cyverApiCli apiAuth getToken --username your-email@example.com
```

### Token Refresh
```bash
cyverApiCli config refresh-token
```

### Re-authentication
```bash
cyverApiCli config re-auth
```

## Usage Examples

### Client Operations
```bash
# List all clients
cyverApiCli client list-clients

# Get specific project
cyverApiCli client get-project-by-id --project-id 550e8400-e29b-41d4-a716-446655440000

# Create a new asset
cyverApiCli client create-asset --body '{"name": "Web Server", "type": "server"}'

# Get findings with custom output
cyverApiCli client get-findings --output custom --max-columns 6
```

### Pentester Operations
```bash
# List all projects (pentester view)
cyverApiCli pentester projects list

# Create a new finding (title, type, severity, status)
cyverApiCli pentester findings create --project-id <uuid> --title "SQL Injection" --severity high --type Vulnerability --status draft

# On success, the create command prints the finding URL

# Import findings from JSON
cyverApiCli pentester findings import findings.json --project-id <uuid>

# Import findings from Markdown/Obsidian frontmatter
cyverApiCli pentester findings import finding.md --project-id <uuid> --file-type markdown

# List labels (default: all types)
cyverApiCli pentester labels list
cyverApiCli pentester labels list --type finding --max-results 20 --filter "web"

# Create evidence for a finding
cyverApiCli pentester findings evidence create --finding-id <uuid> --title "Request capture" --location /login --ip 192.168.1.1

# Import evidence from JSON or Markdown
cyverApiCli pentester findings evidence import evidence.json --finding-id <uuid>
cyverApiCli pentester findings evidence import evidence.md --finding-id <uuid> --file-type markdown

# Update project status
cyverApiCli pentester projects update-status --project-id 550e8400-e29b-41d4-a716-446655440000 --status "in-progress"

# Get team information
cyverApiCli pentester teams get --team-id 6ba7b810-9dad-11d1-80b4-00c04fd430c8
```

## Pentester Findings

### Create Finding

Creates a new finding with the given attributes. The `--title` value is sent to the API as `name`.

**Required:** `--project-id`, `--title`

**Flags:**
- `--title` – Finding title (required; sent as `name` to API)
- `--description` – Finding description
- `--type` – `Vulnerability` or `Observation`
- `--severity` – `critical`, `high`, `medium`, `low`, `info` (API scale: info=0, low=1, medium=2, high=3, critical=4)
- `--status` – Default `draft`; other values: pending-fix, fixed, accepted, to-review, reviewed, mitigated, partial-fix, false-positive, raised, reopen, acknowledged, identified
- `--trigger-events` – Whether to trigger events (default: false)

On successful creation (201), the CLI prints the finding URL:  
`{Host}/App/Projects/Details/{project-id}/Finding/{finding-id}`

### Import Findings

Import findings from structured files. Supports **JSON** and **Markdown (Obsidian frontmatter)**.

**Required:** `--project-id`, file path

**Flags:**
- `--file-type` – `json` (default) or `markdown` (also accepts `md`, `obsidian`)
- `--trigger-events` – Whether to trigger events (default: false)

**JSON format:** Array of objects or a single object. Fields map to the API; unknown fields are ignored.  
Supported fields include: `name`/`title`, `description`, `type`, `status`, `severity`, `code`, `complianceStatus`, `complianceComment`, `impact`, `impactDescription`, `likelihood`, `likelihoodDescription`, `recommendation`, `backgroundInformation`, `cweList`, `cveList`, `mitreAttackTacticsList`, `mitreAttackTechniquesList`, `assetIdList`, `labelIds`, and others from the CreateOrUpdateFindingRequest schema.

**Markdown (Obsidian) format:** Frontmatter between `---` delimiters.  
- One key-value per line: `key: value`
- Lists use indented lines starting with `-`

Example Markdown frontmatter:
```markdown
---
title: SQL Injection
description: Found in login form
type: Vulnerability
severity: high
status: draft
cweList:
  - CWE-89
  - CWE-564
---
```

Enum strings (`type`, `status`, `severity`) are converted to the API’s numeric values. Extra properties in the file are ignored.

## Pentester Findings Evidence

Create and import evidence attached to findings (CreateOrEditFindingInstance API).

### Create Evidence

**Command:** `cyverApiCli pentester findings evidence create`

**Required:** `--finding-id`, `--title`

**Flags:**
- `--finding-id` – Finding ID to attach evidence to (required)
- `--title` – Evidence title (required)
- `--asset`, `--location`, `--version`
- `--ip`, `--hostname`, `--port`, `--protocol`
- `--issue-details`, `--reproduce`, `--evidence`
- `--visible-in-report` – Include in report (default: true)

### Import Evidence

Import evidence from a structured file and attach all records to one finding.

**Command:** `cyverApiCli pentester findings evidence import [file-path]`

**Required:** `--finding-id`, file path

**Flags:**
- `--finding-id` – Finding ID to attach evidence to (required)
- `--file-type` – `json` (default) or `markdown` (also `md`, `obsidian`)

**JSON:** Array of evidence objects or a single object. Each record must have `title`. Other supported fields: `asset`, `location`, `version`, `ip`, `hostname`, `port`, `protocol`, `issueDetails` / `issue-details`, `reproduce`, `evidence`, `visibleInReport` / `visible-in-report`. Unknown fields are ignored.

**Markdown (Obsidian):** One evidence per file. Frontmatter between `---` with `key: value`; same field names as above.

Example Markdown:
```markdown
---
title: Login request capture
location: /login
ip: 192.168.1.1
hostname: target.example.com
port: "443"
protocol: HTTPS
issue-details: Credentials in cleartext
reproduce: "1. Intercept traffic\n2. Submit form"
evidence: Base64 request dump...
visible-in-report: true
---
```

## Pentester Labels

List labels with optional filters and pagination.

**Command:** `cyverApiCli pentester labels list`

**Flags:**
- `--type` – Label type: `finding`/`0`, `client`/`1`, `project`/`2`, `assets`/`3`, `all`/`4` (default: `all`, i.e. 4)
- `--max-results` – Max results (default: 10)
- `--skip-count` – Number to skip (default: 0)
- `--filter` – Text filter
- `--output` – `json`, `table`, or `custom` (default: table)
- `--max-columns` – For custom table (default: 4)

## Output Formats

The CLI supports multiple output formats:

- **table**: Human-readable table format (default)
- **json**: Complete JSON response
- **short**: Simplified JSON with ID and name only
- **custom**: Interactive field selection for table output

### Output Format Examples
```bash
# Table output (default)
cyverApiCli client get-projects

# Full JSON output
cyverApiCli client get-projects --output json

# Custom table with specific columns
cyverApiCli client get-projects --output custom --max-columns 4
```

## Verbosity Levels

Control the amount of logging output:

- **-v**: Show basic request information (method, URL)
- **-vv**: Show request headers and basic response info
- **-vvv**: Show full request/response details including body

### Verbosity Examples
```bash
# Basic verbosity
cyverApiCli -v client get-projects

# High verbosity for debugging
cyverApiCli -vvv pentester findings list
```

## Global Flags

- `-c, --config`: Specify config file path
- `-v, --verbose`: Increase verbosity level (can be used multiple times)
- `-h, --help`: Show help information

## Recent Updates and Changes

### Findings

- **Create**
  - `--title` is sent as `name` in the API request body.
  - Added `--type` with values `Vulnerability` or `Observation`.
  - `--status` defaults to `draft`; supports all API status values.
  - `--severity` limited to `critical`, `high`, `medium`, `low`, `info`. API uses 0–4 (info=0, low=1, medium=2, high=3, critical=4).
  - On 201 success, prints the finding URL using the `result` field from the response.

- **Import**
  - New structured file import for creating findings (replaces simple file-path upload).
  - **File types:** `--file-type json` (default) or `--file-type markdown` (or `md` / `obsidian`).
  - **JSON:** Accepts an array of finding objects or a single object. Unknown fields are ignored; known fields are mapped to the API (e.g. `title` → `name`). Enums (`type`, `status`, `severity`) can be strings or numbers.
  - **Markdown:** Parses Obsidian-style frontmatter between `---` delimiters. Supports `key: value` and indented list items with `-`. One finding per file.
  - Requires `--project-id`. Each finding is created via the pentester findings create API; import reports success/failure counts and prints finding URLs for created items.

- **Update**
  - Update finding uses `name` (not `title`) in the request body when changing the title.

### Pentester Findings Evidence

- **Create:** `pentester findings evidence create` – Create evidence attached to a finding. Required: `--finding-id`, `--title`. Optional: `--asset`, `--location`, `--version`, `--ip`, `--hostname`, `--port`, `--protocol`, `--issue-details`, `--reproduce`, `--evidence`, `--visible-in-report` (default true). Uses the CreateOrEditFindingInstance app service endpoint.
- **Import:** `pentester findings evidence import [file-path]` – Import evidence from JSON or Markdown. Requires `--finding-id`. `--file-type json` (default) or `markdown`. JSON: array or single object; each record needs `title`. Markdown: one evidence per file via Obsidian frontmatter. Reports success/failure counts.

### Pentester Labels

- New `pentester labels` command group with `list` subcommand.
- List labels with `--type`, `--max-results`, `--skip-count`, `--filter`.
- Type values: `finding` (0), `client` (1), `project` (2), `assets` (3), `all` (4). Default is `all`.
- Output formats: `json`, `table`, `custom`.

### General

- **Verbosity:** With `-vvv`, raw HTTP request and response (including body) are printed to stderr.
- **Auth:** 2FA flow only runs when the API responds with `RequiresTwoFactorVerification: true`.
- **Project details:** `pentester projects get` correctly shows project data from the API’s `Result` field.

## Development

### Prerequisites
- Go 1.23.0 or later
- Git

### Building from Source
```bash
# Clone the repository
git clone https://github.com/yourusername/cyverApiCli.git
cd cyverApiCli

# Install dependencies
go mod download

# Build the project
go build

# Run tests
go test ./...
```

### Project Structure
```
cyverApiCli/
├── cmd/                    # Command definitions
│   ├── client/           # Client-specific commands
│   ├── pentester/         # Pentester-specific commands
│   ├── shared/            # Shared utilities
│   ├── error_handler.go   # Error handling utilities
│   ├── config.go          # Configuration commands
│   └── root.go            # Root command
├── internal/              # Internal packages
│   ├── api/              # API client implementations
│   │   └── versions/     # API version implementations
│   ├── config/           # Configuration management
│   └── errors/           # Error handling system
├── logger/               # Logging utilities
├── output/               # Output formatting
├── docs/                 # Documentation
└── main.go              # Application entry point
```

### Error Handling System
The project includes a comprehensive error handling system located in `internal/errors/`:

- **`errors.go`**: Core error types and utilities
- **`retry.go`**: Retry mechanisms with exponential backoff
- **`validation.go`**: Input validation utilities

### Key Dependencies
- **Cobra**: CLI framework
- **Viper**: Configuration management
- **Zerolog**: Structured logging
- **go-pretty**: Table formatting

## Troubleshooting

### Common Issues

1. **Authentication Errors**
   - Ensure your credentials are correct
   - Check if 2FA is enabled and provide the code
   - Verify your account has the necessary permissions

2. **Configuration Issues**
   - Run `cyverApiCli config init` to recreate configuration
   - Check file permissions on `~/.cyverApiCli.yaml`
   - Verify API base URL is correct

3. **Token Expiration**
   - Use `cyverApiCli config refresh-token` to refresh
   - Re-authenticate with `cyverApiCli config re-auth`

4. **Output Format Issues**
   - Use `--output json` for complete JSON responses
   - Use `--output custom` for interactive field selection
   - Adjust `--max-columns` for table formatting

### Getting Help
```bash
# General help
cyverApiCli --help

# Command-specific help
cyverApiCli client --help
cyverApiCli pentester projects --help

# Verbose output for debugging
cyverApiCli -vvv [command]
```

## Error Handling

The CLI includes a comprehensive error handling system with:

### Error Types and Codes
- **Configuration Errors**: `CONFIG_MISSING`, `CONFIG_INVALID`, `CONFIG_FILE_NOT_FOUND`
- **Authentication Errors**: `AUTH_FAILED`, `TOKEN_EXPIRED`, `TOKEN_INVALID`, `CREDENTIALS_INVALID`
- **API Errors**: `API_UNAUTHORIZED`, `API_FORBIDDEN`, `API_NOT_FOUND`, `API_RATE_LIMITED`, `API_SERVER_ERROR`
- **Validation Errors**: `VALIDATION_FAILED`, `INVALID_INPUT`, `MISSING_REQUIRED`
- **Internal Errors**: `INTERNAL_ERROR`, `NOT_IMPLEMENTED`, `UNEXPECTED_TYPE`

### Error Severity Levels
- **Low**: Warnings that don't prevent execution
- **Medium**: Errors that prevent command execution
- **High**: Critical errors that may affect system stability
- **Critical**: Fatal errors that require immediate attention

### Key Features
- **Structured Error Types**: Standardized error codes and severity levels
- **Automatic Retry**: Built-in retry logic for transient failures with exponential backoff
- **Input Validation**: Comprehensive validation with clear error messages
- **User-Friendly Messages**: Clear, actionable error messages for users
- **Logging Integration**: Detailed logging for debugging and monitoring
- **Exit Code Mapping**: Appropriate exit codes based on error severity

### Error Handling Examples

#### Basic Error Handling
```go
// Validate input parameters
if maxResultCount < 0 {
    shared.HandleError(cmd, errors.NewCyverError(errors.ErrCodeValidationFailed, "max-results must be non-negative", nil))
    return
}

// Handle API errors
projects, err := client.ClientOps.GetProjects(status, maxResultCount, skipCount, filter)
if err != nil {
    shared.HandleError(cmd, err)
    return
}
```

#### Input Validation
```go
// Validate required parameters
if bodyJSON == "" {
    shared.HandleError(cmd, errors.NewCyverError(errors.ErrCodeValidationFailed, "body is required", nil))
    return
}

// Validate JSON parsing
var body interface{}
if err := json.Unmarshal([]byte(bodyJSON), &body); err != nil {
    shared.HandleError(cmd, errors.NewCyverError(errors.ErrCodeValidationFailed, "invalid JSON body", err))
    return
}
```

#### Output Format Validation
```go
validFormats := []string{"json", "short", "table"}
isValidFormat := false
for _, format := range validFormats {
    if outputFormat == format {
        isValidFormat = true
        break
    }
}

if !isValidFormat {
    shared.HandleError(cmd, errors.NewCyverError(errors.ErrCodeValidationFailed, 
        fmt.Sprintf("Invalid output format '%s'. Valid options are: %s", outputFormat, strings.Join(validFormats, ", ")), nil))
    return
}
```

#### Error Types and Codes

The CLI uses structured error codes for better error handling:

**Validation Errors**:
- `VALIDATION_FAILED`: Input validation failed
- `INVALID_INPUT`: Invalid input format
- `MISSING_REQUIRED`: Required parameter missing

**API Errors**:
- `API_UNAUTHORIZED`: Authentication failed
- `API_FORBIDDEN`: Access denied
- `API_NOT_FOUND`: Resource not found
- `API_RATE_LIMITED`: Rate limit exceeded

**Configuration Errors**:
- `CONFIG_INVALID`: Invalid configuration
- `CONFIG_MISSING`: Configuration missing

**Internal Errors**:
- `INTERNAL_ERROR`: Unexpected internal error
- `NOT_IMPLEMENTED`: Feature not implemented
- `UNEXPECTED_TYPE`: Unexpected type error

#### Error Severity Levels

- **Low**: Warnings that don't prevent execution
- **Medium**: Errors that prevent command execution
- **High**: Critical errors that may affect system stability
- **Critical**: Fatal errors that require immediate attention

#### Best Practices

1. **Always use `shared.HandleError`** for consistent error handling
2. **Provide context** in error messages for better user experience
3. **Use appropriate error codes** for different error types
4. **Handle errors immediately** after they occur
5. **Validate input** before making API calls
6. **Test error scenarios** to ensure proper error handling

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

MIT License - see LICENSE file for details 
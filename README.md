# JetClient CLI

Command-line interface for running JetClient API requests and tests.

## Requirements

- Java 17 or higher

## Installation

1. Download the latest release (`jetclient-cli.zip` or `jetclient-cli.tar.gz`)
2. Extract the archive:
   ```bash
   # For zip
   unzip jetclient-cli.zip

   # For tar.gz
   tar -xzf jetclient-cli.tar.gz
   ```
3. The extracted folder contains:
   ```
   jetclient-cli/
   ├── bin/
   │   ├── jetclient      # Unix/macOS launcher
   │   └── jetclient.bat  # Windows launcher
   └── lib/
       └── ...            # JAR files
   ```

### Running

**Option 1: Run directly**
```bash
# Unix/macOS
./jetclient-cli/bin/jetclient --help

# Windows
jetclient-cli\bin\jetclient.bat --help
```

**Option 2: Add to PATH (recommended)**
```bash
# Unix/macOS - add to ~/.bashrc or ~/.zshrc
export PATH="$PATH:/path/to/jetclient-cli/bin"

# Windows - add to system PATH via Environment Variables
# Add: C:\path\to\jetclient-cli\bin
```

Then run from anywhere:
```bash
jetclient --help
```

## Docker

### Pull from GitHub Container Registry
```bash
docker pull ghcr.io/jetclient/jetclient-cli:latest
```

### Run with Docker
```bash
# Show help
docker run --rm ghcr.io/jetclient/jetclient-cli

# Run tests (mount project directory)
docker run --rm -v $(pwd):/workspace ghcr.io/jetclient/jetclient-cli run .

# With environment
docker run --rm -v $(pwd):/workspace ghcr.io/jetclient/jetclient-cli run . -e production

# Generate report
docker run --rm -v $(pwd):/workspace ghcr.io/jetclient/jetclient-cli run . \
  -r junit --reporter-out junit=./results.xml
```

### CI/CD Example (GitHub Actions)
```yaml
jobs:
  api-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run API tests
        run: |
          docker run --rm \
            -v ${{ github.workspace }}:/workspace \
            ghcr.io/jetclient/jetclient-cli:latest \
            run . -e ci -r junit --reporter-out junit=./results.xml

      - name: Publish Test Results
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: API Tests
          path: results.xml
          reporter: java-junit
```

## Project Structure

CLI looks for `jetclient.md` in:
1. Current directory
2. `.jetclient/` subdirectory

Run from project root or `.jetclient/` folder.

## Usage

```bash
jetclient run [PROJECT_DIR] [OPTIONS]   # Execute requests and tests
jetclient version                        # Show CLI version
```

### Options

| Option | Description |
|--------|-------------|
| `-e, --env <ENV>` | Environment to activate (repeatable for multiple groups) |
| `-i, --item <ITEM>` | ID or path of request, folder, or test suite to run (repeatable) |
| `-V, --var <KEY=VALUE>` | Set runtime variable (repeatable, overrides all other sources) |
| `--proxy <URL>` | HTTP/HTTPS/SOCKS proxy URL (e.g., `http://proxy:8080`) |
| `--no-proxy <HOSTS>` | Comma-separated hosts to bypass proxy |
| `-k, --insecure` | Disable SSL certificate verification |
| `--no-color` | Disable colored output |
| `-r, --reporter <TYPE>` | Output reporter: `cli`, `json`, `html`, `junit` (repeatable) |
| `--reporter-out <TYPE=PATH>` | Reporter output path (e.g., `json=./report.json`) |
| `-n, --iterations <N>` | Number of times to run (default: 1) |
| `--delay <MS>` | Delay between iterations in milliseconds |

**Default reporter outputs** (when `--reporter-out` not specified):
- JSON: `./jetclient-report.json`
- JUnit: `./jetclient-report.xml`
- HTML: `./jetclient-report.html`

### Environment Variables

| Variable | Description |
|----------|-------------|
| `JETCLIENT_ENV` | Comma-separated environments to activate |
| `JETCLIENT_ITEMS` | Comma-separated item IDs or paths to run |
| `JETCLIENT_VAR_*` | Runtime variables (e.g., `JETCLIENT_VAR_apiKey=secret`) |
| `JETCLIENT_INSECURE` | Set to `true` to disable SSL verification |
| `JETCLIENT_PROXY` | Proxy URL (e.g., `http://proxy:8080`) |
| `JETCLIENT_NO_COLOR` | Disable colored output (any value) |
| `NO_COLOR` | Standard [no-color](https://no-color.org/) convention |
| `NO_PROXY` | Comma-separated hosts to bypass proxy |
| `HTTPS_PROXY` / `HTTP_PROXY` | Standard proxy env vars (lower priority than `JETCLIENT_PROXY`) |

## Examples

Run all requests in a project:
```bash
jetclient run ./my-api-project
```

Run with specific environment:
```bash
jetclient run ./my-api-project -e production
```

Run specific items:
```bash
jetclient run ./my-api-project -i "Authentication/Login" -i "Users/Get All"
```

Run with variables:
```bash
jetclient run ./my-api-project -V "baseUrl=https://api.example.com" -V "apiKey=secret"
```

Generate JSON report:
```bash
jetclient run ./my-api-project -r json --reporter-out json=./report.json
```

**Note:** Items (`-i`) can be specified by path (contains `/`) or ID.

## Exit Codes

| Code | Description |
|------|-------------|
| 0 | All tests passed |
| 1 | One or more tests failed |
| 2 | Configuration or input error |
| 3 | Runtime error |

## Documentation

For more information, visit [jetclient.io/docs/cli](https://jetclient.io/docs/cli)

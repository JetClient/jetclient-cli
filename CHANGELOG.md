# Changelog

## [1.0.1] - 2026-02-10

### Fixed

- Enable cookie jar for HTTP requests

## [1.0.0] - 2026-02-02

### Added

- Initial release of JetClient CLI
- `run` command to execute requests, folders, and test suites
- `version` command to display CLI version
- Environment selection with `-e, --env` option
- Item filtering with `-i, --item` option
- Runtime variables with `-V, --var` option
- Proxy support with `--proxy` and `--no-proxy` options
- SSL verification bypass with `-k, --insecure` option
- Multiple reporter types: `cli`, `json`, `html`, `junit`
- Custom reporter output paths with `--reporter-out`
- Iteration support with `-n, --iterations` and `--delay` options
- Environment variable support (`JETCLIENT_ENV`, `JETCLIENT_INSECURE`)
- Docker image published to GitHub Container Registry
- Multi-architecture Docker builds (amd64, arm64)

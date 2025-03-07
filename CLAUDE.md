# json_exporter Quick Reference

## Common Commands
- `make build` - Build the binary
- `make test` - Run all tests
- `make style` - Check code style with gofmt
- `make lint` - Run golangci-lint
- `make docker` - Build Docker images
- `go test ./exporter -v` - Run exporter tests with verbose output
- `go test ./exporter -run TestName` - Run specific test

## Code Style Guidelines
- Format with `gofmt`, enforce with `make style`
- Use standard Go naming: CamelCase for exported, camelCase for private
- Include license headers in all source files
- Explicit error handling with early returns and proper logging
- Use structured logging with slog for errors and warnings
- Write table-driven tests for comprehensive coverage
- Organize code in modular packages (config, exporter, cmd)
- Follow Prometheus ecosystem conventions for metrics
- Use YAML for configuration files
- Imports ordered: standard lib first, then external packages
- Documentation comments for exported functions and types
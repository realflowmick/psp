# PSP Implementations

This directory contains reference implementations of the Prompt Suggestion Protocol (PSP) in various forms and programming languages.

## Structure

### API Implementations

**Directory:** [`api/`](./api/)

Contains RESTful API server implementations that provide HTTP endpoints for:
- Managing tagged prompts
- Querying prompts by tags
- Validating PSP tags
- Tag discovery and navigation

See the [API directory](./api/) for implementations in different programming languages.

### MCP Server Implementations

**Directory:** [`mcp-servers/`](./mcp-servers/)

Contains Model Context Protocol (MCP) server implementations that:
- Expose PSP functionality through the MCP protocol
- Integrate with MCP-compatible clients and tools
- Provide tagged prompt management via MCP

See the [MCP Servers directory](./mcp-servers/) for implementations in different programming languages.

## Implementation Status

Currently, this directory contains placeholder structure for future implementations. Implementations will be added as the specification matures.

## Contributing Implementations

To contribute an implementation:

1. Review the [PSP Specification](../standard/SPEC.md)
2. Choose the appropriate subdirectory (`api/` or `mcp-servers/`)
3. Create a language-specific subdirectory (e.g., `api/python/`, `mcp-servers/typescript/`)
4. Implement the PSP specification requirements
5. Include comprehensive tests and documentation
6. Submit a pull request

## Implementation Requirements

All implementations MUST:
- Comply with the [PSP Specification](../standard/SPEC.md)
- Include a README with setup and usage instructions
- Provide example usage
- Include appropriate tests
- Follow the language's best practices and conventions

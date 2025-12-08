# PSP MCP Server Implementations

This directory contains Model Context Protocol (MCP) server implementations of the Prompt Suggestion Protocol (PSP) in various programming languages.

## Purpose

The PSP MCP servers provide MCP-compliant interfaces for:
- Exposing tagged prompts as MCP resources
- Managing prompts through MCP tools
- Querying and filtering prompts by tags
- Integrating PSP functionality with MCP clients
- Validating and discovering PSP tags

## Available Implementations

Language-specific implementations will be added in subdirectories:

- `typescript/` - TypeScript/Node.js MCP server
- `python/` - Python MCP server
- `go/` - Go MCP server
- `rust/` - Rust MCP server

## MCP Integration (Reference)

A PSP MCP server should implement the following MCP features:

### Resources
- Expose tagged prompts as MCP resources
- Support resource templates for prompt discovery
- Provide resource metadata including PSP tags

### Tools
- `psp_create_prompt` - Create a new tagged prompt
- `psp_search_prompts` - Search prompts by tags
- `psp_validate_tags` - Validate PSP tag syntax
- `psp_list_tags` - List available tags

### Prompts
- Provide categorized prompt templates
- Support dynamic prompt generation based on tags
- Enable tag-based prompt filtering

## Implementation Guidelines

Each language-specific implementation should:
1. Follow the [PSP Specification](../../standard/SPEC.md)
2. Comply with the MCP specification
3. Provide a comprehensive README with setup instructions
4. Include usage examples with MCP clients
5. Implement proper error handling
6. Include tests for MCP protocol compliance
7. Follow language-specific best practices

## MCP Resources

- [MCP Specification](https://modelcontextprotocol.io/)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)

## Contributing

To add a new language implementation:
1. Create a new directory for the language
2. Implement the PSP functionality via MCP protocol
3. Add comprehensive documentation and examples
4. Submit a pull request

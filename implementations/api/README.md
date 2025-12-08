# PSP API Implementations

This directory contains RESTful API server implementations of the Prompt Suggestion Protocol (PSP) in various programming languages.

## Purpose

The PSP API servers provide HTTP endpoints for:
- Creating and managing tagged prompts
- Querying prompts by tags
- Validating PSP tag syntax and semantics
- Discovering available tags and namespaces
- Navigating tag hierarchies

## Available Implementations

Language-specific implementations will be added in subdirectories:

- `python/` - Python/Flask or FastAPI implementation
- `typescript/` - TypeScript/Node.js implementation
- `go/` - Go implementation
- `rust/` - Rust implementation
- `java/` - Java/Spring Boot implementation

## API Endpoints (Reference)

A compliant PSP API server should implement endpoints such as:

```
GET    /api/v1/prompts          - List all prompts
POST   /api/v1/prompts          - Create a new prompt
GET    /api/v1/prompts/:id      - Get a specific prompt
PUT    /api/v1/prompts/:id      - Update a prompt
DELETE /api/v1/prompts/:id      - Delete a prompt

GET    /api/v1/tags             - List all available tags
GET    /api/v1/tags/:namespace  - List tags in a namespace
POST   /api/v1/tags/validate    - Validate tag syntax

GET    /api/v1/search?tags=...  - Search prompts by tags
```

## Implementation Guidelines

Each language-specific implementation should:
1. Follow the [PSP Specification](../../standard/SPEC.md)
2. Provide a comprehensive README with setup instructions
3. Include API documentation (e.g., OpenAPI/Swagger)
4. Implement proper error handling and validation
5. Include unit and integration tests
6. Follow language-specific best practices

## Contributing

To add a new language implementation:
1. Create a new directory for the language
2. Implement the core PSP API functionality
3. Add comprehensive documentation
4. Submit a pull request

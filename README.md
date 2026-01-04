# Amplifier TypeScript/JavaScript LSP Bundle

**TypeScript/JavaScript code intelligence via typescript-language-server**

The TypeScript/JavaScript LSP Bundle provides pre-configured language support for Amplifier's LSP capabilities, enabling semantic code navigation, type information, and intelligent code exploration for TypeScript, JavaScript, React, Node.js, and other JavaScript ecosystem projects.

## What Is This Bundle?

This bundle extends the core LSP bundle with TypeScript/JavaScript-specific configuration, allowing AI agents to:

- **Navigate TypeScript/JavaScript code** - Jump to definitions, find references, locate implementations
- **Understand types** - Get type information, hover documentation, and call hierarchies (even in plain JavaScript!)
- **Explore modules** - List symbols, search across the workspace, trace imports
- **Support all JavaScript flavors** - TypeScript, JavaScript, React/JSX, Node.js, CommonJS, ESM

## Components

This bundle provides:

1. **typescript-lsp** - Behavior configuring typescript-language-server for TypeScript and JavaScript
2. **typescript-code-intel** - Agent specialized for TypeScript/JavaScript code exploration
3. **typescript-lsp** - Context providing TypeScript/JavaScript-specific LSP usage guidance

The bundle includes the core `lsp` bundle, which provides the `tool-lsp` module and general LSP capabilities.

## Prerequisites

typescript-language-server and TypeScript must be installed:

```bash
# Using npm (recommended)
npm install -g typescript-language-server typescript

# Verify installation
typescript-language-server --version
tsc --version
```

## Installation

### Using the Bundle

Load the bundle directly with Amplifier:

```bash
# Load from git URL
amplifier bundle use git+https://github.com/robotdad/amplifier-bundle-lsp-typescript@main
```

### Including in Another Bundle

Add to your bundle's `includes:` section:

```yaml
includes:
  - bundle: git+https://github.com/robotdad/amplifier-bundle-lsp-typescript@main
```

## Quick Start

### Basic Usage

```bash
# Start a session with TypeScript/JavaScript LSP capabilities
amplifier run --bundle lsp-typescript

# Navigate TypeScript/JavaScript code
> Find all references to the UserService class
> Go to the definition of authenticateUser
> What parameters does createSession accept?
> Show me the call hierarchy for the validateToken method
```

### Example Queries

```bash
# Type information
> What type does getUserById() return?
> Show me the interface for UserConfig

# Symbol search
> Find all classes that implement IAuthProvider
> Where is the Express Request type used?

# References
> Where is the authenticateUser function called in this project?
> Show me all usages of the APP_CONFIG constant

# Call hierarchy
> What functions call sendNotification?
> What does the handleRequest middleware call?

# React-specific
> Find all usages of the UserContext provider
> What props does the Button component accept?
> Where is the useAuth hook used?

# Module navigation
> Show me what this import resolves to
> Trace the dependency chain for this utility function
```

## Supported Project Types

This bundle works with all JavaScript ecosystem projects:

- **TypeScript** - Full type checking and navigation
- **JavaScript** - Type inference from JSDoc and usage patterns
- **React** - JSX/TSX support, component props, hooks
- **Node.js** - Built-in modules, require/import, CommonJS/ESM
- **Express** - Middleware types, request/response
- **Next.js** - Page components, API routes
- **Deno** - Deno-flavored TypeScript
- **Vue/Angular** - TypeScript support in frameworks

## Configuration

The bundle comes pre-configured with sensible defaults. The default configuration:

```yaml
tools:
  - module: tool-lsp
    config:
      languages:
        typescript:
          extensions: [.ts, .tsx, .mts, .cts]
          workspace_markers: [tsconfig.json, package.json, .git]
          server:
            command: [typescript-language-server, --stdio]
        javascript:
          extensions: [.js, .jsx, .mjs, .cjs]
          workspace_markers: [jsconfig.json, package.json, .git]
          server:
            command: [typescript-language-server, --stdio]
```

### Customizing

Override the configuration in your behavior:

```yaml
tools:
  - module: tool-lsp
    config:
      languages:
        typescript:
          initialization_options:
            preferences:
              includeInlayParameterNameHints: all
              includeInlayFunctionParameterTypeHints: true
```

## LSP Operations

All operations from the core LSP bundle are available:

| Operation | Description |
|-----------|-------------|
| `goToDefinition` | Find where a symbol is defined |
| `findReferences` | Find all references to a symbol |
| `hover` | Get documentation and type info |
| `documentSymbol` | List all symbols in a file |
| `workspaceSymbol` | Search for symbols across the workspace |
| `goToImplementation` | Find implementations of interfaces |
| `prepareCallHierarchy` | Get call hierarchy at a position |
| `incomingCalls` | Find callers of a function |
| `outgoingCalls` | Find functions called by a function |

## Bundle Structure

```
amplifier-bundle-lsp-typescript/
  bundle.yaml                    # Bundle definition, includes lsp
  behaviors/
    typescript-lsp.yaml          # typescript-language-server configuration
  agents/
    typescript-code-intel.md     # TypeScript/JavaScript-specialized agent
  context/
    typescript-lsp.md            # TypeScript/JavaScript LSP usage guidance
  .amplifier/
    bundle.yaml                  # Example combined bundle (foundation + lsp-typescript)
    settings.yaml                # Example settings file
```

## Project Configuration

### TypeScript Projects

Ensure you have a `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

### JavaScript Projects

For best results, add a `jsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "checkJs": true
  },
  "exclude": ["node_modules"]
}
```

### Type Definitions

Install type definitions for better type inference:

```bash
# Node.js
npm install --save-dev @types/node

# React
npm install --save-dev @types/react @types/react-dom

# Express
npm install --save-dev @types/express

# Jest
npm install --save-dev @types/jest
```

## Troubleshooting

### typescript-language-server not found

```bash
npm install -g typescript-language-server typescript
which typescript-language-server  # Should show a path
typescript-language-server --version
```

### Server fails to start

Check that TypeScript is installed:
```bash
npm install -g typescript
tsc --version
```

### Types not resolving

Install missing type definitions:
```bash
npm install --save-dev @types/node @types/react
```

### Using nvm

Ensure Node.js is activated:
```bash
nvm use --lts
npm install -g typescript-language-server typescript
```

## Philosophy

The TypeScript/JavaScript LSP Bundle follows Amplifier's core principles:

- **Composable** - Extends the core LSP bundle with TypeScript/JavaScript specifics
- **Pre-configured** - Works out of the box with typescript-language-server
- **Observable** - All operations emit events for logging and debugging
- **Minimal** - Adds only TypeScript/JavaScript-specific configuration
- **Universal** - Works for all JavaScript ecosystem projects

## Example: Using in a Bundle

Here's how to combine this bundle with Amplifier Foundation:

**.amplifier/bundle.yaml:**
```yaml
bundle:
  name: my-dev-bundle
  version: 1.0.0

includes:
  - bundle: git+https://github.com/microsoft/amplifier-foundation@main
  - bundle: git+https://github.com/robotdad/amplifier-bundle-lsp-typescript@main

default_behavior: foundation
```

Then activate it:
```bash
amplifier bundle use .amplifier/bundle.yaml
amplifier run
```

## Contributing

This bundle is open for contributions! To contribute:

1. Fork the repository
2. Create a feature branch
3. Test your changes thoroughly
4. Submit a pull request

## License

MIT License - See LICENSE file for details

## Author

Created by [@robotdad](https://github.com/robotdad)

## Project Status

**ACTIVE DEVELOPMENT**

This bundle is under active development. Feedback and contributions are welcome!

## Support

For issues and questions:
- Open an issue on GitHub
- Discuss in Amplifier community channels

---

**Ready to navigate TypeScript/JavaScript code semantically?** Install the bundle and start exploring!

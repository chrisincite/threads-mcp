# Threads MCP Server

A powerful Model Context Protocol (MCP) server for integrating with the Threads.com API. This server provides comprehensive tools for creating, reading, and managing Threads content programmatically.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-blue.svg)](https://www.typescriptlang.org/)
[![Node](https://img.shields.io/badge/Node-%3E%3D18.0.0-green.svg)](https://nodejs.org/)
[![Test Coverage](https://img.shields.io/badge/coverage-%3E90%25-brightgreen.svg)](https://vitest.dev/)

🌐 **[Visit the Website](https://pegasusheavyindustries.github.io/threads-mcp/)** for a better reading experience!

## Features

- 🚀 **Complete Threads API Integration** - Full support for Threads.com API v1.0
- 🔧 **MCP Protocol** - Standard Model Context Protocol implementation
- 📊 **Analytics & Insights** - Access engagement metrics and analytics
- 💬 **Conversation Management** - Create threads, replies, and manage conversations
- 🎯 **Type-Safe** - Full TypeScript support with Zod schema validation
- ⚡ **Rate Limiting** - Token bucket algorithm to prevent API rate limit violations
- 💾 **Caching Layer** - In-memory caching with TTL and LRU eviction
- 🔔 **Webhook Support** - Event-based webhooks with automatic retries and signature verification
- ✅ **Well Tested** - >90% test coverage with Vitest
- 🔄 **Modern Stack** - Built with latest npm packages and best practices

## Available Tools

The server provides the following MCP tools:

### Profile & Content Management

- **`threads_get_profile`** - Get authenticated user's profile information
- **`threads_get_threads`** - Retrieve user's threads (posts) with pagination
- **`threads_get_thread`** - Get a specific thread by ID
- **`threads_create_thread`** - Create new threads with text, images, or videos
- **`threads_delete_thread`** - Delete a thread owned by the authenticated user
- **`threads_reply_to_thread`** - Reply to existing threads

### Engagement & Analytics

- **`threads_get_insights`** - Get analytics for threads or user account
- **`threads_get_replies`** - Retrieve replies to a specific thread
- **`threads_get_conversation`** - Get full conversation threads

## Prerequisites

Before you begin, you'll need:

1. **Node.js 18+** - [Download here](https://nodejs.org/)
2. **pnpm** - Install with `npm install -g pnpm`
3. **Meta Developer Account** - [Sign up here](https://developers.facebook.com/)
4. **Threads Account** - Your personal Threads account
5. **OAuth 2.0 Credentials** - See [OAuth Setup Guide](docs/OAUTH_SETUP.md)

### Getting Your Threads API Credentials

**⚠️ Important**: Threads uses OAuth 2.0 authentication. You cannot use simple API keys.

#### Quick Setup (Automatic OAuth)

1. **Create a Meta App**
   - Visit [Meta for Developers](https://developers.facebook.com/)
   - Create a new app and add the Threads API product
   - Add OAuth redirect URI: `http://localhost:48810/callback`
   - Get your **App ID** and **App Secret**

2. **Configure Claude Desktop**
   - Add to your Claude Desktop config with just your App ID and Secret
   - The server will automatically handle OAuth on first run
   - A browser will open for you to authorize the app
   - Tokens are stored and automatically refreshed!

That's it! No manual OAuth flow needed. 🎉

📖 **For detailed instructions**, see the [OAuth Setup Guide](docs/OAUTH_SETUP.md)

## Installation

### From npm (when published)

```bash
pnpm install -g threads-mcp
```

### From Source

```bash
# Clone the repository
git clone https://github.com/PegasusHeavyIndustries/threads-mcp.git
cd threads-mcp

# Install dependencies
pnpm install

# Build the project
pnpm run build
```

## Configuration

### Automatic OAuth (Recommended)

The simplest way is to let the server handle OAuth automatically:

**For Claude Desktop**, edit your config file:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "threads": {
      "command": "npx",
      "args": ["-y", "@pegasusheavy/threads-mcp"],
      "env": {
        "THREADS_APP_ID": "your-app-id",
        "THREADS_APP_SECRET": "your-app-secret"
      }
    }
  }
}
```

When you start Claude Desktop:

1. A browser will automatically open
2. Log in and authorize the app
3. Done! Tokens are stored and auto-refreshed

### Manual Token (Legacy)

If you prefer to use pre-generated tokens:

```json
{
  "mcpServers": {
    "threads": {
      "command": "npx",
      "args": ["-y", "@pegasusheavy/threads-mcp"],
      "env": {
        "THREADS_ACCESS_TOKEN": "your-long-lived-token",
        "THREADS_USER_ID": "your-user-id"
      }
    }
  }
}
```

See [OAuth Setup Guide](docs/OAUTH_SETUP.md) for manual OAuth flow instructions.

## Usage

### Running the Server

```bash
# If installed globally
threads-mcp

# If running from source
pnpm run dev
```

### Using with MCP Clients

Configure your MCP client (like Claude Desktop) to use this server:

```json
{
  "mcpServers": {
    "threads": {
      "command": "threads-mcp",
      "env": {
        "THREADS_ACCESS_TOKEN": "your-access-token",
        "THREADS_USER_ID": "your-user-id"
      }
    }
  }
}
```

### Example Tool Calls

#### Get Your Profile

```json
{
  "name": "threads_get_profile",
  "arguments": {}
}
```

#### Create a Thread

```json
{
  "name": "threads_create_thread",
  "arguments": {
    "text": "Hello from the Threads MCP Server! 🚀",
    "replyControl": "everyone"
  }
}
```

#### Get Thread Insights

```json
{
  "name": "threads_get_insights",
  "arguments": {
    "threadId": "thread-id-here",
    "metrics": ["views", "likes", "replies", "reposts"]
  }
}
```

#### Reply to a Thread

```json
{
  "name": "threads_reply_to_thread",
  "arguments": {
    "threadId": "thread-id-to-reply-to",
    "text": "Great post!",
    "replyControl": "accounts_you_follow"
  }
}
```

## Development

### Project Structure

```
threads-mcp/
├── src/
│   ├── client/           # Threads API client
│   │   ├── __tests__/    # Client tests
│   │   └── threads-client.ts
│   ├── server/           # MCP server implementation
│   │   ├── __tests__/    # Server tests
│   │   └── server.ts
│   ├── types/            # TypeScript types & Zod schemas
│   │   ├── __tests__/    # Type tests
│   │   └── threads.ts
│   └── index.ts          # Entry point
├── dist/                 # Compiled output
├── coverage/             # Test coverage reports
└── tests/                # Integration tests
```

### Available Scripts

```bash
# Development
pnpm run dev              # Run in development mode with tsx
pnpm run build            # Compile TypeScript to JavaScript

# Testing
pnpm test                 # Run tests once
pnpm test:watch          # Run tests in watch mode
pnpm test:coverage       # Generate coverage report

# Code Quality
pnpm run lint            # Lint code with ESLint
pnpm run format          # Format code with Prettier
```

### Running Tests

The project has comprehensive test coverage (>90%):

```bash
# Run all tests
pnpm test

# Run with coverage
pnpm test:coverage

# Watch mode for development
pnpm test:watch
```

### Code Quality

```bash
# Lint your code
pnpm run lint

# Format code
pnpm run format
```

## API Reference

### ThreadsClient

The core client for interacting with the Threads API.

#### Constructor

```typescript
const client = new ThreadsClient({
  accessToken: string;
  userId: string;
  apiVersion?: string; // Optional, defaults to 'v1.0'
});
```

#### Methods

- `getProfile(fields?: string[]): Promise<ThreadsUser>`
- `getThreads(params?: GetMediaParams): Promise<ThreadsMedia[]>`
- `getThread(threadId: string, fields?: string[]): Promise<ThreadsMedia>`
- `createThread(params: CreateThreadParams): Promise<CreateThreadResponse>`
- `getThreadInsights(threadId: string, params: GetInsightsParams): Promise<ThreadsInsights[]>`
- `getUserInsights(params: GetInsightsParams): Promise<ThreadsInsights[]>`
- `getReplies(threadId: string, params?: GetRepliesParams): Promise<ThreadsReplies>`
- `getConversation(threadId: string, params?: GetRepliesParams): Promise<ThreadsConversation>`
- `replyToThread(threadId: string, text: string, replyControl?: string): Promise<CreateThreadResponse>`
- `validateToken(): Promise<boolean>`

## Error Handling

The client includes comprehensive error handling:

```typescript
import { ThreadsAPIError } from 'threads-mcp';

try {
  await client.createThread({ text: 'Hello!' });
} catch (error) {
  if (error instanceof ThreadsAPIError) {
    console.error('API Error:', error.message);
    console.error('Status Code:', error.statusCode);
    console.error('Response:', error.response);
  }
}
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Write tests for new features (maintain >90% coverage)
- Follow the existing code style
- Update documentation as needed
- Use conventional commits (enforced by Git hooks)

### Git Hooks

This project uses Husky to enforce code quality:

- **pre-commit**: Runs linting, formatting, and type checking
- **pre-push**: Runs full test suite, coverage check, and build verification
- **commit-msg**: Validates commit message format (conventional commits)

See [docs/GIT_HOOKS.md](docs/GIT_HOOKS.md) for detailed information about Git hooks.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Copyright (c) 2025 Pegasus Heavy Industries LLC

## Support

- 📚 [Threads API Documentation](https://developers.facebook.com/docs/threads)
- 🐛 [Report Issues](https://github.com/PegasusHeavyIndustries/threads-mcp/issues)
- 💬 [MCP Protocol Documentation](https://modelcontextprotocol.io/)

## Acknowledgments

- Built with [MCP SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- Powered by [Threads API](https://developers.facebook.com/docs/threads)
- Type validation with [Zod](https://github.com/colinhacks/zod)
- Testing with [Vitest](https://vitest.dev/)

---

Made with ❤️ by Pegasus Heavy Industries LLC

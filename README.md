# Wave - Full-Stack Monorepo

A modern full-stack application built with a monorepo architecture using Next.js, React Native, and NestJS.

## 🏗️ Project Structure

```
wave/
├── api/                 # Backend API (NestJS)
├── web/                 # Web application (Next.js)
├── mobile/              # Mobile application (React Native)
├── packages/            # Shared packages
│   ├── shared/          # Shared utilities and types
│   ├── store/           # State management
│   └── ui/              # Shared UI components
└── docs/                # Documentation
```

## 🚀 Quick Start

### Prerequisites

- Node.js (v18 or higher)
- pnpm (recommended) or npm
- Git

### Installation

1. Clone the repository:

```bash
git clone <your-repo-url>
cd wave
```

2. Install dependencies:

```bash
pnpm install
```

3. Set up environment variables:

```bash
# Copy environment files
cp api/.env.example api/.env
cp web/.env.example web/.env
cp mobile/.env.example mobile/.env
```

4. Start development servers:

```bash
# Start all applications
pnpm dev

# Or start individually:
pnpm dev:api    # Backend API
pnpm dev:web    # Web application
pnpm dev:mobile # Mobile application
```

## 📱 Applications

### Web Application

- **Framework**: Next.js 14
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Port**: 3001

### Mobile Application

- **Framework**: React Native with Expo
- **Language**: TypeScript
- **Port**: 8081

### Backend API

- **Framework**: NestJS
- **Language**: TypeScript
- **Database**: (Configure as needed)
- **Port**: 3000

## 🛠️ Development

### Available Scripts

```bash
# Development
pnpm dev              # Start all applications
pnpm dev:api          # Start API only
pnpm dev:web          # Start web only
pnpm dev:mobile       # Start mobile only

# Build
pnpm build            # Build all applications
pnpm build:api        # Build API only
pnpm build:web        # Build web only
pnpm build:mobile     # Build mobile only

# Testing
pnpm test             # Run all tests
pnpm test:api         # Run API tests
pnpm test:web         # Run web tests
pnpm test:mobile      # Run mobile tests

# Linting
pnpm lint             # Lint all applications
pnpm lint:fix         # Fix linting issues

# Type checking
pnpm type-check       # Check TypeScript types
```

### Package Management

This monorepo uses pnpm workspaces for efficient package management. Shared packages are located in the `packages/` directory and can be used across all applications.

## 📦 Shared Packages

- **@wave/shared**: Common utilities, types, and constants
- **@wave/store**: State management (Zustand/Redux)
- **@wave/ui**: Reusable UI components

## 🔧 Configuration

### Environment Variables

Each application has its own environment configuration:

- `api/.env` - Backend API configuration
- `web/.env` - Web application configuration
- `mobile/.env` - Mobile application configuration

### Turbo Configuration

The monorepo uses Turbo for build orchestration. Configuration is in `turbo.json`.

## 📚 Documentation

- [Architecture Guide](./docs/ARCHITECTURE.md)
- [Development Guide](./docs/DEVELOPMENT_GUIDE.md)
- [Quick Start Guide](./docs/QUICK_START.md)
- [Roadmap](./docs/ROADMAP.md)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

If you encounter any issues or have questions, please:

1. Check the [documentation](./docs/)
2. Search existing [issues](../../issues)
3. Create a new issue with detailed information

---

Built with ❤️ using modern web technologies

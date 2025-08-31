# Contributing to Wave

Thank you for your interest in contributing to Wave! This document provides guidelines and information for contributors.

## 🚀 Getting Started

### Prerequisites

- Node.js (v18 or higher)
- pnpm (recommended) or npm
- Git

### Development Setup

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/wave.git
   cd wave
   ```

3. Install dependencies:
   ```bash
   pnpm install
   ```

4. Set up environment variables:
   ```bash
   cp api/.env.example api/.env
   cp web/.env.example web/.env
   cp mobile/.env.example mobile/.env
   ```

5. Start the development servers:
   ```bash
   pnpm dev
   ```

## 📝 Development Workflow

### Branch Naming Convention

- `feature/feature-name` - For new features
- `bugfix/bug-description` - For bug fixes
- `hotfix/urgent-fix` - For urgent fixes
- `docs/documentation-update` - For documentation changes
- `refactor/refactor-description` - For code refactoring

### Commit Message Convention

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
type(scope): description

[optional body]

[optional footer(s)]
```

Types:
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `perf`: A code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools

Examples:
```
feat(web): add user authentication page
fix(api): resolve database connection timeout
docs: update README with new setup instructions
```

### Pull Request Process

1. Create a feature branch from `main`
2. Make your changes following the coding standards
3. Write or update tests as needed
4. Ensure all tests pass locally
5. Update documentation if necessary
6. Commit your changes using conventional commit messages
7. Push your branch and create a pull request
8. Fill out the pull request template completely
9. Request reviews from maintainers

## 🧪 Testing

### Running Tests

```bash
# Run all tests
pnpm test

# Run tests for specific applications
pnpm test:api
pnpm test:web
pnpm test:mobile

# Run tests in watch mode
pnpm test:watch
```

### Writing Tests

- Write unit tests for all new functionality
- Ensure test coverage is maintained or improved
- Use descriptive test names
- Follow the existing test patterns in the codebase

## 📋 Code Standards

### TypeScript

- Use TypeScript for all new code
- Provide proper type definitions
- Avoid using `any` type
- Use interfaces for object shapes
- Prefer `const` over `let` when possible

### Code Style

- Use Prettier for code formatting
- Follow ESLint rules
- Use meaningful variable and function names
- Write self-documenting code
- Add comments for complex logic

### File Organization

- Keep files focused and single-purpose
- Use consistent file naming (kebab-case for files, PascalCase for components)
- Organize imports: external libraries first, then internal imports
- Group related functionality together

## 🐛 Bug Reports

When reporting bugs, please include:

1. A clear description of the bug
2. Steps to reproduce
3. Expected vs actual behavior
4. Environment information (OS, browser, version)
5. Screenshots if applicable
6. Console errors or logs

## 💡 Feature Requests

When requesting features, please include:

1. A clear description of the feature
2. The problem it solves
3. Proposed implementation approach
4. Mockups or examples if applicable
5. Priority level

## 📚 Documentation

- Update documentation when adding new features
- Include code examples in documentation
- Keep README files up to date
- Document breaking changes clearly

## 🔒 Security

- Report security vulnerabilities privately to maintainers
- Do not include sensitive information in issues or pull requests
- Follow security best practices in your code

## 🎯 Review Process

- All pull requests require at least one review
- Address review comments promptly
- Be open to feedback and suggestions
- Maintain a respectful and collaborative environment

## 📞 Getting Help

- Check existing issues and pull requests
- Search the documentation
- Ask questions in discussions
- Join our community channels (if available)

## 📄 License

By contributing to Wave, you agree that your contributions will be licensed under the same license as the project.

---

Thank you for contributing to Wave! 🎉 
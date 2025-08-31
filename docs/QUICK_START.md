# Wave - Quick Start Guide

## Getting Started (First Day)

### Prerequisites

- Node.js 18+ installed
- PostgreSQL installed and running
- Git configured
- Code editor (VS Code recommended)
- pnpm installed globally (`npm install -g pnpm`)

### Step 1: Initialize Monorepo (pnpm + Turborepo)

```bash
# Create project directory
mkdir wave
cd wave

# Initialize root package.json
pnpm init

# Install Turborepo
pnpm add -D turbo

# Create workspace configuration
echo "packages:
  - 'packages/*'" > pnpm-workspace.yaml

# Create packages directory
mkdir packages

# Create root turbo.json for build pipeline
cat > turbo.json << EOF
{
  "\$schema": "https://turbo.build/schema.json",
  "globalDependencies": ["**/.env.*local"],
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**", "!.next/cache/**", "dist/**", "build/**"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "lint": {
      "dependsOn": ["^lint"]
    },
    "test": {
      "dependsOn": ["^build"],
      "outputs": ["coverage/**"]
    },
    "type-check": {
      "dependsOn": ["^build"]
    },
    "clean": {
      "cache": false
    }
  }
}
EOF

# Update root package.json scripts
cat > package.json << EOF
{
  "name": "wave",
  "version": "1.0.0",
  "description": "A real-time social networking platform built with Next.js, React Native, and Nest.js",
  "private": true,
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "test": "turbo run test",
    "lint": "turbo run lint",
    "clean": "turbo run clean",
    "type-check": "turbo run type-check",
    "format": "prettier --write \"**/*.{ts,tsx,md}\"",
    "format:check": "prettier --check \"**/*.{ts,tsx,md}\""
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0",
    "turbo": "^1.10.0",
    "typescript": "^5.0.0"
  },
  "engines": {
    "node": ">=18.0.0",
    "pnpm": ">=8.0.0"
  },
  "packageManager": "pnpm@8.0.0"
}
EOF

# Create root TypeScript config
cat > tsconfig.json << EOF
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["dom", "dom.iterable", "es6"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "baseUrl": ".",
    "paths": {
      "@wave/shared": ["./shared/src"],
      "@wave/ui": ["./ui/src"],
      "@wave/store": ["./store/src"],
      "@wave/backend": ["./api/src"],
      "@wave/web": ["./web/src"],
      "@wave/app": ["./mobile/src"]
    }
  },
  "include": [
    "packages/*/src/**/*",
    "packages/*/tsconfig.json"
  ],
  "exclude": [
    "node_modules",
    "packages/*/node_modules",
    "packages/*/dist",
    "packages/*/build",
    "packages/*/.next"
  ]
}
EOF

# Create root ESLint config
cat > .eslintrc.js << EOF
module.exports = {
  root: true,
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'prettier'
  ],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  env: {
    node: true,
    es6: true
  },
  parserOptions: {
    ecmaVersion: 2020,
    sourceType: 'module'
  },
  rules: {
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/no-explicit-any': 'warn',
    'prefer-const': 'error',
    'no-var': 'error'
  },
  ignorePatterns: [
    'node_modules/',
    'dist/',
    'build/',
    '.next/',
    'coverage/',
    'packages/*/node_modules/',
    'packages/*/dist/',
    'packages/*/build/',
    'packages/*/.next/'
  ]
};
EOF

# Create root Prettier config
cat > .prettierrc << EOF
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
EOF

# Install root dependencies
pnpm install
```

### Step 2: Initialize Shared Packages

```bash
# Create shared types package
cd packages
mkdir shared
cd shared

cat > package.json << EOF
{
  "name": "@wave/shared",
  "version": "1.0.0",
  "description": "Shared types and utilities for Wave platform",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch",
    "clean": "rm -rf dist",
    "type-check": "tsc --noEmit"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0"
  },
  "files": [
    "dist"
  ]
}
EOF

cat > tsconfig.json << EOF
{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["dist", "node_modules"]
}
EOF

mkdir src
touch src/index.ts

# Create shared UI package
cd ..
mkdir ui
cd ui

cat > package.json << EOF
{
  "name": "@wave/ui",
  "version": "1.0.0",
  "description": "Shared UI components for Wave platform",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch",
    "clean": "rm -rf dist",
    "type-check": "tsc --noEmit"
  },
  "dependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.0",
    "@types/react-dom": "^18.0.0",
    "typescript": "^5.0.0"
  },
  "files": [
    "dist"
  ]
}
EOF

cat > tsconfig.json << EOF
{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "jsx": "react-jsx"
  },
  "include": ["src/**/*"],
  "exclude": ["dist", "node_modules"]
}
EOF

mkdir src
touch src/index.ts

# Create shared store package
cd ..
mkdir store
cd store

cat > package.json << EOF
{
  "name": "@wave/store",
  "version": "1.0.0",
  "description": "Shared state management for Wave platform",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch",
    "clean": "rm -rf dist",
    "type-check": "tsc --noEmit"
  },
  "dependencies": {
    "zustand": "^4.4.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0"
  },
  "files": [
    "dist"
  ]
}
EOF

cat > tsconfig.json << EOF
{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["dist", "node_modules"]
}
EOF

mkdir src
touch src/index.ts

cd ../..
```

### Step 3: Initialize Backend (Nest.js)

```bash
cd packages
mkdir backend
cd backend

# Initialize Nest.js project
pnpm add -g @nestjs/cli
nest new . --package-manager pnpm --skip-git

# Install additional dependencies
pnpm add @nestjs/typeorm typeorm pg @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt class-validator class-transformer @nestjs/config @nestjs/websockets @nestjs/platform-socket.io socket.io

# Install dev dependencies
pnpm add -D @types/passport-jwt @types/bcrypt @types/pg

# Add shared package dependencies
pnpm add @wave/shared @wave/store

cd ../..
```

### Step 4: Initialize Frontend (Next.js)

```bash
cd packages
mkdir web
cd web

# Create Next.js project
pnpm create next-app@latest . --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

# Install additional dependencies
pnpm add axios @tanstack/react-query zustand socket.io-client react-hook-form @hookform/resolvers zod

# Add shared package dependencies
pnpm add @wave/shared @wave/ui @wave/store

cd ../..
```

### Step 5: Initialize Mobile App (React Native)

```bash
cd packages
mkdir app
cd app

# Create React Native project with Expo
pnpm create expo-app@latest . --template

# Install additional dependencies
pnpm add @react-navigation/native @react-navigation/stack @react-navigation/bottom-tabs
pnpm add axios @tanstack/react-query zustand socket.io-client
pnpm add expo-notifications expo-device expo-constants
pnpm add react-hook-form @hookform/resolvers zod

# Add shared package dependencies
pnpm add @wave/shared @wave/ui @wave/store

cd ../..
```

### Step 6: Build Shared Packages

```bash
# Build all shared packages
cd packages/shared && pnpm build && cd ../..
cd packages/ui && pnpm build && cd ../..
cd packages/store && pnpm build && cd ../..

# Or build all at once from root
pnpm run build
```

### Step 7: Set Up Database

```bash
# Create database
createdb wave_social

# Or using psql
psql -U postgres
CREATE DATABASE wave_social;
\q
```

### Step 8: Environment Configuration

Create `.env` files in respective package directories:

**Backend (.env):**

```env
# Database
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=your_password
DB_DATABASE=wave_social

# JWT
JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRES_IN=7d

# App
PORT=3001
NODE_ENV=development
```

**Frontend (.env.local):**

```env
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_WS_URL=ws://localhost:3001
```

**Mobile (.env):**

```env
EXPO_PUBLIC_API_URL=http://localhost:3001
EXPO_PUBLIC_WS_URL=ws://localhost:3001
```

### Step 9: Start Development

```bash
# Option 1: Start all services from root (recommended)
pnpm dev

# Option 2: Start individual services
# Terminal 1 - Backend
cd packages/backend
pnpm run start:dev

# Terminal 2 - Frontend
cd packages/web
pnpm run dev

# Terminal 3 - Mobile
cd packages/app
pnpm start
```

## First Features to Implement

### Backend Priority Order:

1. **User Entity & Authentication** (Day 1-2)
   - Create user entity with TypeORM
   - Implement JWT authentication
   - Create register/login endpoints

2. **Basic User CRUD** (Day 3-4)
   - User profile endpoints
   - Update profile functionality
   - Basic validation

3. **Post System** (Day 5-7)
   - Post entity and relationships
   - Create/read posts
   - Basic feed endpoint

### Frontend Priority Order:

1. **Authentication Pages** (Day 1-2)
   - Login form
   - Register form
   - Basic routing

2. **Layout & Navigation** (Day 3-4)
   - Main layout component
   - Navigation bar
   - Protected routes

3. **User Profile** (Day 5-7)
   - Profile page
   - Edit profile form
   - Basic styling

### Mobile Priority Order:

1. **Authentication Screens** (Day 1-2)
   - Login screen
   - Register screen
   - Navigation setup

2. **Layout & Navigation** (Day 3-4)
   - Tab navigation
   - Stack navigation
   - Protected routes

3. **User Profile** (Day 5-7)
   - Profile screen
   - Edit profile screen
   - Basic styling

## Monorepo Benefits

### **Shared Code:**

- **Types**: `@wave/shared` - Common TypeScript interfaces
- **UI Components**: `@wave/ui` - Reusable React components
- **State Management**: `@wave/store` - Zustand stores for both web and mobile

### **Development Workflow:**

- **Single command**: `pnpm dev` starts all services
- **Shared dependencies**: No duplicate packages
- **Type safety**: Shared types across all packages
- **Consistent tooling**: ESLint, Prettier, TypeScript config

### **Build Pipeline:**

- **Turborepo**: Fast, incremental builds
- **Caching**: Smart caching for faster rebuilds
- **Parallel execution**: Builds packages in parallel

## Development Tips

### Backend Development:

- Use Nest.js CLI generators: `nest generate resource users`
- Implement DTOs for all inputs
- Use class-validator for validation
- Write tests as you go

### Frontend Development:

- Use TypeScript strictly
- Implement proper error handling
- Use React Query for data fetching
- Keep components small and focused

### Mobile Development:

- Use TypeScript strictly
- Implement proper error handling
- Use React Query for data fetching
- Keep components small and focused
- Test on both iOS and Android

### Shared State Management:

- Use the shared Zustand store from `@wave/store`
- Implement platform-specific persistence
- Keep stores modular and focused
- Use TypeScript for type safety
- Test stores across both platforms

### Database:

- Use migrations for schema changes
- Add indexes for performance
- Test queries with real data

## Common Issues & Solutions

### Backend Issues:

- **CORS errors**: Configure CORS in main.ts
- **Database connection**: Check PostgreSQL is running
- **JWT errors**: Verify JWT_SECRET is set

### Frontend Issues:

- **API calls failing**: Check NEXT_PUBLIC_API_URL
- **Styling issues**: Verify Tailwind is configured
- **Type errors**: Use proper TypeScript interfaces

### Mobile Issues:

- **API calls failing**: Check EXPO_PUBLIC_API_URL
- **Navigation issues**: Verify React Navigation setup
- **Build errors**: Check Expo CLI and dependencies

## Next Steps After Quick Start

1. **Complete Phase 1** from the main development guide
2. **Set up Git repository** and push initial code
3. **Create feature branches** for each component
4. **Implement basic CRUD** operations
5. **Add tests** for core functionality

## Useful Commands

```bash
# Root (Monorepo)
pnpm dev                   # Start all services
pnpm build                 # Build all packages
pnpm test                  # Run all tests
pnpm lint                  # Lint all packages
pnpm clean                 # Clean all builds
pnpm type-check            # Type check all packages

# Individual Packages
cd packages/backend
pnpm run start:dev          # Start development server
pnpm run build             # Build for production
pnpm run test              # Run tests
pnpm run test:e2e          # Run e2e tests

cd packages/web
pnpm run dev               # Start development server (with Turbopack)
pnpm run dev --turbo       # Start with Turbopack explicitly
pnpm run build             # Build for production
pnpm run test              # Run tests
pnpm run lint              # Run linter

cd packages/app
pnpm start                 # Start Expo development server
pnpm run ios               # Run on iOS simulator
pnpm run android           # Run on Android emulator
pnpm run build:ios         # Build for iOS
pnpm run build:android     # Build for Android

# Shared Packages
cd packages/shared
pnpm build                 # Build shared types
pnpm dev                   # Watch mode for development

cd packages/ui
pnpm build                 # Build UI components
pnpm dev                   # Watch mode for development

cd packages/store
pnpm build                 # Build state management
pnpm dev                   # Watch mode for development

# Database
cd packages/backend
pnpm run typeorm:run       # Run migrations
pnpm run typeorm:generate  # Generate migration
```

## Resources for Immediate Help

- [Nest.js Quick Start](https://docs.nestjs.com/first-steps)
- [Next.js Tutorial](https://nextjs.org/learn)
- [TypeORM Getting Started](https://typeorm.io/#/getting-started)
- [Tailwind CSS Quick Start](https://tailwindcss.com/docs/installation)

---

**Start with the backend authentication system first, then move to the frontend. This gives you a solid foundation to build upon.**

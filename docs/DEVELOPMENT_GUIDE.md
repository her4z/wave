# Wave - Social Platform Development Guide

## Project Overview

**Wave** is a real-time social networking platform built with Next.js (frontend) and Nest.js (backend) that demonstrates mid-senior level software engineering skills.

## Project Structure

```
wave/
├── packages/
│   ├── backend/          # Nest.js API server
│   ├── web/             # Next.js frontend application
│   ├── app/             # React Native mobile application
│   ├── shared/          # Shared utilities and types
│   ├── ui/              # Shared UI components
│   └── store/           # Shared Zustand state management
├── docs/                # Documentation and guides
├── pnpm-workspace.yaml  # Workspace configuration
├── package.json         # Root package.json
└── turbo.json          # Turborepo configuration
```

## Development Phases

### Phase 1: Project Setup & Foundation (Week 1-2)

#### 1.1 Backend Setup (Nest.js)

**Location:** `packages/backend/`

**Tasks:**

- [ ] Initialize Nest.js project with TypeScript
- [ ] Set up PostgreSQL database with TypeORM
- [ ] Configure environment variables
- [ ] Set up JWT authentication
- [ ] Create basic user entity and DTOs
- [ ] Implement user registration/login endpoints
- [ ] Set up file upload service (AWS S3 or local storage)
- [ ] Configure CORS for frontend communication

**Key Files to Create:**

```
backend/
├── src/
│   ├── auth/
│   │   ├── auth.module.ts
│   │   ├── auth.service.ts
│   │   ├── auth.controller.ts
│   │   ├── jwt.strategy.ts
│   │   └── guards/
│   ├── users/
│   │   ├── users.module.ts
│   │   ├── users.service.ts
│   │   ├── users.controller.ts
│   │   ├── entities/
│   │   └── dto/
│   ├── common/
│   │   ├── guards/
│   │   ├── interceptors/
│   │   └── decorators/
│   ├── config/
│   └── app.module.ts
├── package.json
├── docker-compose.yml
└── .env.example
```

#### 1.2 Frontend Setup (Next.js)

**Location:** `packages/web/`

**Tasks:**

- [ ] Initialize Next.js 14 project with TypeScript
- [ ] Set up Tailwind CSS for styling
- [ ] Configure authentication context
- [ ] Create basic layout components
- [ ] Set up API client (Axios/React Query)
- [ ] Implement protected routes
- [ ] Create login/register pages

#### 1.3 Mobile App Setup (React Native)

**Location:** `packages/app/`

**Tasks:**

- [ ] Initialize React Native project with TypeScript
- [ ] Set up navigation (React Navigation)
- [ ] Configure authentication context
- [ ] Set up API client (Axios/React Query)
- [ ] Create basic screens and components
- [ ] Implement push notifications
- [ ] Set up development environment (iOS/Android)

#### 1.4 Shared State Management Setup (Zustand)

**Location:** `packages/store/`

**Tasks:**

- [ ] Initialize shared Zustand store package
- [ ] Create authentication store (shared between web and mobile)
- [ ] Create user store for profile management
- [ ] Create posts store for feed management
- [ ] Create notifications store
- [ ] Set up persistence layer for mobile
- [ ] Configure store synchronization between platforms
- [ ] Create store middleware for logging and debugging

**Key Files to Create:**

```
web/
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   └── register/
│   │   ├── (dashboard)/
│   │   │   ├── feed/
│   │   │   ├── profile/
│   │   │   └── notifications/
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── ui/
│   │   ├── layout/
│   │   └── forms/
│   ├── lib/
│   │   ├── api.ts
│   │   ├── auth.ts
│   │   └── utils.ts
│   ├── hooks/
│   └── types/
├── package.json
├── tailwind.config.js
└── next.config.js
```

#### 1.4 Mobile App Structure (React Native)

**Key Files to Create:**

````
app/
├── src/
│   ├── screens/
│   │   ├── auth/
│   │   │   ├── LoginScreen.tsx
│   │   │   └── RegisterScreen.tsx
│   │   ├── main/
│   │   │   ├── FeedScreen.tsx
│   │   │   ├── ProfileScreen.tsx
│   │   │   ├── NotificationsScreen.tsx
│   │   │   └── SearchScreen.tsx
│   │   └── index.ts
│   ├── components/
│   │   ├── ui/
│   │   ├── forms/
│   │   └── features/
│   ├── navigation/
│   │   ├── AppNavigator.tsx
│   │   ├── AuthNavigator.tsx
│   │   └── MainNavigator.tsx
│   ├── services/
│   │   ├── api.ts
│   │   ├── auth.ts
│   │   └── notifications.ts
│   ├── hooks/
│   ├── store/
│   └── types/
├── android/
├── ios/
├── package.json
├── metro.config.js
└── app.json
```

#### 1.5 Shared Store Structure (Zustand)

**Key Files to Create:**

```
packages/store/
├── src/
│   ├── auth/
│   │   ├── authStore.ts
│   │   └── authSlice.ts
│   ├── user/
│   │   ├── userStore.ts
│   │   └── userSlice.ts
│   ├── posts/
│   │   ├── postsStore.ts
│   │   └── postsSlice.ts
│   ├── notifications/
│   │   ├── notificationsStore.ts
│   │   └── notificationsSlice.ts
│   ├── middleware/
│   │   ├── persistence.ts
│   │   └── logger.ts
│   └── index.ts
├── package.json
└── tsconfig.json

### Phase 2: Core Features Implementation (Week 3-6)

#### 2.1 User Management & Profiles

**Backend Tasks:**

- [ ] Complete user CRUD operations
- [ ] Implement profile management
- [ ] Add profile picture upload
- [ ] Create user search functionality
- [ ] Add user validation and error handling

**Frontend Tasks:**

- [ ] Build user profile pages
- [ ] Create profile editing forms
- [ ] Implement user search interface
- [ ] Add profile picture upload component
- [ ] Create user cards and lists

**Mobile Tasks:**

- [ ] Create user profile screens
- [ ] Build profile editing forms
- [ ] Implement user search interface
- [ ] Add profile picture upload functionality
- [ ] Create user cards and lists

#### 2.2 Post System

**Backend Tasks:**

- [ ] Create post entity and relationships
- [ ] Implement post CRUD operations
- [ ] Add image upload for posts
- [ ] Create post validation
- [ ] Implement post pagination

**Frontend Tasks:**

- [ ] Build post creation form
- [ ] Create post feed component
- [ ] Implement post editing/deletion
- [ ] Add image upload for posts
- [ ] Create post detail pages

**Mobile Tasks:**

- [ ] Create post creation screen
- [ ] Build post feed screen
- [ ] Implement post editing/deletion
- [ ] Add image upload for posts
- [ ] Create post detail screen

#### 2.3 Follow System

**Backend Tasks:**

- [ ] Create follow entity
- [ ] Implement follow/unfollow logic
- [ ] Create follower/following endpoints
- [ ] Add follow validation

**Frontend Tasks:**

- [ ] Add follow/unfollow buttons
- [ ] Create followers/following lists
- [ ] Update user cards with follow status

**Mobile Tasks:**

- [ ] Add follow/unfollow buttons
- [ ] Create followers/following lists
- [ ] Update user cards with follow status

### Phase 3: Real-Time Features (Week 7-8)

#### 3.1 WebSocket Implementation

**Backend Tasks:**

- [ ] Set up WebSocket gateway
- [ ] Implement real-time notifications
- [ ] Add live post updates
- [ ] Create notification system
- [ ] Set up Redis for pub/sub

**Frontend Tasks:**

- [ ] Connect to WebSocket
- [ ] Implement real-time notifications
- [ ] Add live feed updates
- [ ] Create notification components

**Mobile Tasks:**

- [ ] Connect to WebSocket
- [ ] Implement real-time notifications
- [ ] Add live feed updates
- [ ] Create notification components
- [ ] Set up push notifications

#### 3.2 Feed Generation

**Backend Tasks:**

- [ ] Implement feed algorithm
- [ ] Add post ordering logic
- [ ] Optimize feed queries
- [ ] Add feed pagination

**Frontend Tasks:**

- [ ] Build infinite scroll feed
- [ ] Add feed filtering options
- [ ] Implement feed refresh

**Mobile Tasks:**

- [ ] Build infinite scroll feed
- [ ] Add feed filtering options
- [ ] Implement feed refresh
- [ ] Add pull-to-refresh functionality

### Phase 4: Advanced Features (Week 9-10)

#### 4.1 Search & Discovery

**Backend Tasks:**

- [ ] Implement full-text search
- [ ] Add search filters
- [ ] Optimize search performance
- [ ] Add search suggestions

**Frontend Tasks:**

- [ ] Create search interface
- [ ] Add search filters
- [ ] Implement search suggestions
- [ ] Add search history

**Mobile Tasks:**

- [ ] Create search interface
- [ ] Add search filters
- [ ] Implement search suggestions
- [ ] Add search history

#### 4.2 Performance Optimization

**Backend Tasks:**

- [ ] Add database indexing
- [ ] Implement caching (Redis)
- [ ] Optimize API responses
- [ ] Add rate limiting

**Frontend Tasks:**

- [ ] Implement image optimization
- [ ] Add lazy loading
- [ ] Optimize bundle size
- [ ] Add service worker

**Mobile Tasks:**

- [ ] Optimize image loading
- [ ] Implement lazy loading for lists
- [ ] Optimize bundle size
- [ ] Add offline support

### Phase 5: Testing & Quality Assurance (Week 11)

#### 5.1 Backend Testing

- [ ] Unit tests for services
- [ ] Integration tests for controllers
- [ ] E2E tests for API endpoints
- [ ] Database testing

#### 5.2 Frontend Testing

- [ ] Component testing (Jest/React Testing Library)
- [ ] Integration tests
- [ ] E2E tests (Playwright/Cypress)
- [ ] Accessibility testing

#### 5.3 Mobile Testing

- [ ] Component testing (Jest/React Native Testing Library)
- [ ] Integration tests
- [ ] E2E tests (Detox)
- [ ] Device testing (iOS/Android)

### Phase 6: Deployment & DevOps (Week 12)

#### 6.1 Containerization

- [ ] Create Dockerfiles for backend and web
- [ ] Set up docker-compose for development
- [ ] Configure production builds
- [ ] Set up mobile build pipeline

#### 6.2 CI/CD Pipeline

- [ ] Set up GitHub Actions
- [ ] Configure automated testing
- [ ] Set up deployment pipeline
- [ ] Add environment management

#### 6.3 Cloud Deployment

- [ ] Deploy backend to cloud platform
- [ ] Deploy frontend to Vercel/Netlify
- [ ] Configure domain and SSL
- [ ] Set up monitoring
- [ ] Deploy mobile app to App Store/Play Store

## Technical Stack

### Backend (Nest.js)

- **Framework:** Nest.js with TypeScript
- **Database:** PostgreSQL with TypeORM
- **Authentication:** JWT with Passport
- **File Storage:** AWS S3 or local storage
- **Real-time:** WebSocket with Socket.io
- **Caching:** Redis
- **Testing:** Jest, Supertest
- **Documentation:** Swagger/OpenAPI

### Frontend (Next.js)

- **Framework:** Next.js 14 with TypeScript
- **Styling:** Tailwind CSS
- **State Management:** Shared Zustand store from `@wave/store`
- **Data Fetching:** React Query/SWR
- **Real-time:** Socket.io client
- **Testing:** Jest, React Testing Library
- **Deployment:** Vercel

### Mobile (React Native)

- **Framework:** React Native with TypeScript
- **Navigation:** React Navigation
- **State Management:** Shared Zustand store from `@wave/store`
- **Data Fetching:** React Query/SWR
- **Real-time:** Socket.io client
- **Push Notifications:** Expo Notifications
- **Testing:** Jest, React Native Testing Library, Detox
- **Deployment:** Expo/App Store/Play Store

### Shared State Management (Zustand)

- **Framework:** Zustand with TypeScript
- **Architecture:** Modular stores with slices
- **Persistence:** AsyncStorage for mobile, localStorage for web
- **Middleware:** Logging, persistence, and synchronization
- **Cross-Platform:** Shared between web and mobile
- **Type Safety:** Full TypeScript support
- **Performance:** Lightweight and fast

### DevOps

- **Containerization:** Docker
- **CI/CD:** GitHub Actions
- **Cloud:** AWS/Vercel
- **Monitoring:** Sentry, LogRocket

## Database Schema

### Core Entities

```sql
-- Users table
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  bio TEXT,
  profile_picture_url VARCHAR(500),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Posts table
CREATE TABLE posts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  content TEXT NOT NULL,
  image_url VARCHAR(500),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Follows table
CREATE TABLE follows (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  follower_id UUID REFERENCES users(id) ON DELETE CASCADE,
  following_id UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(follower_id, following_id)
);

-- Notifications table
CREATE TABLE notifications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  type VARCHAR(50) NOT NULL,
  content TEXT NOT NULL,
  is_read BOOLEAN DEFAULT FALSE,
  related_id UUID,
  created_at TIMESTAMP DEFAULT NOW()
);
````

## API Endpoints

### Authentication

- `POST /auth/register` - User registration
- `POST /auth/login` - User login
- `POST /auth/refresh` - Refresh token
- `POST /auth/logout` - User logout

### Users

- `GET /users/profile` - Get current user profile
- `PUT /users/profile` - Update user profile
- `GET /users/:id` - Get user by ID
- `GET /users/search` - Search users
- `POST /users/:id/follow` - Follow user
- `DELETE /users/:id/follow` - Unfollow user
- `GET /users/:id/followers` - Get user followers
- `GET /users/:id/following` - Get user following

### Posts

- `GET /posts` - Get posts feed
- `POST /posts` - Create new post
- `GET /posts/:id` - Get post by ID
- `PUT /posts/:id` - Update post
- `DELETE /posts/:id` - Delete post
- `POST /posts/:id/like` - Like post
- `DELETE /posts/:id/like` - Unlike post

### Notifications

- `GET /notifications` - Get user notifications
- `PUT /notifications/:id/read` - Mark notification as read
- `PUT /notifications/read-all` - Mark all notifications as read

## Shared State Management Architecture

### Overview

The Wave platform uses a shared Zustand state management system across both web and mobile applications. This approach ensures consistency, reduces code duplication, and provides a unified state management experience.

### Store Structure

```
@wave/store/
├── auth/           # Authentication state
├── user/           # User profile and preferences
├── posts/          # Posts and feed management
├── notifications/  # Real-time notifications
└── middleware/     # Store middleware and utilities
```

### Key Benefits

- **Code Reuse**: Same state logic for web and mobile
- **Consistency**: Unified data flow across platforms
- **Type Safety**: Shared TypeScript interfaces
- **Performance**: Lightweight and efficient
- **Maintainability**: Single source of truth for state logic

### Implementation Strategy

1. **Shared Store Package**: Centralized state management
2. **Platform-Specific Persistence**: localStorage for web, AsyncStorage for mobile
3. **Middleware Integration**: Logging, persistence, and synchronization
4. **Type Safety**: Full TypeScript support with shared interfaces
5. **Performance Optimization**: Selective re-rendering and memoization

### Store Examples

#### Authentication Store

```typescript
// Shared between web and mobile
interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
  refreshToken: () => Promise<void>;
}
```

#### Posts Store

```typescript
// Shared feed management
interface PostsStore {
  posts: Post[];
  hasMore: boolean;
  isLoading: boolean;
  fetchPosts: () => Promise<void>;
  createPost: (post: CreatePostDto) => Promise<void>;
  likePost: (postId: string) => Promise<void>;
}
```

## Development Workflow

### Daily Development Process

1. **Morning Setup**

   - Pull latest changes
   - Start development environment
   - Review current tasks

2. **Development**

   - Work on assigned features
   - Write tests for new code
   - Commit changes frequently

3. **End of Day**
   - Push changes to feature branch
   - Update progress in project management tool
   - Plan next day's tasks

### Code Quality Standards

- **TypeScript:** Strict mode enabled
- **ESLint:** Configured for both frontend and backend
- **Prettier:** Consistent code formatting
- **Git Hooks:** Pre-commit hooks for linting and testing
- **Code Review:** All changes require review before merge

### Testing Strategy

- **Unit Tests:** 80%+ coverage required
- **Integration Tests:** All API endpoints
- **E2E Tests:** Critical user flows
- **Performance Tests:** Load testing for key endpoints

## Success Metrics

### Technical Metrics

- **Performance:** < 2s page load time
- **Uptime:** 99.9% availability
- **Test Coverage:** > 80%
- **Security:** No critical vulnerabilities

### Feature Metrics

- **User Registration:** Complete flow working
- **Post Creation:** Text and image posts
- **Real-time Updates:** WebSocket connections stable
- **Search:** Fast and accurate results
- **Mobile Responsive:** Works on all devices

## Next Steps

1. **Start with Phase 1:** Set up the basic project structure
2. **Follow the checklist:** Complete each task systematically
3. **Test frequently:** Don't wait until the end to test
4. **Document as you go:** Keep code and API documentation updated
5. **Deploy early:** Get feedback from real users

## Resources

### Documentation

- [Nest.js Documentation](https://docs.nestjs.com/)
- [Next.js Documentation](https://nextjs.org/docs)
- [TypeORM Documentation](https://typeorm.io/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

### Tools & Services

- [PostgreSQL](https://www.postgresql.org/)
- [Redis](https://redis.io/)
- [AWS S3](https://aws.amazon.com/s3/)
- [Vercel](https://vercel.com/)
- [GitHub Actions](https://github.com/features/actions)

---

**Remember:** This is a portfolio project designed to showcase your skills. Focus on clean code, good architecture, and demonstrating your understanding of modern development practices. Quality over quantity - it's better to have fewer features that work perfectly than many features that are buggy.

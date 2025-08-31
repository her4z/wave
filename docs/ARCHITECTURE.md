# Wave - Technical Architecture

## 🏗️ System Overview

Wave is a modern, scalable social networking platform built with a monorepo architecture using Next.js for the web frontend, React Native for the mobile frontend, and Nest.js for the backend API.

**Fixed Architecture**: WebSocket is part of the backend, and CDN connects to file storage.

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   Next.js App   │◄────────┤   Nest.js API   ├────────►│   PostgreSQL    │
│   (Web)         │         │   (Backend)     │         │   (Database)    │
└─────────────────┘         └─────────────────┘         └─────────────────┘
                                    │
                                    ├──────► Redis Cache
                                    │        (Session)
                                    │
                                    ├──────► File Storage ────► CDN
                                    │        (AWS S3)           (Static)
                                    │
                                    │
┌─────────────────┐                 │        ┌─────────────────┐
│ React Native    │◄────────────────┴────────┤   WebSocket     │
│   (Mobile)      │                          │   (Real-time)   │
└─────────────────┘                          └─────────────────┘
         │
         │
         ▼
┌─────────────────┐
│ Push Notif.     │
│   Service       │
└─────────────────┘
```

## 🎯 Architecture Principles

### 1. Separation of Concerns

- **Web Frontend**: UI/UX, state management, client-side validation
- **Mobile Frontend**: Native UI, mobile-specific features, offline support
- **Backend**: Business logic, data persistence, authentication
- **Database**: Data storage and retrieval
- **Cache**: Performance optimization and session management

### 2. Scalability

- Horizontal scaling capability
- Stateless API design
- Efficient database queries
- Caching strategies

### 3. Security

- JWT-based authentication
- Input validation and sanitization
- CORS configuration
- Rate limiting

### 4. Performance

- Server-side rendering (SSR)
- Image optimization
- Database indexing
- CDN integration

## 🏛️ Backend Architecture (Nest.js)

### Module Structure

```
src/
├── auth/                 # Authentication module
│   ├── auth.module.ts
│   ├── auth.service.ts
│   ├── auth.controller.ts
│   ├── jwt.strategy.ts
│   └── guards/
├── users/               # User management module
│   ├── users.module.ts
│   ├── users.service.ts
│   ├── users.controller.ts
│   ├── entities/
│   └── dto/
├── posts/               # Post management module
│   ├── posts.module.ts
│   ├── posts.service.ts
│   ├── posts.controller.ts
│   ├── entities/
│   └── dto/
├── notifications/       # Notification system
│   ├── notifications.module.ts
│   ├── notifications.service.ts
│   ├── notifications.controller.ts
│   └── entities/
├── common/             # Shared utilities
│   ├── guards/
│   ├── interceptors/
│   ├── decorators/
│   └── filters/
├── config/             # Configuration
├── database/           # Database configuration
└── app.module.ts       # Root module
```

### Key Components

#### 1. Authentication Module

```typescript
// JWT Strategy
@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor(private authService: AuthService) {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: process.env.JWT_SECRET,
    });
  }

  async validate(payload: any) {
    return this.authService.validateUser(payload.sub);
  }
}
```

#### 2. User Module

```typescript
// User Entity
@Entity("users")
export class User {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column({ unique: true })
  username: string;

  @Column({ unique: true })
  email: string;

  @Column()
  passwordHash: string;

  @OneToMany(() => Post, (post) => post.user)
  posts: Post[];

  @OneToMany(() => Follow, (follow) => follow.follower)
  following: Follow[];

  @OneToMany(() => Follow, (follow) => follow.following)
  followers: Follow[];
}
```

#### 3. WebSocket Gateway

```typescript
@WebSocketGateway({
  cors: {
    origin: process.env.FRONTEND_URL,
  },
})
export class AppGateway implements OnGatewayConnection, OnGatewayDisconnect {
  @WebSocketServer()
  server: Server;

  handleConnection(client: Socket) {
    // Handle new connection
  }

  handleDisconnect(client: Socket) {
    // Handle disconnection
  }

  @SubscribeMessage("joinRoom")
  handleJoinRoom(client: Socket, room: string) {
    client.join(room);
  }
}
```

## 🎨 Web Frontend Architecture (Next.js)

### App Router Structure

```
src/app/
├── (auth)/              # Authentication routes
│   ├── login/
│   │   └── page.tsx
│   └── register/
│       └── page.tsx
├── (dashboard)/         # Protected dashboard routes
│   ├── feed/
│   │   └── page.tsx
│   ├── profile/
│   │   └── page.tsx
│   └── notifications/
│       └── page.tsx
├── layout.tsx           # Root layout
└── page.tsx            # Home page
```

### Component Architecture

```
src/components/
├── ui/                  # Reusable UI components
│   ├── Button.tsx
│   ├── Input.tsx
│   ├── Modal.tsx
│   └── Card.tsx
├── layout/              # Layout components
│   ├── Header.tsx
│   ├── Sidebar.tsx
│   └── Footer.tsx
├── forms/               # Form components
│   ├── LoginForm.tsx
│   ├── RegisterForm.tsx
│   └── PostForm.tsx
└── features/            # Feature-specific components
    ├── PostCard.tsx
    ├── UserCard.tsx
    └── NotificationItem.tsx
```

### State Management

```typescript
// Shared Zustand Store (from @wave/store)
import { useAuthStore } from "@wave/store";

// The store is shared between web and mobile
interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
  updateUser: (user: User) => void;
}

// Usage in components
const { user, isAuthenticated, login } = useAuthStore();
```

## 📱 Mobile Frontend Architecture (React Native)

### Navigation Structure

```
src/navigation/
├── AppNavigator.tsx      # Root navigator
├── AuthNavigator.tsx     # Authentication flow
├── MainNavigator.tsx     # Main app tabs
└── types.ts             # Navigation types
```

### Screen Architecture

```
src/screens/
├── auth/
│   ├── LoginScreen.tsx
│   └── RegisterScreen.tsx
├── main/
│   ├── FeedScreen.tsx
│   ├── ProfileScreen.tsx
│   ├── NotificationsScreen.tsx
│   └── SearchScreen.tsx
└── index.ts
```

### State Management

```typescript
// Zustand Store (shared with web)
interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
  updateUser: (user: User) => void;
}

// Mobile-specific store
interface MobileStore {
  isOnline: boolean;
  pushToken: string | null;
  notifications: Notification[];
  setPushToken: (token: string) => void;
  addNotification: (notification: Notification) => void;
}
```

## 🔄 Shared State Management Architecture

### Overview

The Wave platform implements a shared Zustand state management system that works seamlessly across both web and mobile applications. This architecture ensures consistency, reduces code duplication, and provides a unified development experience.

### Store Structure

```
packages/store/
├── src/
│   ├── auth/
│   │   ├── authStore.ts      # Authentication state
│   │   └── authSlice.ts      # Auth actions and reducers
│   ├── user/
│   │   ├── userStore.ts      # User profile state
│   │   └── userSlice.ts      # User actions
│   ├── posts/
│   │   ├── postsStore.ts     # Posts and feed state
│   │   └── postsSlice.ts     # Posts actions
│   ├── notifications/
│   │   ├── notificationsStore.ts
│   │   └── notificationsSlice.ts
│   ├── middleware/
│   │   ├── persistence.ts    # Platform-specific persistence
│   │   └── logger.ts         # Store logging
│   └── index.ts              # Main exports
```

### Key Features

- **Cross-Platform**: Same store logic for web and mobile
- **Type Safety**: Full TypeScript support with shared interfaces
- **Persistence**: Platform-specific storage (localStorage/AsyncStorage)
- **Middleware**: Logging, persistence, and synchronization
- **Performance**: Lightweight and efficient state updates

### Implementation Example

```typescript
// packages/store/src/auth/authStore.ts
import { create } from "zustand";
import { persist } from "zustand/middleware";
import { AuthState, LoginCredentials } from "@wave/shared";

export const useAuthStore = create<AuthState>()(
  persist(
    (set, get) => ({
      user: null,
      isAuthenticated: false,
      isLoading: false,
      error: null,

      login: async (credentials: LoginCredentials) => {
        set({ isLoading: true, error: null });
        try {
          // API call logic
          const response = await authAPI.login(credentials);
          set({
            user: response.user,
            isAuthenticated: true,
            isLoading: false,
          });
        } catch (error) {
          set({ error: error.message, isLoading: false });
        }
      },

      logout: () => {
        set({ user: null, isAuthenticated: false });
      },
    }),
    {
      name: "auth-storage",
      // Platform-specific storage
      storage: createJSONStorage(() =>
        typeof window !== "undefined" ? localStorage : AsyncStorage
      ),
    }
  )
);
```

### Usage Across Platforms

```typescript
// Web (Next.js)
import { useAuthStore } from "@wave/store";

export function LoginForm() {
  const { login, isLoading } = useAuthStore();
  // Component logic
}

// Mobile (React Native)
import { useAuthStore } from "@wave/store";

export function LoginScreen() {
  const { login, isLoading } = useAuthStore();
  // Screen logic
}
```

## 🗄️ Database Design

### Entity Relationships

```sql
-- Users table (Core entity)
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

-- Follows table (Many-to-many relationship)
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
```

### Indexes for Performance

```sql
-- User search optimization
CREATE INDEX idx_users_username ON users(username);
CREATE INDEX idx_users_email ON users(email);

-- Post feed optimization
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_created_at ON posts(created_at DESC);

-- Follow system optimization
CREATE INDEX idx_follows_follower_id ON follows(follower_id);
CREATE INDEX idx_follows_following_id ON follows(following_id);

-- Notification optimization
CREATE INDEX idx_notifications_user_id ON notifications(user_id);
CREATE INDEX idx_notifications_created_at ON notifications(created_at DESC);
```

## 🔄 Data Flow

### 1. User Authentication Flow

```
1. User submits login form
2. Frontend sends credentials to /auth/login
3. Backend validates credentials
4. Backend generates JWT token
5. Frontend stores token in localStorage
6. Frontend includes token in subsequent requests
```

### 2. Post Creation Flow

```
1. User creates post in frontend
2. Frontend sends post data to /posts
3. Backend validates and saves post
4. Backend emits WebSocket event
5. Connected clients receive real-time update
6. Frontend updates UI immediately
```

### 3. Real-time Notification Flow

```
1. User performs action (like, follow, etc.)
2. Backend processes action
3. Backend creates notification record
4. Backend emits WebSocket event
5. Target user's frontend receives notification
6. Frontend updates notification UI
```

## 🚀 Performance Optimization

### Backend Optimizations

1. **Database Query Optimization**

   - Use eager loading for relationships
   - Implement pagination
   - Add database indexes
   - Use query optimization

2. **Caching Strategy**

   - Redis for session storage
   - Cache frequently accessed data
   - Implement cache invalidation

3. **API Response Optimization**
   - Use DTOs for response transformation
   - Implement response compression
   - Add rate limiting

### Web Frontend Optimizations

1. **Next.js Optimizations**

   - Server-side rendering (SSR)
   - Static site generation (SSG)
   - Image optimization
   - Code splitting

2. **React Optimizations**

   - Memoization with React.memo
   - UseCallback and useMemo hooks
   - Virtual scrolling for large lists
   - Lazy loading components

3. **Bundle Optimization**
   - Tree shaking
   - Code splitting
   - Dynamic imports
   - Bundle analysis

### Mobile Frontend Optimizations

1. **React Native Optimizations**

   - FlatList for efficient list rendering
   - Image caching and optimization
   - Lazy loading for screens
   - Memory management

2. **Performance Optimizations**

   - UseCallback and useMemo hooks
   - React.memo for component optimization
   - Virtual scrolling for large lists
   - Background task optimization

3. **Bundle Optimization**
   - Hermes engine for better performance
   - Code splitting with dynamic imports
   - Asset optimization
   - Bundle analysis

## 🔒 Security Considerations

### Authentication & Authorization

- JWT tokens with expiration
- Refresh token rotation
- Role-based access control
- Password hashing with bcrypt

### Data Protection

- Input validation and sanitization
- SQL injection prevention
- XSS protection
- CSRF protection

### API Security

- Rate limiting
- CORS configuration
- Request size limits
- Error handling without information leakage

## 📊 Monitoring & Logging

### Application Monitoring

- Error tracking with Sentry
- Performance monitoring
- User analytics
- Server health checks

### Logging Strategy

- Structured logging
- Log levels (error, warn, info, debug)
- Log aggregation
- Audit trails for user actions

## 🐳 Deployment Architecture

### Development Environment

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   Next.js Dev   │◄────────┤   Nest.js Dev   ├────────►│   PostgreSQL    │
│   localhost:3000│         │   localhost:3001│         │   localhost:5432│
└─────────────────┘         └─────────────────┘         └─────────────────┘
                                    │
                                    │
                                    ▼
                    ┌─────────────────┐    ┌─────────────────┐
                    │   Redis Dev     │    │   Expo Dev      │
                    │   localhost:6379│    │   localhost:19000│
                    └─────────────────┘    └─────────────────┘
                                    │
                                    │
                                    ▼
┌─────────────────┐         ┌─────────────────┐
│ React Native    │◄────────┤   WebSocket     │
│   localhost:8081│         │   Development   │
└─────────────────┘         └─────────────────┘
```

### Production Environment

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   Vercel        │◄────────┤   AWS EC2       ├────────►│   AWS RDS       │
│   (Web)         │         │   (Backend)     │         │   (Database)    │
└─────────────────┘         └─────────────────┘         └─────────────────┘
                                    │
                                    │
                                    ▼
                    ┌─────────────────┐    ┌─────────────────┐
                    │   Redis         │    │   AWS S3        │
                    │   (Cache)       │    │   (Storage)     │
                    └─────────────────┘    └─────────────────┘
                                    │
                                    │
                                    ▼
┌─────────────────┐         ┌─────────────────┐
│ App Store/      │◄────────┤   WebSocket     │
│ Play Store      │         │   (Real-time)   │
│   (Mobile)      │         │                 │
└─────────────────┘         └─────────────────┘
         │
         │
         ▼
┌─────────────────┐    ┌─────────────────┐
│ Push Notif.     │    │   CDN           │
│   Service       │    │   (Static)      │
└─────────────────┘    └─────────────────┘
```

## 🔄 CI/CD Pipeline

### GitHub Actions Workflow

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
      - name: Install dependencies
        run: npm ci
      - name: Run tests
        run: npm test
      - name: Run linting
        run: npm run lint

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy to production
        run: |
          # Deployment steps
```

## 📈 Scalability Considerations

### Horizontal Scaling

- Load balancer for multiple backend instances
- Database read replicas
- CDN for static assets
- Microservices architecture (future)

### Vertical Scaling

- Optimize database queries
- Implement caching strategies
- Use connection pooling
- Monitor resource usage

---

**This architecture provides a solid foundation for a scalable, maintainable social platform that demonstrates modern software engineering practices.**

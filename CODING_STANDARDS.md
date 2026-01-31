# Coding Standards - Gateway Global AI Platform

## Overview

This document defines the coding standards for all TypeScript development within the Gateway Global AI organization. Adherence to these standards ensures code quality, maintainability, and consistency across the platform.

## TypeScript Standards

### General Rules

1. **Strict Mode**: Always enable TypeScript strict mode
   ```json
   // tsconfig.json
   {
     "compilerOptions": {
       "strict": true,
       "noImplicitAny": true,
       "strictNullChecks": true,
       "strictFunctionTypes": true,
       "strictBindCallApply": true,
       "strictPropertyInitialization": true,
       "noImplicitThis": true,
       "alwaysStrict": true
     }
   }
   ```

2. **Type Safety**: Never use `any` type
   ```typescript
   // ❌ Bad
   function processData(data: any): any {
     return data.value;
   }

   // ✅ Good
   function processData(data: unknown): string {
     if (typeof data === 'object' && data !== null && 'value' in data) {
       const typedData = data as { value: string };
       return typedData.value;
     }
     throw new Error('Invalid data format');
   }
   ```

3. **Explicit Return Types**: Always declare return types for functions
   ```typescript
   // ❌ Bad
   function getUserName(id: string) {
     return database.users.find(id).name;
   }

   // ✅ Good
   function getUserName(id: string): string {
     return database.users.find(id).name;
   }

   async function fetchUser(id: string): Promise<User> {
     const response = await api.get(`/users/${id}`);
     return response.data;
   }
   ```

### Naming Conventions

**Variables and Functions:**
```typescript
// camelCase for variables and functions
const userName = 'John';
const userAge = 30;

function getUserInfo(): UserInfo {
  return { name: userName, age: userAge };
}
```

**Classes and Interfaces:**
```typescript
// PascalCase for classes, interfaces, types, enums
class UserService {
  // ...
}

interface User {
  id: string;
  name: string;
}

type UserId = string;

enum UserRole {
  Admin = 'ADMIN',
  User = 'USER',
  Guest = 'GUEST'
}
```

**Constants:**
```typescript
// UPPER_SNAKE_CASE for constants
const MAX_RETRY_ATTEMPTS = 3;
const DEFAULT_TIMEOUT_MS = 5000;
const API_BASE_URL = 'https://api.gateway-global-ai.com';
```

**Private Members:**
```typescript
// Use private keyword for class members
class UserManager {
  private users: Map<string, User>;
  private readonly maxUsers: number;

  constructor(maxUsers: number) {
    this.users = new Map();
    this.maxUsers = maxUsers;
  }

  public addUser(user: User): void {
    this.validateUser(user);
    this.users.set(user.id, user);
  }

  private validateUser(user: User): void {
    // Validation logic
  }
}
```

### Type Definitions

**Prefer Interfaces for Objects:**
```typescript
// ✅ Good - Use interface for object shapes
interface User {
  id: string;
  name: string;
  email: string;
}

// Use type for unions, intersections, and primitives
type UserId = string;
type UserRole = 'admin' | 'user' | 'guest';
type AdminUser = User & { role: 'admin'; permissions: string[] };
```

**Use Zod for Runtime Validation:**
```typescript
import { z } from 'zod';

// Define schema
const UserSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1).max(100),
  email: z.string().email(),
  age: z.number().min(0).max(150).optional()
});

// Infer TypeScript type from schema
type User = z.infer<typeof UserSchema>;

// Validate at runtime
function createUser(data: unknown): User {
  return UserSchema.parse(data);
}
```

### Error Handling

**Custom Error Classes:**
```typescript
// Define custom error classes
export class ValidationError extends Error {
  constructor(
    message: string,
    public readonly field?: string
  ) {
    super(message);
    this.name = 'ValidationError';
  }
}

export class NotFoundError extends Error {
  constructor(
    message: string,
    public readonly resource: string,
    public readonly id: string
  ) {
    super(message);
    this.name = 'NotFoundError';
  }
}
```

**Error Handling Pattern:**
```typescript
// ✅ Good - Proper error handling
async function getUserById(id: string): Promise<User> {
  try {
    const user = await database.users.findById(id);
    
    if (!user) {
      throw new NotFoundError(
        `User not found`,
        'User',
        id
      );
    }
    
    return user;
  } catch (error) {
    if (error instanceof NotFoundError) {
      throw error;
    }
    
    // Log unexpected errors
    logger.error('Unexpected error fetching user', { id, error });
    throw new Error('Failed to fetch user');
  }
}
```

### Async/Await

**Always use async/await over Promise chains:**
```typescript
// ❌ Bad
function fetchUserData(id: string): Promise<UserData> {
  return fetchUser(id)
    .then(user => fetchUserProfile(user.profileId))
    .then(profile => ({ user, profile }))
    .catch(error => {
      logger.error('Error fetching user data', error);
      throw error;
    });
}

// ✅ Good
async function fetchUserData(id: string): Promise<UserData> {
  try {
    const user = await fetchUser(id);
    const profile = await fetchUserProfile(user.profileId);
    return { user, profile };
  } catch (error) {
    logger.error('Error fetching user data', error);
    throw error;
  }
}
```

### Null and Undefined

**Explicit null checks:**
```typescript
// ✅ Good
function getUserName(user: User | null): string {
  if (user === null) {
    return 'Anonymous';
  }
  return user.name;
}

// Optional chaining
const userName = user?.profile?.name ?? 'Anonymous';

// Nullish coalescing
const port = config.port ?? 3000;
```

## React Standards

### Component Structure

**Functional Components with TypeScript:**
```typescript
import { FC, useState, useEffect } from 'react';

interface UserCardProps {
  userId: string;
  onSelect?: (userId: string) => void;
}

export const UserCard: FC<UserCardProps> = ({ userId, onSelect }) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState<boolean>(true);

  useEffect(() => {
    const fetchUser = async (): Promise<void> => {
      try {
        const userData = await getUserById(userId);
        setUser(userData);
      } catch (error) {
        logger.error('Failed to fetch user', { userId, error });
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [userId]);

  if (loading) {
    return <Loader />;
  }

  if (!user) {
    return <div>User not found</div>;
  }

  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      {onSelect && (
        <button onClick={() => onSelect(userId)}>
          Select User
        </button>
      )}
    </div>
  );
};
```

### Custom Hooks

**Reusable hooks with TypeScript:**
```typescript
import { useState, useEffect } from 'react';

interface UseApiOptions<T> {
  initialData?: T;
  onError?: (error: Error) => void;
}

interface UseApiReturn<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
  refetch: () => Promise<void>;
}

export function useApi<T>(
  fetcher: () => Promise<T>,
  options: UseApiOptions<T> = {}
): UseApiReturn<T> {
  const [data, setData] = useState<T | null>(options.initialData ?? null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<Error | null>(null);

  const fetchData = async (): Promise<void> => {
    try {
      setLoading(true);
      setError(null);
      const result = await fetcher();
      setData(result);
    } catch (err) {
      const error = err as Error;
      setError(error);
      options.onError?.(error);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  return { data, loading, error, refetch: fetchData };
}
```

### State Management

**Zustand Store:**
```typescript
import { create } from 'zustand';

interface UserState {
  user: User | null;
  isAuthenticated: boolean;
  login: (credentials: Credentials) => Promise<void>;
  logout: () => void;
}

export const useUserStore = create<UserState>((set) => ({
  user: null,
  isAuthenticated: false,
  
  login: async (credentials) => {
    const user = await authService.login(credentials);
    set({ user, isAuthenticated: true });
  },
  
  logout: () => {
    set({ user: null, isAuthenticated: false });
  }
}));
```

## File Organization

### File Structure
```
component-name/
├── ComponentName.tsx          # Component implementation
├── ComponentName.test.tsx     # Component tests
├── ComponentName.types.ts     # Type definitions
├── ComponentName.styles.ts    # Styled components (if used)
└── index.ts                   # Barrel export
```

### Barrel Exports
```typescript
// index.ts
export { ComponentName } from './ComponentName';
export type { ComponentNameProps } from './ComponentName.types';
```

## Code Documentation

### JSDoc Comments

**Functions and Methods:**
```typescript
/**
 * Fetches a user by their unique identifier
 * 
 * @param userId - The unique identifier of the user
 * @returns Promise resolving to the user object
 * @throws {NotFoundError} When user doesn't exist
 * @throws {ValidationError} When userId is invalid
 * 
 * @example
 * ```typescript
 * const user = await getUserById('123e4567-e89b-12d3-a456-426614174000');
 * console.log(user.name);
 * ```
 */
async function getUserById(userId: string): Promise<User> {
  // Implementation
}
```

**Interfaces and Types:**
```typescript
/**
 * Represents a user in the system
 */
interface User {
  /** Unique identifier (UUID v4) */
  id: string;
  
  /** Full name of the user */
  name: string;
  
  /** Email address (must be unique) */
  email: string;
  
  /** Optional profile picture URL */
  avatarUrl?: string;
}
```

## Testing Standards

### Unit Tests with Vitest

```typescript
import { describe, it, expect, beforeEach, vi } from 'vitest';
import { UserService } from './UserService';

describe('UserService', () => {
  let userService: UserService;

  beforeEach(() => {
    userService = new UserService();
  });

  describe('getUserById', () => {
    it('should return user when found', async () => {
      const userId = 'test-id';
      const mockUser = { id: userId, name: 'Test User' };
      
      vi.spyOn(database.users, 'findById').mockResolvedValue(mockUser);
      
      const result = await userService.getUserById(userId);
      
      expect(result).toEqual(mockUser);
    });

    it('should throw NotFoundError when user does not exist', async () => {
      const userId = 'non-existent';
      
      vi.spyOn(database.users, 'findById').mockResolvedValue(null);
      
      await expect(userService.getUserById(userId)).rejects.toThrow(
        NotFoundError
      );
    });
  });
});
```

### Component Tests

```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import { UserCard } from './UserCard';

describe('UserCard', () => {
  it('should render user information', async () => {
    render(<UserCard userId="test-id" />);
    
    expect(await screen.findByText('Test User')).toBeInTheDocument();
    expect(screen.getByText('test@example.com')).toBeInTheDocument();
  });

  it('should call onSelect when button is clicked', async () => {
    const onSelect = vi.fn();
    render(<UserCard userId="test-id" onSelect={onSelect} />);
    
    const button = await screen.findByRole('button', { name: /select user/i });
    fireEvent.click(button);
    
    expect(onSelect).toHaveBeenCalledWith('test-id');
  });
});
```

## ESLint Configuration

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "rules": {
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/explicit-function-return-type": "warn",
    "@typescript-eslint/no-unused-vars": ["error", { 
      "argsIgnorePattern": "^_" 
    }],
    "@typescript-eslint/no-floating-promises": "error",
    "react/react-in-jsx-scope": "off",
    "react/prop-types": "off"
  }
}
```

## Prettier Configuration

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "always"
}
```

## Git Commit Standards

### Conventional Commits

Format: `<type>(<scope>): <description>`

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(auth): add social login support
fix(api): handle null user in getUserById
docs(readme): update installation instructions
test(user-service): add tests for edge cases
refactor(components): extract common button logic
```

## Performance Best Practices

1. **Memoization in React:**
   ```typescript
   import { useMemo, useCallback } from 'react';

   const expensiveValue = useMemo(() => {
     return computeExpensiveValue(data);
   }, [data]);

   const handleClick = useCallback(() => {
     doSomething(id);
   }, [id]);
   ```

2. **Lazy Loading:**
   ```typescript
   import { lazy, Suspense } from 'react';

   const HeavyComponent = lazy(() => import('./HeavyComponent'));

   function App() {
     return (
       <Suspense fallback={<Loader />}>
         <HeavyComponent />
       </Suspense>
     );
   }
   ```

3. **Debouncing:**
   ```typescript
   import { debounce } from 'lodash-es';

   const debouncedSearch = debounce(async (query: string) => {
     const results = await searchApi(query);
     setResults(results);
   }, 300);
   ```

## Security Best Practices

1. **Input Validation:**
   ```typescript
   import { z } from 'zod';

   const UserInputSchema = z.object({
     name: z.string().min(1).max(100),
     email: z.string().email(),
     age: z.number().int().min(0).max(150)
   });

   function validateUserInput(data: unknown): UserInput {
     return UserInputSchema.parse(data);
   }
   ```

2. **SQL Injection Prevention:**
   ```typescript
   // ✅ Good - Using ORM with parameterized queries
   const user = await prisma.user.findFirst({
     where: { email: userEmail }
   });

   // ❌ Bad - Raw SQL with string interpolation
   const user = await db.raw(`SELECT * FROM users WHERE email = '${userEmail}'`);
   ```

3. **XSS Prevention:**
   ```typescript
   // React automatically escapes values
   const UserProfile = ({ userName }: { userName: string }) => {
     return <div>{userName}</div>; // Safe - auto-escaped
   };

   // For dangerouslySetInnerHTML, sanitize first
   import DOMPurify from 'dompurify';

   const sanitizedHtml = DOMPurify.sanitize(userContent);
   return <div dangerouslySetInnerHTML={{ __html: sanitizedHtml }} />;
   ```

## Code Review Checklist

- [ ] Code follows TypeScript strict mode
- [ ] No `any` types used
- [ ] All functions have explicit return types
- [ ] Proper error handling implemented
- [ ] Input validation with Zod
- [ ] Tests written and passing
- [ ] JSDoc comments for public APIs
- [ ] No console.log statements (use logger)
- [ ] Security best practices followed
- [ ] Performance considerations addressed
- [ ] Accessibility standards met (for UI)
- [ ] Mobile responsive (for UI)
- [ ] Follows naming conventions
- [ ] Conventional commit message

**Version:** 1.0.0  
**Last Updated:** 2026-01-31

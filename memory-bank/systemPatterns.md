# System Patterns: Architecture & Coding Standards

**Last Updated**: [DATE] - Update when you establish new patterns

> **IMPORTANT**: This is a TEMPLATE. Replace with YOUR project's actual patterns as they emerge.

## How to Use This File

1. **Don't fill this out upfront** - Let patterns emerge naturally as you build
2. **Document what works** - When you solve a problem well, document the pattern here
3. **Evolve over time** - Update as your understanding deepens
4. **Keep it practical** - Focus on patterns you actually use, not theoretical best practices

## Architectural Principles

### [Your Core Principle #1]
[Describe the principle and why it matters for your project]

Example:
> **Mobile-First Design**
> - Touch targets minimum 44x44px (Apple HIG)
> - Swipe gestures for common actions
> - Offline-first data layer for reliability
> - Test on actual devices, not just browser DevTools

### [Your Core Principle #2]
[Describe the principle]

Example:
> **Data-First Design**
> - Define database schema and TypeScript interfaces BEFORE building UI
> - Data structures drive component design, not the reverse
> - Validate data at boundaries (API input, form submission)

### [Your Core Principle #3]
[Your principle]

Common architectural principles to consider:
- **API-First**: Design API before implementation
- **Test-Driven**: Write tests before code
- **Domain-Driven**: Organize by business domains, not technical layers
- **Microservices**: Independent services vs monolith
- **Serverless**: Event-driven, stateless functions
- **JAMstack**: Static sites with dynamic APIs

## Database Patterns

### [Your Database Pattern #1]
```sql
-- Example pattern with actual code

-- Example: Soft delete pattern
ALTER TABLE users ADD COLUMN deleted_at TIMESTAMP NULL;
CREATE INDEX idx_users_active ON users(id) WHERE deleted_at IS NULL;

-- Query only active records
SELECT * FROM users WHERE deleted_at IS NULL;
```

### [Your Database Pattern #2]
[Describe when and how to use this pattern]

Common database patterns:
- **Audit Logs**: Track who changed what and when
- **Soft Deletes**: Mark as deleted instead of removing
- **UUIDs vs Auto-increment**: Trade-offs for primary keys
- **JSONB Fields**: When to use vs separate tables
- **Indexing Strategy**: What columns to index
- **Migrations**: How you version schema changes

## Component/Code Patterns

### [Your Component Pattern #1]
```typescript
// Example of your pattern with actual code

// Example: Result type for error handling
type Result<T> = 
  | { success: true; data: T }
  | { success: false; error: string };

async function fetchUser(id: string): Promise<Result<User>> {
  try {
    const response = await api.get(`/users/${id}`);
    return { success: true, data: response.data };
  } catch (error) {
    return { success: false, error: error.message };
  }
}

// Usage
const result = await fetchUser('123');
if (!result.success) {
  console.error(result.error);
  return;
}
// TypeScript knows result.data exists here
const user = result.data;
```

### [Your Component Pattern #2]
[Describe the pattern]

Common code patterns:
- **Component Structure**: Functional vs class components
- **State Management**: Redux, Zustand, Context, props drilling
- **Error Handling**: Try/catch, Result types, error boundaries
- **Loading States**: Suspense, skeleton screens, spinners
- **Form Handling**: Controlled vs uncontrolled, validation
- **API Calls**: Where they live (components, hooks, services)

## Authentication & Authorization Patterns

### [Your Auth Pattern]
[Describe how authentication works in your app]

Example:
> **JWT-based Authentication**
> - Server issues JWT on login
> - Client stores in httpOnly cookie (not localStorage)
> - Include in Authorization header for API calls
> - Refresh token rotation every 7 days
> - Role-based access control (RBAC) for permissions

## Code Quality Standards

### TypeScript/Type Safety
```typescript
// ✅ DO: Explicit types
interface User {
  id: string;
  email: string;
  name: string | null; // Explicit null handling
}

// ❌ DON'T: Using 'any'
const data: any = await fetchData(); // Loses all type safety

// ✅ DO: Type narrowing
const data: unknown = await fetchData();
if (isUser(data)) {
  // TypeScript knows it's User here
  console.log(data.email);
}
```

### [Your Code Standard #2]
[Your standard]

Common standards:
- **Naming Conventions**: camelCase, PascalCase, SCREAMING_SNAKE_CASE
- **File Organization**: Feature folders vs layer folders
- **Import Order**: External → internal → relative
- **Comments**: When to write them, JSDoc standards
- **Error Messages**: User-friendly vs developer-friendly

## Testing Standards

### What to Test
1. **[Category 1]**: [Description - e.g., "Business logic functions"]
2. **[Category 2]**: [Description - e.g., "Critical user workflows"]
3. **[Category 3]**: [Description - e.g., "API integration points"]

### What NOT to Test
1. **[Category 1]**: [Description - e.g., "Third-party library internals"]
2. **[Category 2]**: [Description - e.g., "Simple getters/setters"]
3. **[Category 3]**: [Description - e.g., "UI styling (use visual regression)"]

### Testing Approach
```typescript
// Example test pattern you use
describe('User Service', () => {
  it('should create a new user', async () => {
    const result = await userService.create({
      email: 'test@example.com',
      name: 'Test User'
    });
    
    expect(result.success).toBe(true);
    if (result.success) {
      expect(result.data.email).toBe('test@example.com');
    }
  });
});
```

## Performance Patterns

### [Your Performance Pattern]
[Describe how you optimize performance]

Examples:
- **Code Splitting**: Route-based chunks with lazy loading
- **Image Optimization**: WebP format, responsive sizes, lazy loading
- **Caching Strategy**: Browser cache, CDN cache, API cache
- **Database Queries**: N+1 problem solutions, eager loading
- **Bundle Size**: Tree shaking, analyzing with webpack-bundle-analyzer

## Deployment Patterns

### [Your Deployment Pattern]
[Describe your deployment process]

Example:
> **CI/CD Pipeline**
> 1. Push to `main` branch
> 2. GitHub Actions runs tests
> 3. If tests pass, build production bundle
> 4. Deploy to Vercel
> 5. Run smoke tests on production
> 6. Send Slack notification

### Environment Strategy
- **Development**: [How you run locally]
- **Staging**: [Your pre-production environment]
- **Production**: [Your live environment]

## Anti-Patterns (What NOT to Do)

Document mistakes you've made or want to avoid:

### ❌ [Anti-Pattern #1]
**Problem**: [What's wrong with this approach]  
**Why It's Bad**: [The consequences]  
**Instead Do**: [The correct approach]

Example:
> ❌ **Storing Secrets in Frontend Code**
> **Problem**: API keys committed to git or bundled in client JavaScript
> **Why It's Bad**: Anyone can steal your keys and rack up charges
> **Instead Do**: Use environment variables, keep secrets server-side only

### ❌ [Anti-Pattern #2]
[Your anti-pattern]

Common anti-patterns:
- **Premature Optimization**: Optimizing before measuring
- **Over-Engineering**: Building for scale you don't have
- **God Objects**: Classes/components that do too much
- **Tight Coupling**: Can't change one thing without breaking others
- **Magic Numbers**: Hardcoded values without explanation
- **Copy-Paste Code**: Duplication instead of abstraction

## Decision Framework

When facing architectural decisions, ask:

1. **Does this solve a real problem?** (Not speculative future-proofing)
2. **Is this the simplest solution?** (YAGNI - You Aren't Gonna Need It)
3. **Can I change it later?** (Avoid irreversible decisions)
4. **Does it fit existing patterns?** (Consistency matters)
5. **Is it documented?** (Future you will forget why)

## Resources & References

Link to external resources that inform your patterns:

- [Resource 1]: [URL and why it's relevant]
- [Resource 2]: [URL and why it's relevant]
- [Internal Doc]: [Link to team wiki, ADRs, etc.]

Examples:
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html): Official TS reference
- [12 Factor App](https://12factor.net/): Principles for SaaS applications
- [React Docs](https://react.dev/): Official React documentation

---

## Remember

1. **Patterns emerge from practice** - Don't copy blindly, discover what works for YOU
2. **Update as you learn** - This file should evolve with your project
3. **Be specific** - Generic advice helps less than concrete examples
4. **Question everything** - Including these patterns as your project matures
5. **Simplicity wins** - The best pattern is often the simplest one that works

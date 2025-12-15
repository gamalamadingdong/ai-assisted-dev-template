# System Patterns: Architecture & Coding Standards

**Last Updated**: December 15, 2025

## Architectural Principles

### 1. Mobile-First, Always
- Touch targets minimum 44x44px
- Swipe gestures for navigation
- Optimistic UI updates
- Offline-first data layer
- Responsive breakpoints: mobile (default) → tablet (768px) → desktop (1024px)

### 2. Multi-Tenant Isolation
- Every table has `business_id` foreign key
- Row Level Security (RLS) enforces isolation
- No cross-tenant data leakage possible
- Business context from auth claims

### 3. Data-First Design
**"Bad programmers worry about code. Good programmers worry about data structures"**

Always define:
1. Database entities and relationships FIRST
2. TypeScript interfaces matching DB schema
3. Business logic and invariants
4. Then build UI components

### 4. Simplicity Over Cleverness
- YAGNI: Reject "future-proofing"
- Direct functions over design patterns
- No abstractions until duplication proven
- Challenge complexity-introducing requests

## Database Patterns

### Multi-Tenant Schema Pattern
```sql
-- Every business entity table
CREATE TABLE service_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
  -- ... business-specific fields
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

-- RLS Policy Template
CREATE POLICY "Users can access own business data"
  ON service_items
  FOR ALL
  USING (business_id = current_setting('app.current_business_id')::uuid);
```

### Audit Trail Pattern
```sql
-- Track all business-critical operations
CREATE TABLE audit_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  business_id UUID NOT NULL REFERENCES businesses(id),
  user_id UUID NOT NULL REFERENCES auth.users(id),
  action TEXT NOT NULL, -- 'create', 'update', 'delete'
  entity_type TEXT NOT NULL, -- 'job', 'client', 'worker'
  entity_id UUID NOT NULL,
  changes JSONB, -- old/new values
  created_at TIMESTAMPTZ DEFAULT now()
);
```

### Status Workflow Pattern
```sql
-- Configurable per business type
CREATE TYPE job_status AS ENUM (
  'draft',
  'scheduled',
  'in_progress',
  'completed',
  'cancelled'
);

-- Different industries have different flows:
-- HVAC: draft → scheduled → in_progress → completed
-- Cleaning: scheduled → in_progress → completed (no draft)
```

## Component Patterns

### Business Config Adaptation
```typescript
interface BusinessConfig {
  businessType: 'hvac' | 'cleaning' | 'personal_care';
  terminology: {
    serviceItem: 'job' | 'appointment' | 'booking';
    worker: 'technician' | 'cleaner' | 'stylist';
    // ... configurable terms
  };
  features: {
    equipmentTracking: boolean;
    recurringSchedules: boolean;
    // ... feature toggles
  };
}

// Components adapt based on config
function ServiceItemCard({ item, config }: Props) {
  const itemLabel = config.terminology.serviceItem;
  // ... render accordingly
}
```

### Mobile Interaction Pattern
```typescript
// Touch-optimized list item
<div
  className="touch-item" // min-h-[44px] active:bg-gray-100
  onTouchStart={handleTouchStart}
  onClick={handleClick}
>
  {/* Swipe-to-delete gesture */}
</div>

// Pull-to-refresh pattern
const [isPulling, setIsPulling] = useState(false);
// ... touch gesture handling
```

### Offline-First Pattern
```typescript
// Optimistic update with rollback
const updateServiceItem = async (id: string, updates: Partial<ServiceItem>) => {
  // 1. Update local state immediately
  setServiceItem(prev => ({ ...prev, ...updates }));
  
  try {
    // 2. Sync to server
    await supabase.from('service_items').update(updates).eq('id', id);
  } catch (error) {
    // 3. Rollback on failure
    setServiceItem(prev => prev); // restore previous state
    toast.error('Update failed. Check connection.');
  }
};
```

## Authentication Patterns

### Invite-Based Onboarding
1. Business owner creates invite (email + role)
2. Edge function sends email via Resend
3. Recipient clicks link → auto-creates account
4. User assigned to business with specified role
5. No manual password creation required

### Role-Based Access Control
```typescript
enum UserRole {
  OWNER = 7,      // Full access
  ADMIN = 6,      // Manage users, settings
  MANAGER = 5,    // Assign work, view reports
  SUPERVISOR = 4, // Oversee workers
  WORKER = 3,     // Complete assigned work
  CLIENT = 2,     // View own data
  USER = 1        // Basic read access
}

// Enforce at both API and UI level
if (currentUser.role < UserRole.MANAGER) {
  throw new Error('Insufficient permissions');
}
```

## Code Quality Standards

### TypeScript Strictness
```typescript
// ✅ Always type everything
interface ServiceItem {
  id: string;
  business_id: string;
  status: JobStatus;
  scheduled_at: Date | null; // explicit null handling
}

// ❌ Never use any
const data: any = await fetchData(); // NO!

// ✅ Use unknown and narrow
const data: unknown = await fetchData();
if (isServiceItem(data)) {
  // TypeScript knows the type now
}
```

### Error Handling Pattern
```typescript
// API calls always return Result type
type Result<T> = { success: true; data: T } | { success: false; error: string };

async function fetchServiceItems(): Promise<Result<ServiceItem[]>> {
  try {
    const { data, error } = await supabase.from('service_items').select();
    if (error) return { success: false, error: error.message };
    return { success: true, data };
  } catch (err) {
    return { success: false, error: 'Network error' };
  }
}

// Usage
const result = await fetchServiceItems();
if (!result.success) {
  toast.error(result.error);
  return;
}
// TypeScript knows result.data exists
const items = result.data;
```

### React Component Standards
```typescript
// ✅ Functional components only
export function ServiceItemList({ businessId }: Props) {
  // Custom hooks for complex logic
  const { items, loading, error } = useServiceItems(businessId);
  
  // Early returns for loading/error states
  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage error={error} />;
  
  // Render data
  return (
    <ul>
      {items.map(item => (
        <ServiceItemCard key={item.id} item={item} />
      ))}
    </ul>
  );
}

// ❌ No class components
class ServiceItemList extends React.Component {} // NO!
```

## Testing Standards

### What to Test
1. **Business logic**: Pure functions with complex logic
2. **Database operations**: RLS policies, multi-tenant isolation
3. **Critical workflows**: Auth, payments, invite flow
4. **Mobile interactions**: Touch gestures, offline behavior

### What NOT to Test
- Simple getters/setters
- UI styling (use visual regression instead)
- Third-party library internals

## Performance Patterns

### Lazy Loading
```typescript
// Route-based code splitting
const ServiceItemsPage = lazy(() => import('./pages/ServiceItems'));
const ClientsPage = lazy(() => import('./pages/Clients'));

// Suspense boundary
<Suspense fallback={<LoadingScreen />}>
  <Routes>
    <Route path="/items" element={<ServiceItemsPage />} />
  </Routes>
</Suspense>
```

### Database Query Optimization
```sql
-- Always index foreign keys
CREATE INDEX idx_service_items_business_id ON service_items(business_id);
CREATE INDEX idx_service_items_status ON service_items(status);

-- Composite indexes for common queries
CREATE INDEX idx_service_items_business_status 
  ON service_items(business_id, status);
```

## Deployment Patterns

### Environment Variables
```bash
# Required for all environments
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
VITE_STRIPE_PUBLISHABLE_KEY=

# Backend-only (Edge Functions)
SUPABASE_SERVICE_ROLE_KEY=
RESEND_API_KEY=
STRIPE_SECRET_KEY=
```

### Edge Function Structure
```typescript
// packages/functions/<category>/<function-name>/index.ts
import { serve } from 'https://deno.land/std@0.177.0/http/server.ts';

serve(async (req) => {
  // 1. Validate auth
  // 2. Parse request
  // 3. Business logic
  // 4. Return response
});
```

## Anti-Patterns (Never Do This)

❌ **Hardcoded Business Logic**
```typescript
if (item.type === 'hvac_job') {} // NO! Use config
```

❌ **Any Type Usage**
```typescript
const data: any = await fetch(); // NO! Use unknown
```

❌ **Client-Side Security**
```typescript
if (user.role === 'ADMIN') showAdminUI(); // UI-only, not security
```

❌ **Skipping Mobile Testing**
```typescript
// Works in Chrome DevTools ≠ works on iPhone
```

❌ **Cross-Tenant Queries**
```sql
-- Missing business_id filter = security vulnerability
SELECT * FROM service_items WHERE status = 'completed';
```

## Remember

1. **Mobile-first** means test on actual devices
2. **Data-first** means define schema before UI
3. **Multi-tenant** means RLS on every table
4. **YAGNI** means reject complexity
5. **Type-safe** means no `any` types

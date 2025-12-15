# Tech Context: Stack Versions & Configuration

**Last Updated**: December 15, 2025

## Core Technology Stack

### Frontend
- **React**: 18.3.1
- **TypeScript**: 5.6.3
- **Vite**: 5.4.11
- **React Router**: 6.28.0

### UI Framework
- **Tailwind CSS**: 3.4.15
- **shadcn/ui**: Latest (component library, not versioned package)
- **Radix UI**: Various components (@radix-ui/react-*)
- **Lucide React**: 0.468.0 (icons)

### Mobile
- **Capacitor**: 6.2.0
  - `@capacitor/core`: 6.2.0
  - `@capacitor/ios`: 6.2.0
  - `@capacitor/android`: 6.2.0
  - `@capacitor/cli`: 6.2.0

### Backend & Database
- **Supabase JS**: 2.46.2
- **PostgreSQL**: 15.x (managed by Supabase)
- **Deno**: 1.x (for Edge Functions)

### Authentication
- **Supabase Auth**: Built-in
- **Email Verification**: Required
- **Invite System**: Custom (via Edge Functions)

### Email
- **Resend**: Latest
- **Domain**: `scheduleboard.co` (verified)
- **From Address**: `noreply@scheduleboard.co`

### Payments
- **Stripe**: Latest
- **Products**: Configured in Stripe Dashboard
- **Webhook**: Edge Function handler

### Hosting & Deployment
- **Vercel**: Latest
- **Domain Management**: Vercel DNS
- **Environment Variables**: Vercel project settings

## Development Environment

### Required Tools
- **Node.js**: 20.x LTS
- **pnpm**: 8.x (package manager)
- **VS Code**: Latest (with GitHub Copilot)
- **Xcode**: Latest (for iOS builds)
- **Android Studio**: Latest (for Android builds)

### VS Code Extensions
- GitHub Copilot
- ESLint
- Prettier
- Tailwind CSS IntelliSense
- TypeScript and JavaScript Language Features

## Environment Variables

### Frontend (Vite - `VITE_` prefix required)
```bash
VITE_SUPABASE_URL=https://[project-ref].supabase.co
VITE_SUPABASE_ANON_KEY=eyJ...
VITE_STRIPE_PUBLISHABLE_KEY=pk_test_...
```

### Backend (Edge Functions - Server-side only)
```bash
SUPABASE_SERVICE_ROLE_KEY=eyJ...
RESEND_API_KEY=re_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

### Development vs Production
- **Development**: Use Stripe test mode keys
- **Production**: Use live keys, enforce HTTPS

## Database Configuration

### Supabase Project Settings
- **Project ID**: (Set per deployed instance)
- **Region**: US West (or closest to target users)
- **Postgres Version**: 15.x
- **Extensions Enabled**:
  - `uuid-ossp`: UUID generation
  - `pgcrypto`: Encryption functions
  - `pg_stat_statements`: Query performance monitoring

### Connection Details
```typescript
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY,
  {
    auth: {
      autoRefreshToken: true,
      persistSession: true,
      detectSessionInUrl: true
    }
  }
);
```

## Mobile Platform Configuration

### iOS (Capacitor)
```json
// ios/App/App/capacitor.config.json
{
  "appId": "co.scheduleboard.app",
  "appName": "ScheduleBoard",
  "bundleId": "co.scheduleboard.app",
  "version": "1.0.0",
  "build": "1"
}
```

**Minimum iOS Version**: 13.0  
**Target iOS Version**: Latest  
**Build Increment Script**: `scripts/increment-ios-build.js`

### Android (Capacitor)
```json
// android/app/build.gradle
{
  "applicationId": "co.scheduleboard.app",
  "versionName": "1.0.0",
  "versionCode": 1,
  "minSdkVersion": 22,
  "targetSdkVersion": 34
}
```

**Minimum Android Version**: 22 (Android 5.1)  
**Target Android Version**: 34 (Android 14)  
**Build Increment Script**: `scripts/increment-android-build.js`

## API Keys & Secrets Management

### Development
- Local `.env` file (not committed)
- Load via Vite (`import.meta.env`)

### Production
- Vercel Environment Variables
- Edge Functions Secrets (via Supabase Dashboard)
- Never commit secrets to git

### Rotation Schedule
- **Stripe Test Keys**: Never rotate (test mode)
- **Stripe Live Keys**: Rotate annually
- **Supabase Keys**: Rotate on security incident
- **Resend API Key**: Rotate quarterly

## Build & Deploy Configuration

### Vercel Project Settings
```json
{
  "framework": "vite",
  "buildCommand": "pnpm build",
  "outputDirectory": "dist",
  "installCommand": "pnpm install",
  "nodeVersion": "20.x"
}
```

### Build Optimization
- **Code Splitting**: Automatic (Vite)
- **Tree Shaking**: Enabled
- **Minification**: Enabled in production
- **Source Maps**: Enabled for debugging

## Performance Targets

### Core Web Vitals
- **LCP** (Largest Contentful Paint): < 2.5s
- **FID** (First Input Delay): < 100ms
- **CLS** (Cumulative Layout Shift): < 0.1

### Mobile Performance
- **App Launch**: < 2s to interactive
- **Route Navigation**: < 300ms
- **API Response**: < 1s for 95th percentile

## Monitoring & Observability

### Error Tracking
- **Frontend**: Browser console + Vercel Analytics
- **Backend**: Edge Function logs in Supabase Dashboard
- **Database**: Postgres logs + slow query monitoring

### Analytics
- **User Events**: (TBD - add analytics provider)
- **Performance**: Vercel Analytics
- **Database**: Supabase Dashboard metrics

## Security Configuration

### Content Security Policy
```html
<meta http-equiv="Content-Security-Policy" 
  content="default-src 'self'; connect-src 'self' https://*.supabase.co;">
```

### CORS Configuration
- **Supabase**: Configured in dashboard
- **Vercel**: Automatic HTTPS enforcement
- **Edge Functions**: Explicit CORS headers

### Rate Limiting
- **Supabase**: Built-in (per-plan limits)
- **Stripe**: Built-in (Stripe manages)
- **Custom**: Implement via Edge Functions if needed

## Third-Party Service Limits

### Supabase (Free Tier Limits)
- Database: 500 MB
- Storage: 1 GB
- Bandwidth: 2 GB
- Edge Functions: 500K requests/month

### Vercel (Hobby Tier Limits)
- Builds: 6,000 minutes/month
- Bandwidth: 100 GB
- Serverless Functions: 100 GB-hours

### Resend (Free Tier Limits)
- Emails: 3,000/month
- Rate: 100/hour

### Stripe
- No request limits on paid accounts
- Test mode: Unlimited

## Upgrade Paths

When hitting free tier limits:
1. **Supabase**: Pro plan ($25/month)
2. **Vercel**: Pro plan ($20/month)
3. **Resend**: Growth plan ($20/month)
4. **Stripe**: No action needed (paid on usage)

## Known Issues & Workarounds

### iOS Keyboard Overlap
- **Issue**: Keyboard covers input on some iOS versions
- **Workaround**: Use `viewport-fit=cover` meta tag

### Android Back Button
- **Issue**: Back button doesn't work as expected
- **Workaround**: Implement custom back button handler in Capacitor

### Supabase Realtime Reconnection
- **Issue**: Websocket doesn't auto-reconnect reliably
- **Workaround**: Manual reconnect logic in app

## Dependency Update Strategy

- **Major versions**: Manual review required
- **Minor versions**: Auto-update via Dependabot
- **Patch versions**: Auto-update and deploy
- **Security updates**: Immediate manual update

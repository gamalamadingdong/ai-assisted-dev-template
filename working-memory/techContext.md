# Tech Context: Stack Versions & Configuration

**Last Updated**: [DATE] - Update when tech stack or versions change

> **IMPORTANT**: This is a TEMPLATE. Replace all placeholder values with your actual tech stack when starting your project.

## Core Technology Stack

### Frontend
- **Framework**: [e.g., React, Vue, Svelte, Angular, Next.js]
- **Version**: [e.g., 18.x, 3.x, etc.]
- **Language**: [e.g., TypeScript, JavaScript]
- **Build Tool**: [e.g., Vite, Webpack, Turbopack]
- **Router**: [e.g., React Router, Vue Router, TanStack Router]

Example:
```
- React: 18.3.1
- TypeScript: 5.6.3
- Vite: 5.4.11
- React Router: 6.28.0
```

### UI Framework
- **CSS Framework**: [e.g., Tailwind CSS, Material-UI, Bootstrap, Chakra UI]
- **Component Library**: [e.g., shadcn/ui, Radix UI, Headless UI, Ant Design]
- **Icons**: [e.g., Lucide React, Heroicons, Font Awesome, React Icons]

### Mobile (if applicable)
- **Platform**: [e.g., React Native, Flutter, Capacitor, Native iOS/Android]
- **Version**: [Specify versions]
- **Supported OS**: 
  - iOS: [Minimum version - e.g., iOS 13+]
  - Android: [Minimum API level - e.g., API 22+]

Example:
```
- Capacitor: 6.2.0
- iOS: 13.0+
- Android: API 22+ (Android 5.1+)
```

### Backend & Database
- **Backend**: [e.g., Supabase, Firebase, Node.js/Express, Python/FastAPI, Go]
- **Database**: [e.g., PostgreSQL, MySQL, MongoDB, SQLite]
- **ORM/Query Builder**: [e.g., Prisma, Drizzle, TypeORM, Sequelize]
- **API Type**: [e.g., REST, GraphQL, tRPC, gRPC]

### Authentication
- **Provider**: [e.g., Supabase Auth, Firebase Auth, Auth0, Clerk, NextAuth]
- **Methods**: [e.g., Email/Password, OAuth (Google, GitHub), Magic Links]
- **Session Management**: [e.g., JWT, Cookies, Server sessions]

### Email (if applicable)
- **Provider**: [e.g., Resend, SendGrid, AWS SES, Mailgun, Postmark]
- **Domain**: [Your verified sending domain]
- **From Address**: [e.g., noreply@yourdomain.com]

### Payments (if applicable)
- **Provider**: [e.g., Stripe, PayPal, Square, Paddle]
- **Integration Type**: [e.g., Stripe Checkout, Payment Intents, Subscriptions]
- **Webhook Handling**: [How you process webhooks]

### Hosting & Deployment
- **Frontend Hosting**: [e.g., Vercel, Netlify, Cloudflare Pages, AWS Amplify]
- **Backend Hosting**: [e.g., Vercel, Railway, Render, AWS, GCP, DigitalOcean]
- **Domain**: [Your domain name]
- **CDN**: [e.g., Vercel Edge Network, Cloudflare, AWS CloudFront]

## Development Environment

### Required Tools
- **Runtime**: [e.g., Node.js 20.x LTS, Python 3.11, Go 1.21]
- **Package Manager**: [e.g., npm, pnpm, yarn, pip, poetry]
- **IDE**: [e.g., VS Code, WebStorm, PyCharm]
- **Mobile Tools** (if applicable): [e.g., Xcode, Android Studio]

### IDE Extensions/Plugins
- [List your essential extensions]
- [e.g., ESLint, Prettier, Tailwind CSS IntelliSense]
- [e.g., GitHub Copilot, TypeScript Language Features]

## Environment Variables

> **IMPORTANT**: Replace with your actual environment variables

### Frontend (Client-side)
```bash
# Example for Vite (VITE_ prefix)
VITE_API_URL=https://api.yourdomain.com
VITE_PUBLIC_KEY=your_public_key

# Example for Next.js (NEXT_PUBLIC_ prefix)
NEXT_PUBLIC_API_URL=https://api.yourdomain.com
```

### Backend (Server-side only)
```bash
# Example secrets (NEVER expose to client)
DATABASE_URL=postgresql://user:password@host:5432/db
API_SECRET_KEY=your_secret_key
STRIPE_SECRET_KEY=sk_live_...
```

### Development vs Production
- **Development**: Use test/sandbox API keys
- **Production**: Use live keys, enforce HTTPS
- **Staging** (optional): Separate environment for testing

## Database Configuration

### [Your Database] Settings
- **Connection String**: [How to connect]
- **Version**: [e.g., PostgreSQL 15.x, MongoDB 7.x]
- **Region/Location**: [Where hosted]
- **Extensions/Features**: [What you're using]

Example for PostgreSQL:
```typescript
// Connection configuration
import { createClient } from '@supabase/supabase-js';

const db = createClient(
  process.env.DATABASE_URL,
  process.env.DATABASE_KEY
);
```

Example for MongoDB:
```typescript
import { MongoClient } from 'mongodb';

const client = new MongoClient(process.env.MONGODB_URI);
```

## Mobile Platform Configuration (if applicable)

### iOS
```json
// Example: ios/App/capacitor.config.json
{
  "appId": "com.yourcompany.app",
  "appName": "Your App Name",
  "bundleId": "com.yourcompany.app",
  "version": "1.0.0",
  "build": "1"
}
```

**Minimum iOS Version**: [e.g., 13.0]  
**Target iOS Version**: [e.g., Latest]

### Android
```json
// Example: android/app/build.gradle
{
  "applicationId": "com.yourcompany.app",
  "versionName": "1.0.0",
  "versionCode": 1,
  "minSdkVersion": [e.g., 22],
  "targetSdkVersion": [e.g., 34]
}
```

**Minimum Android Version**: [e.g., API 22 (Android 5.1)]  
**Target Android Version**: [e.g., API 34 (Android 14)]

## API Keys & Secrets Management

### Development
- Local `.env` file (not committed to git)
- Add `.env` to `.gitignore`
- Use `.env.example` as template for team

### Production
- [e.g., Vercel Environment Variables]
- [e.g., AWS Secrets Manager]
- [e.g., Doppler, HashiCorp Vault]
- **NEVER commit secrets to git**

### Rotation Schedule
Document when to rotate keys:
- **[Service 1]**: [e.g., Rotate quarterly]
- **[Service 2]**: [e.g., Rotate annually]
- **[Service 3]**: [e.g., Rotate on security incident]

## Build & Deploy Configuration

### [Your Hosting Provider] Settings
```json
{
  "framework": "[your framework]",
  "buildCommand": "[your build command]",
  "outputDirectory": "[your output dir]",
  "installCommand": "[your install command]"
}
```

Example for Vercel:
```json
{
  "framework": "vite",
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "installCommand": "npm install"
}
```

### Build Optimization
- **Code Splitting**: [Enabled/Disabled, how configured]
- **Tree Shaking**: [Enabled/Disabled]
- **Minification**: [Enabled in production]
- **Source Maps**: [Enabled for debugging]

## Performance Targets

### Core Web Vitals (if web app)
- **LCP** (Largest Contentful Paint): [e.g., < 2.5s]
- **FID** (First Input Delay): [e.g., < 100ms]
- **CLS** (Cumulative Layout Shift): [e.g., < 0.1]

### App Performance (if mobile)
- **App Launch**: [e.g., < 2s to interactive]
- **Route Navigation**: [e.g., < 300ms]
- **API Response**: [e.g., < 1s for 95th percentile]

## Monitoring & Observability

### Error Tracking
- **Frontend**: [e.g., Sentry, LogRocket, Rollbar]
- **Backend**: [e.g., Sentry, Datadog, CloudWatch]
- **Database**: [e.g., Built-in logs, PgAnalyze]

### Analytics
- **User Events**: [e.g., Google Analytics, Mixpanel, PostHog]
- **Performance**: [e.g., Vercel Analytics, Lighthouse CI]
- **Custom Metrics**: [Your specific tracking]

## Security Configuration

### Content Security Policy (if web app)
```html
<meta http-equiv="Content-Security-Policy" 
  content="default-src 'self'; connect-src 'self' https://api.yourdomain.com;">
```

### CORS Configuration
- **[Your API]**: [Allowed origins]
- **[Your CDN]**: [Configuration]

### Rate Limiting
- **[Service 1]**: [Limits and configuration]
- **[Service 2]**: [Limits and configuration]

## Third-Party Service Limits

Document free tier limits for planning:

### [Service 1]
- [Limit 1]: [e.g., 500 MB storage]
- [Limit 2]: [e.g., 100K requests/month]
- [Limit 3]: [e.g., 5 GB bandwidth]

### [Service 2]
- [Limit 1]: [e.g., 3,000 emails/month]
- [Limit 2]: [e.g., 100 emails/hour]

### Upgrade Paths
When hitting free tier limits:
1. **[Service 1]**: [Plan name] at [$X/month]
2. **[Service 2]**: [Plan name] at [$X/month]

## Known Issues & Workarounds

Document platform-specific issues and solutions:

### [Issue 1 Title]
- **Issue**: [Description of the problem]
- **Workaround**: [How to handle it]

### [Issue 2 Title]
- **Issue**: [Description of the problem]
- **Workaround**: [How to handle it]

## Dependency Update Strategy

- **Major versions**: [e.g., Manual review required]
- **Minor versions**: [e.g., Auto-update via Dependabot]
- **Patch versions**: [e.g., Auto-update and deploy]
- **Security updates**: [e.g., Immediate manual update]

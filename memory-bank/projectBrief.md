# Project Brief: [Your Project Name]

**Last Updated**: [DATE] - Update when your mission or requirements change  
**Project Phase**: [Planning | Development | MVP | Beta | Production]

## Core Mission

[Describe the core goal of your project in 2-3 sentences. What problem are you solving? Who are you solving it for?]

## Non-Negotiable Requirements

### 1. Production Quality Preservation
- All extracted components MUST maintain ScheduleBoard v2's production-ready quality
- Mobile-first responsive design is mandatory
- Touch interactions must be preserved and optimized
- Offline-capable core functionality required

### 2. Multi-Business Type Flexibility
- Template must support: HVAC, plumbing, cleaning, personal care, general service businesses
- Components adapt terminology and workflows per business type
- Configurable feature toggles for industry-specific requirements
- No hard-coded business logic

### 3. Instruction-Driven Architecture
- `.github/instructions/` contains structured prompt files
- Memory Bank pattern for persistent context across sessions
- No code generator CLI - Copilot executes instructions directly
- Documentation-first approach with clear implementation patterns

### 4. Technology Stack (Non-Negotiable)
- **Frontend**: React 18 + TypeScript + Vite
- **Backend**: Supabase (PostgreSQL + Edge Functions + Auth)
- **Mobile**: Capacitor (native iOS/Android compilation)
- **UI**: Tailwind CSS + shadcn/ui
- **Email**: Resend
- **Payments**: Stripe

## Success Criteria

1. **Speed**: New business app setup in < 1 hour
2. **Quality**: Zero mobile compliance issues on iOS/Android
3. **Flexibility**: Works across 5+ different service business types
4. **Maintainability**: Clear instructions, no "magic" generation
5. **Production-Ready**: Auth, payments, email, database from day one

## What This Project IS NOT

- ❌ A no-code platform
- ❌ A universal framework for all app types
- ❌ A code generator with complex CLI
- ❌ A learning/tutorial project

## What This Project IS

- ✅ A battle-tested architecture template
- ✅ An instruction-driven Copilot workflow
- ✅ A mobile-first production foundation
- ✅ A personal business accelerator

## Business Context

**Target User**: Solo entrepreneur (me) building multiple service business applications  
**Use Case**: Rapidly spin up production-quality mobile apps for different business ideas  
**Priority**: Speed + Quality + Maintainability over configurability

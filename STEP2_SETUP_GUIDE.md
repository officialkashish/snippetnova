# 📖 SnippetStream - Complete Setup Guide

## ✅ Step 2 Completion Checklist

Your Next.js project is now fully configured with Supabase integration!

### Files Created:
- ✅ **Environment Variables**: `.env.local`, `.env.example`
- ✅ **Type Definitions**: `src/lib/types.ts`
- ✅ **Constants**: `src/lib/constants.ts`
- ✅ **Utilities**: `src/lib/utils.ts`
- ✅ **Supabase Clients**: `src/lib/supabase/{client,server,admin}.ts`
- ✅ **Styles**: `src/styles/globals.css`
- ✅ **Middleware**: `src/middleware.ts`
- ✅ **Hooks**: `src/hooks/useAuth.ts`
- ✅ **Providers**: `src/components/ReactQueryProvider.tsx`
- ✅ **Components**: Navbar, Footer
- ✅ **Layouts**: Root layout with providers
- ✅ **Pages**: Home page, OAuth callback
- ✅ **Config**: Tailwind, Next.js, TypeScript

---

## 🚀 Local Setup Instructions

### Step 1: Create Next.js App (if not already done)

```bash
npx create-next-app@latest snippetstream \
  --typescript \
  --tailwind \
  --eslint \
  --app \
  --import-alias '@/*'

cd snippetstream
```

### Step 2: Install Dependencies

```bash
npm install \
  @supabase/supabase-js \
  @supabase/auth-helpers-nextjs \
  @tanstack/react-query \
  @monaco-editor/react \
  shiki \
  react-markdown \
  remark-gfm \
  lucide-react \
  clsx \
  tailwind-merge
```

### Step 3: Copy All Files from GitHub

All files are committed to your repository. Pull them or copy from the provided code.

### Step 4: Configure Supabase

**Get your credentials from Supabase:**

1. Go to **Settings → API**
2. Copy:
   - `Project URL` 
   - `anon public` (NEXT_PUBLIC_SUPABASE_ANON_KEY)
   - `service_role` (SUPABASE_SERVICE_ROLE_KEY)

**Update `.env.local`:**

```bash
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGc...
SUPABASE_SERVICE_ROLE_KEY=eyJhbGc...
```

### Step 5: Test the Setup

```bash
npm run dev
```

Visit **http://localhost:3000**

### Step 6: Verify Auth Flow

1. ✅ See "Sign in with GitHub" button
2. ✅ Click button → redirects to GitHub OAuth
3. ✅ Authorize the app
4. ✅ Redirects back to app
5. ✅ Profile auto-created in database
6. ✅ See username in navbar
7. ✅ "New" and "Dashboard" links visible
8. ✅ Logout button works

---

## 🔍 Key Files Explained

### `src/lib/supabase/client.ts`
**Purpose**: Client-side Supabase operations
- Used in React components
- Has access to current user session via cookies
- Limited by RLS policies

```typescript
const supabase = createClient();
const { data } = await supabase.from("snippets").select("*");
```

### `src/lib/supabase/server.ts`
**Purpose**: Server-side operations (Server Components)
- Can run in Next.js Server Components
- Maintains session via cookies
- Also limited by RLS

### `src/lib/supabase/admin.ts`
**Purpose**: Privileged operations (Bypass RLS)
- **NEVER** use in frontend code
- Only use in API routes with `SUPABASE_SERVICE_ROLE_KEY`
- For admin-only tasks

### `src/hooks/useAuth.ts`
**Purpose**: Authentication state management
```typescript
const { user, profile, isAuthenticated, signInWithGitHub, signOut } = useAuth();
```

### `src/middleware.ts`
**Purpose**: Session refresh on each request
- Automatically refreshes Supabase session
- Maintains user logged-in state
- Routes requests to correct handlers

---

## 📊 Architecture Diagram

```
┌─────────────────────────────────────────┐
│      Browser / Client                    │
│  (React Components + useAuth hook)       │
└────────────────┬────────────────────────┘
                 │
     ┌───────────┴──────────┐
     │                      │
┌────▼─────────┐     ┌─────▼──────────┐
│ Client Code  │     │ Middleware     │
│ @supabase/   │     │ Refresh Session│
│ supabase-js  │     └─────┬──────────┘
└────┬─────────┘            │
     │                      │
     └──────────┬───────────┘
                │
     ┌──────────▼──────────┐
     │ Supabase (PostgreSQL)│
     │ + RLS Policies       │
     │ + Triggers           │
     │ + Real-time          │
     └──────────────────────┘
```

---

## 🛡️ Security Checklist

- ✅ `SUPABASE_SERVICE_ROLE_KEY` in `.env.local` (not `.env.example`)
- ✅ RLS policies prevent unauthorized access
- ✅ Triggers maintain data consistency
- ✅ Session tokens auto-refresh in middleware
- ✅ OAuth redirects to callback route

---

## 🧪 Testing Checklist

Before moving to Step 3, verify:

```
□ npm run dev starts without errors
□ Page loads at http://localhost:3000
□ "Sign in with GitHub" button visible
□ Click button → GitHub OAuth page
□ Authorize → redirects back to app
□ Check Supabase: profile created
□ Navbar shows username
□ "New" link navigates to /create
□ "Dashboard" link navigates to /dashboard
□ "Logout" signs out user
□ After logout → "Sign in" button back
```

---

## 📝 Debugging Tips

### Issue: "NEXT_PUBLIC_SUPABASE_URL is required"
**Solution**: Check `.env.local` has correct values

### Issue: OAuth returns error
**Solution**: 
1. Check GitHub OAuth app is registered
2. Verify `Authorization callback URL` in GitHub settings
3. Confirm `NEXT_PUBLIC_SUPABASE_URL` matches Supabase project

### Issue: Profile not created after login
**Solution**:
1. Check `create_user_profile` trigger exists in Supabase
2. Check `profiles` table has no constraint errors
3. Look at Supabase logs for trigger errors

### Issue: Middleware errors
**Solution**: Restart dev server with `npm run dev`

---

## 🎯 Next Steps (STEP 3)

When ready, we'll build:

### **STEP 3: Snippet Creation & Server Actions**

1. **CodeEditor Component** ✍️
   - Monaco Editor integration
   - Multi-language support
   - Real-time preview

2. **SnippetForm Component** 📋
   - Title, description, tags input
   - Language selector
   - Publish/draft toggle

3. **Server Actions** ⚡
   - `actions/snippets.ts` → create, update, delete
   - Form validation
   - Syntax highlighting with Shiki

4. **Create Page** 🚀
   - Full snippet creation flow
   - Error handling
   - Success notification

---

## 📚 Additional Resources

- [Supabase Docs](https://supabase.com/docs)
- [Next.js App Router](https://nextjs.org/docs/app)
- [React Query Docs](https://tanstack.com/query/latest)
- [Tailwind CSS](https://tailwindcss.com)
- [Monaco Editor](https://microsoft.github.io/monaco-editor/)
- [Shiki](https://shiki.matsu.io/)

---

## 💡 Key Concepts Review

### React Server Components
- Components rendered on server
- Can directly query database
- Better performance, smaller bundle

### Row Level Security (RLS)
- Database-level authorization
- Users can only see/modify their own data
- No backend authentication needed

### Server Actions
- `'use server'` functions
- Can safely call from client
- Database mutations with security

### Triggers
- Automatically execute on events
- Maintain `upvote_count` and `snippet_count`
- Ensure data consistency

---

**✨ You're now ready to build the snippet creation system!**

When ready, confirm and we'll move to **STEP 3: Snippet Creation**.

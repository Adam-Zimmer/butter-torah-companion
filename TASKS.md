# Torah Companion - Task Breakdown

**Status Legend:**
- ⬜ Not started
- 🔄 In progress
- ✅ Complete
- 🚫 Blocked

---

## PHASE 1: Foundation & Authentication

### 1.1 Project Setup ⬜
- [ ] Create monorepo directory structure (`frontend/`, `backend/`, `shared/`)
- [ ] Initialize frontend: `npm create vite@latest frontend -- --template react-ts`
- [ ] Install frontend deps: React Router, Tailwind, tRPC client, Firebase SDK
- [ ] Initialize backend: `npm init -y` + TypeScript + tRPC + Express
- [ ] Install backend deps: tRPC, Prisma, Zod, Anthropic SDK, Express
- [ ] Create shared types package (if needed)
- [ ] Set up Doppler project and sync secrets
- [ ] Create `.env.example` files for local development
- [ ] Write README with setup instructions
- [ ] Initialize git, create `.gitignore`

**Estimated Time:** 4 hours

---

### 1.2 Database & Schema ⬜
- [ ] Create PostgreSQL database on Railway
- [ ] Install Prisma: `npm install prisma @prisma/client`
- [ ] Initialize Prisma: `npx prisma init`
- [ ] Define Prisma schema (User, Conversation, Message, Source, UserContext)
- [ ] Create initial migration: `npx prisma migrate dev --name init`
- [ ] Generate Prisma client: `npx prisma generate`
- [ ] Create seed script with test data
- [ ] Run seed: `npx prisma db seed`
- [ ] Test database connection

**Estimated Time:** 3 hours

---

### 1.3 Authentication ⬜
- [ ] Create Firebase project in console
- [ ] Enable Google OAuth provider
- [ ] Add Firebase config to frontend
- [ ] Create Firebase Auth context (React)
- [ ] Implement Google sign-in button
- [ ] Implement sign-out functionality
- [ ] Create protected route wrapper
- [ ] Create backend tRPC middleware to verify Firebase tokens
- [ ] Create `auth.register` mutation (create user on first login)
- [ ] Create `auth.me` query (get current user)
- [ ] Test auth flow end-to-end

**Estimated Time:** 5 hours

---

## PHASE 2: Core Chat Functionality

### 2.1 Backend Chat API ⬜
- [ ] Create `chat` tRPC router
- [ ] Implement `chat.getConversations` query
  - Fetch all conversations for current user
  - Include latest message preview
  - Sort by updatedAt DESC
- [ ] Implement `chat.getConversation` query
  - Fetch single conversation with all messages
  - Include sources for each message
- [ ] Implement `chat.createConversation` mutation
  - Create new conversation for user
  - Return conversation ID
- [ ] Implement `chat.sendMessage` mutation
  - Accept conversationId, content
  - Fetch user profile and conversation history
  - Build system prompt with context
  - Call Claude API
  - Parse response
  - Extract and save sources
  - Save user message and assistant response
  - Return assistant message
- [ ] Implement `chat.deleteConversation` mutation
- [ ] Add error handling and validation (Zod schemas)
- [ ] Test all endpoints with Postman/tRPC panel

**Estimated Time:** 8 hours

---

### 2.2 Claude Integration ⬜
- [ ] Install Anthropic SDK: `npm install @anthropic-ai/sdk`
- [ ] Create Claude service class/module
- [ ] Build system prompt template function
  - Accept user profile (denomination, life stage, etc.)
  - Accept conversation summary (if exists)
  - Include instructions to cite Torah sources
  - Include instructions for Hebrew text handling
- [ ] Implement `generateResponse` method
  - Accept messages array + user context
  - Call Anthropic API
  - Handle streaming (optional for MVP)
  - Return response text
- [ ] Implement rate limiting
- [ ] Implement retry logic for API failures
- [ ] Add token usage tracking
- [ ] Test with sample prompts

**Estimated Time:** 4 hours

---

### 2.3 Source Citation Parser ⬜
- [ ] Define citation format expected from Claude
- [ ] Create regex/parser to extract citations from response
- [ ] Parse citation types: Torah, Talmud, Midrash, commentary
- [ ] Validate citation references (basic format check)
- [ ] Create Source records in database
- [ ] Handle edge cases (no citations, malformed citations)
- [ ] Test with various response formats

**Estimated Time:** 3 hours

---

## PHASE 3: Frontend Chat UI

### 3.1 Chat Interface ⬜
- [ ] Create `ChatPage` component
- [ ] Build `MessageBubble` component
  - User message (blue bubble, right-aligned)
  - Assistant message (gray bubble, left-aligned)
  - Timestamp display
  - Star of David icon for assistant
- [ ] Build `SourceCitation` component
  - Display citation reference
  - Expandable to show full text
  - Hebrew RTL support
- [ ] Build `ChatInput` component
  - Auto-resizing textarea
  - Send button
  - Disabled state while loading
  - Enter to send (Shift+Enter for new line)
- [ ] Build `LoadingIndicator` component (typing dots)
- [ ] Implement message list with auto-scroll to bottom
- [ ] Add empty state ("Start a conversation...")
- [ ] Test on mobile viewport

**Estimated Time:** 6 hours

---

### 3.2 Conversation Management ⬜
- [ ] Create `ConversationSidebar` component
- [ ] Build `ConversationListItem` component
  - Show title or preview
  - Show timestamp
  - Highlight active conversation
- [ ] Implement "New Conversation" button
- [ ] Implement conversation switching
- [ ] Auto-generate conversation title
  - Use first user message (truncated)
  - Or use Claude to generate title (optional)
- [ ] Implement delete conversation (with confirmation)
- [ ] Add conversation search/filter (optional)
- [ ] Test conversation flow

**Estimated Time:** 4 hours

---

### 3.3 Source Citation UI ⬜
- [ ] Style citation display in message bubble
- [ ] Implement expandable accordion for citations
- [ ] Display Hebrew text with proper RTL
- [ ] Display English translation (if available)
- [ ] Add "Learn More" link to external Torah sources (optional)
- [ ] Handle multiple citations per message
- [ ] Test with various citation types

**Estimated Time:** 3 hours

---

## PHASE 4: Personalization & Onboarding

### 4.1 Onboarding Flow ⬜
- [ ] Create `OnboardingWizard` component (multi-step)
- [ ] Step 1: Welcome screen
  - Value proposition
  - "Get Started" button
- [ ] Step 2: Denomination selection
  - Orthodox, Conservative, Reform, Reconstructionist, Secular, Other
  - Optional field
- [ ] Step 3: Hebrew proficiency
  - None, Beginner, Intermediate, Fluent
  - Optional field
- [ ] Step 4: Life stage
  - Student, Young Professional, Parent, Retiree, etc.
  - Optional field
- [ ] Step 5: Optional open text ("What are you exploring?")
- [ ] Create `user.updateProfile` mutation
- [ ] Save onboarding data on completion
- [ ] Redirect to chat after onboarding
- [ ] Show onboarding only on first login
- [ ] Test full flow

**Estimated Time:** 5 hours

---

### 4.2 User Profile & Settings ⬜
- [ ] Create `SettingsPage` component
- [ ] Build profile edit form
  - Name
  - Denomination
  - Hebrew proficiency
  - Life stage
  - Email (read-only)
- [ ] Create `user.updateProfile` mutation (if not already)
- [ ] Implement save functionality
- [ ] Add "Delete Account" button
  - Confirmation modal
  - `user.deleteAccount` mutation
  - Sign out and redirect
- [ ] Test profile updates

**Estimated Time:** 3 hours

---

### 4.3 Context Management ⬜
- [ ] Create `UserContext` table (already in schema)
- [ ] Implement conversation summary generation
  - Use Claude to summarize conversation after 10+ messages
  - Store summary in `Conversation.summary`
- [ ] Include summaries in system prompt
- [ ] Create `context.update` mutation (for manual context updates)
- [ ] Display user context in settings (optional)
- [ ] Test context continuity across conversations

**Estimated Time:** 4 hours

---

## PHASE 5: Polish & Observability

### 5.1 Analytics Integration ⬜
- [ ] Create Mixpanel project
- [ ] Install Mixpanel SDK: `npm install mixpanel-browser`
- [ ] Create analytics service/hook
- [ ] Instrument events:
  - `user_signed_up`
  - `user_signed_in`
  - `message_sent`
  - `conversation_created`
  - `source_clicked`
  - `onboarding_completed`
- [ ] Set user properties (denomination, life stage, etc.)
- [ ] Test event tracking in Mixpanel dashboard

**Estimated Time:** 3 hours

---

### 5.2 Error Tracking ⬜
- [ ] Create Sentry project
- [ ] Install Sentry SDK (frontend + backend)
- [ ] Configure Sentry in frontend (`Sentry.init`)
- [ ] Configure Sentry in backend
- [ ] Add error boundaries in React
- [ ] Test error capture (trigger intentional error)
- [ ] Verify errors appear in Sentry dashboard

**Estimated Time:** 2 hours

---

### 5.3 Performance & UX ⬜
- [ ] Add loading skeletons for conversation list
- [ ] Add loading skeleton for messages
- [ ] Implement optimistic UI for sending messages
- [ ] Add toast notifications for errors
- [ ] Test mobile responsiveness
- [ ] Optimize API response times
  - Add database indexes
  - Use connection pooling
- [ ] Test on slow network (throttle in DevTools)

**Estimated Time:** 4 hours

---

### 5.4 Security & Compliance ⬜
- [ ] Implement rate limiting on API endpoints
  - Use `express-rate-limit` or tRPC middleware
- [ ] Add input validation (Zod) on all mutations
- [ ] Sanitize user input (prevent XSS)
- [ ] Configure CORS properly
- [ ] Write Privacy Policy page
- [ ] Write Terms of Service page
- [ ] Implement data export (GDPR)
  - `user.exportData` mutation
- [ ] Implement data deletion (already in 4.2)
- [ ] Add cookie consent banner (if using cookies)
- [ ] Test security with OWASP checklist

**Estimated Time:** 6 hours

---

## PHASE 6: Deployment & Launch Prep

### 6.1 Production Deployment ⬜
- [ ] Create Railway project
- [ ] Set up PostgreSQL service
- [ ] Set up backend service
  - Connect to database
  - Add environment variables
  - Deploy from GitHub
- [ ] Set up frontend service
  - Add environment variables
  - Deploy from GitHub
- [ ] Configure Railway domains
- [ ] Run database migrations on production
- [ ] Test production deployment end-to-end
- [ ] Set up custom domain (optional)
- [ ] Configure DNS (if custom domain)

**Estimated Time:** 4 hours

---

### 6.2 Monitoring & Logging ⬜
- [ ] Set up Railway metrics dashboard
- [ ] Configure log aggregation (Railway built-in)
- [ ] Set up uptime monitoring (UptimeRobot)
- [ ] Create admin dashboard for usage stats (optional)
  - Total users
  - Total conversations
  - Total messages
  - Active users today/week/month
- [ ] Set up alerts for errors/downtime

**Estimated Time:** 3 hours

---

### 6.3 Launch Preparation ⬜
- [ ] Update landing page with real CTA
  - Link "Get Started" buttons to app
  - Add screenshots/demo
- [ ] Write FAQ page
- [ ] Write Help/Documentation page
- [ ] Create demo video (optional)
- [ ] Test complete user journey
  - Sign up → Onboarding → Chat → Settings → Logout
- [ ] Prepare launch email (if early signups exist)
- [ ] Create launch announcement (Twitter, Reddit, etc.)
- [ ] Soft launch to friends/family for feedback

**Estimated Time:** 5 hours

---

## Total Estimated Time

**Total Hours:** ~80 hours  
**Timeline (full-time):** 2 weeks  
**Timeline (part-time, 20h/week):** 4 weeks  

---

## Critical Path (Minimum Viable)

If time is constrained, prioritize these tasks first:

1. ✅ Project setup (1.1)
2. ✅ Database schema (1.2)
3. ✅ Authentication (1.3)
4. ✅ Backend chat API (2.1)
5. ✅ Claude integration (2.2)
6. ✅ Chat interface (3.1)
7. ✅ Conversation management (3.2)
8. ✅ Production deployment (6.1)

**Critical Path Time:** ~40 hours (1 week full-time)

Everything else (onboarding, settings, analytics, polish) can be added post-launch.

---

## Dependencies

- **Firebase** project created
- **Railway** account set up
- **Anthropic API** key obtained
- **Doppler** account set up (optional, can use .env)
- **Mixpanel** account (for analytics)
- **Sentry** account (for error tracking)
- **Domain name** (if using custom domain)

---

## Notes

- This plan assumes **single developer, full-time** work
- Adjust timeline if working part-time or with a team
- Many tasks can be parallelized (frontend + backend)
- Some tasks are marked "optional" — can be deferred post-MVP
- Testing is embedded in each task, not a separate phase

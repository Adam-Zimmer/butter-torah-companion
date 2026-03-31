# Torah Companion - Implementation Plan

## Product Vision
A personalized AI spiritual companion that provides Torah-based guidance, remembers user context over time, and creates a meaningful relationship between users and Jewish wisdom.

---

## Key Differentiators (vs Competitors)
1. **True Personalization** — Remembers user's spiritual journey, life context, past conversations
2. **Relationship Building** — Not just Q&A, but ongoing companion that knows you
3. **Context-Aware Guidance** — Understands life stage, denomination, Hebrew proficiency
4. **Conversational Memory** — Builds on previous discussions naturally

---

## Technical Architecture

### Frontend
- **Technology:** React + Vite + TypeScript + Tailwind
- **Authentication:** Firebase Auth (Google OAuth)
- **State Management:** React Context / Zustand (if needed)
- **Components:**
  - Chat interface (main screen)
  - User profile/settings
  - Conversation history
  - Source citation viewer
  - Landing page (already built)

### Backend
- **Technology:** Node.js + TypeScript + tRPC
- **Database:** PostgreSQL + Prisma ORM
- **AI Provider:** Claude (Anthropic) via API
- **Architecture:** Monolith (for MVP speed)

### Infrastructure
- **Hosting:** Railway (all services)
- **Env Management:** Doppler + local .env overrides
- **Observability:** Sentry for errors
- **Analytics:** Mixpanel for user events
- **Email:** Resend (for notifications/onboarding)
- **Payments:** Stripe (when monetization starts)

### Data Model (Prisma Schema)

```prisma
model User {
  id                String   @id @default(uuid())
  email             String   @unique
  name              String?
  firebaseUid       String   @unique
  
  // Personalization fields
  denomination      String?  // orthodox, conservative, reform, reconstructionist, secular
  hebrewProficiency String?  // none, beginner, intermediate, fluent
  lifeStage         String?  // student, young_professional, parent, retiree, etc.
  preferences       Json?    // flexible JSON for additional context
  
  conversations     Conversation[]
  messages          Message[]
  
  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt
}

model Conversation {
  id          String    @id @default(uuid())
  userId      String
  user        User      @relation(fields: [userId], references: [id])
  
  title       String?   // Auto-generated from first message
  summary     String?   // AI-generated summary for context
  
  messages    Message[]
  
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}

model Message {
  id              String       @id @default(uuid())
  conversationId  String
  conversation    Conversation @relation(fields: [conversationId], references: [id])
  userId          String
  user            User         @relation(fields: [userId], references: [id])
  
  role            String       // user, assistant, system
  content         String       @db.Text
  
  // Source citations
  sources         Source[]
  
  // AI metadata
  modelUsed       String?
  tokensUsed      Int?
  
  createdAt       DateTime     @default(now())
}

model Source {
  id          String   @id @default(uuid())
  messageId   String
  message     Message  @relation(fields: [messageId], references: [id])
  
  type        String   // torah, talmud, midrash, commentary, etc.
  reference   String   // e.g., "Genesis 1:1", "Pirkei Avot 3:17"
  text        String?  @db.Text
  translation String?  @db.Text
  
  createdAt   DateTime @default(now())
}

model UserContext {
  id              String   @id @default(uuid())
  userId          String   @unique
  
  // Long-term memory/context for personalization
  ongoingChallenges  Json?  // e.g., career stress, family issues
  spiritualGoals     Json?  // what they're working on
  topicHistory       Json?  // frequently discussed topics
  
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt
}
```

---

## Core Features (MVP)

### Phase 1: Basic Chat (Week 1-2)
- [ ] User can sign in with Google
- [ ] User can ask a question
- [ ] Claude responds with Torah-grounded answer
- [ ] Responses include source citations
- [ ] Chat history is saved
- [ ] User can view past conversations

### Phase 2: Personalization (Week 2-3)
- [ ] Onboarding flow captures user profile (denomination, Hebrew level, life stage)
- [ ] System prompt includes user context
- [ ] Conversation summaries auto-generated for long-term memory
- [ ] User profile editable in settings

### Phase 3: Enhanced UX (Week 3-4)
- [ ] Source citations are clickable/expandable
- [ ] Conversation titles auto-generated
- [ ] Hebrew text displays properly (RTL support)
- [ ] Mobile-responsive design
- [ ] Loading states and error handling

### Phase 4: Analytics & Observability (Week 4)
- [ ] Mixpanel events for key user actions
- [ ] Sentry error tracking
- [ ] Usage metrics dashboard (admin)

---

## Implementation Tasks

### **PHASE 1: Foundation & Authentication**

#### Task 1.1: Project Setup
- Create monorepo structure
- Initialize frontend (Vite + React + TypeScript + Tailwind)
- Initialize backend (Node + TypeScript + tRPC)
- Set up Doppler for environment variables
- Create Railway project and initial deployments
- Set up GitHub Actions for CI/CD (optional for MVP)

#### Task 1.2: Database & Schema
- Set up PostgreSQL on Railway
- Define Prisma schema (as above)
- Create initial migration
- Seed database with test data

#### Task 1.3: Authentication
- Set up Firebase project
- Implement Google OAuth flow (frontend)
- Create Firebase Auth middleware (backend)
- Create user registration/login endpoints (tRPC)
- Store user in database on first login

---

### **PHASE 2: Core Chat Functionality**

#### Task 2.1: Backend Chat API
- Create tRPC router for chat operations
- Implement `sendMessage` mutation
  - Accept user message
  - Fetch user context from DB
  - Build system prompt with personalization
  - Call Claude API
  - Parse response and extract sources
  - Save message + sources to DB
  - Return response
- Implement `getConversations` query
- Implement `getConversation` query (with messages)
- Implement `createConversation` mutation

#### Task 2.2: Claude Integration
- Set up Anthropic API client
- Build system prompt template
  - Include user profile context
  - Include conversation summary (if exists)
  - Instruct to cite sources
- Implement streaming support (optional for MVP)
- Handle rate limits and errors

#### Task 2.3: Source Citation Parser
- Create parser to extract Torah citations from Claude response
- Validate citation format
- Store structured citation data
- Handle multiple citation types (Torah, Talmud, Midrash, etc.)

---

### **PHASE 3: Frontend Chat UI**

#### Task 3.1: Chat Interface
- Build chat message component
  - User message bubble
  - Assistant message bubble
  - Source citation display
  - Loading indicator
- Build chat input component
  - Text area with auto-resize
  - Send button
  - Character limit display (optional)
- Build conversation list sidebar
  - List all conversations
  - Show conversation title + preview
  - Click to switch conversation
  - Create new conversation button

#### Task 3.2: Conversation Management
- Implement conversation switching
- Auto-generate conversation title from first message
- Delete conversation functionality
- Edit conversation title (optional)

#### Task 3.3: Source Citation UI
- Build expandable citation component
- Display Hebrew text with RTL support
- Show translation (if available)
- Link to external Torah text sources (optional)

---

### **PHASE 4: Personalization & Onboarding**

#### Task 4.1: Onboarding Flow
- Create multi-step onboarding wizard
  - Step 1: Welcome + value prop
  - Step 2: Denomination selection
  - Step 3: Hebrew proficiency
  - Step 4: Life stage / context
  - Step 5: Optional: What are you exploring?
- Save onboarding data to user profile
- Skip if user already onboarded

#### Task 4.2: User Profile & Settings
- Build settings page
- Edit profile fields
- View/edit personalization context
- Delete account (GDPR compliance)

#### Task 4.3: Context Management
- Auto-generate conversation summaries (use Claude)
- Store summaries in UserContext table
- Include summaries in system prompt for continuity
- Periodic context refresh (batch job or on-demand)

---

### **PHASE 5: Polish & Observability**

#### Task 5.1: Analytics Integration
- Set up Mixpanel project
- Instrument key events:
  - User sign up
  - User sign in
  - Message sent
  - Conversation created
  - Source citation clicked
  - Onboarding completed
- Create user properties (denomination, life stage, etc.)

#### Task 5.2: Error Tracking
- Set up Sentry project
- Integrate Sentry SDK (frontend + backend)
- Configure error boundaries (frontend)
- Test error reporting

#### Task 5.3: Performance & UX
- Optimize API response times
- Add loading skeletons
- Implement optimistic UI updates
- Add toast notifications for errors
- Mobile responsiveness audit

#### Task 5.4: Security & Compliance
- Rate limiting on API endpoints
- Input validation and sanitization
- CORS configuration
- Privacy policy page
- Terms of service page
- GDPR compliance (data export, deletion)

---

### **PHASE 6: Deployment & Launch Prep**

#### Task 6.1: Production Deployment
- Configure Railway production environment
- Set up production database
- Run migrations on production
- Configure production environment variables
- Set up custom domain (if applicable)

#### Task 6.2: Monitoring & Logging
- Set up Railway metrics dashboard
- Configure log aggregation
- Set up uptime monitoring (UptimeRobot or similar)
- Create admin dashboard for usage stats

#### Task 6.3: Launch Preparation
- Write help documentation / FAQ
- Create demo video (optional)
- Update landing page with "Launch" CTA
- Prepare launch email (if collecting early signups)
- Test full user journey end-to-end

---

## Future Enhancements (Post-MVP)

### Advanced Personalization
- Proactive check-ins ("How's that career decision going?")
- Spiritual growth tracking
- Torah study plan recommendations

### Social Features
- Share conversation snippets
- Community questions (anonymized)
- Rabbi verification badge (for verified sources)

### Premium Features
- Voice input/output
- Daily Torah insights
- Study partner matching
- Advanced search across all conversations

### Technical Improvements
- Real-time streaming responses
- Mobile app (React Native)
- Offline mode
- Multi-language support (beyond Hebrew)

---

## Success Metrics (MVP)

### Engagement
- Daily Active Users (DAU)
- Messages per user per day
- Conversation length (messages per conversation)
- Retention (D1, D7, D30)

### Quality
- User satisfaction rating
- Citation accuracy (manual review)
- Response time (p50, p95)
- Error rate

### Growth
- Sign-ups per day
- Conversion from landing page
- Referral rate

---

## Timeline Estimate

**Total MVP: 4-6 weeks** (single developer, full-time)

- **Week 1:** Foundation + Auth + Database
- **Week 2:** Backend chat API + Claude integration
- **Week 3:** Frontend chat UI + Personalization
- **Week 4:** Polish + Analytics + Deployment

**Stretch:** Add onboarding, settings, and advanced features in weeks 5-6.

---

## Risk Mitigation

### Technical Risks
- **Claude API rate limits** → Implement queueing, use caching for common questions
- **Database performance** → Index properly, use connection pooling
- **Citation accuracy** → Manual review process, user feedback on citations

### Product Risks
- **Low differentiation** → Double down on personalization features early
- **User trust** → Clear privacy policy, rabbi disclaimer, transparent sourcing
- **Retention** → Focus on relationship-building, not just Q&A

### Business Risks
- **Monetization unclear** → Start free, test freemium after 100+ users
- **Competition** → Ship fast, iterate based on user feedback
- **Scope creep** → Ruthlessly prioritize MVP features, defer everything else

---

## Questions to Answer Before Implementation

1. **What's the minimum viable personalization?**
   - Just denomination + life stage?
   - Or include conversation memory from day 1?

2. **How do we handle source citations?**
   - Trust Claude to cite correctly?
   - Validate against a Torah text database?
   - Manual review process?

3. **What's the pricing model?**
   - Free forever?
   - Freemium (limited messages/month)?
   - Subscription from day 1?

4. **Do we need Hebrew input support?**
   - Or just display Hebrew in responses?

5. **What's the disclaimer strategy?**
   - Show on every response?
   - One-time acknowledgment?
   - Footer only?

---

## Next Steps

1. **Review this plan** with stakeholder (you!)
2. **Answer open questions** above
3. **Finalize MVP scope** (cut anything non-essential)
4. **Begin Phase 1: Foundation**

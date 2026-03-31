# Torah Companion

Your personal AI spiritual companion rooted in Torah wisdom.

## Project Structure

```
torah-companion/
├── frontend/          # React + Vite + TypeScript + Tailwind
├── backend/           # Node + TypeScript + tRPC + Express
├── shared/            # Shared types (if needed)
└── README.md
```

## Tech Stack

### Frontend
- **React** with TypeScript
- **Vite** for build tooling
- **Tailwind CSS** for styling
- **tRPC** for type-safe API calls
- **React Query** for data fetching
- **Firebase Auth** for Google OAuth
- **React Router** for navigation

### Backend
- **Node.js** with TypeScript
- **tRPC** for type-safe API routes
- **Express** for HTTP server
- **Prisma** ORM for database
- **PostgreSQL** database
- **Anthropic Claude** API for AI responses
- **Zod** for validation

### Infrastructure
- **Railway** for hosting (database + backend + frontend)
- **Doppler** for environment variable management (optional)
- **Sentry** for error tracking
- **Mixpanel** for analytics

## Setup Instructions

### Prerequisites
- Node.js 18+ installed
- PostgreSQL database (local or Railway)
- Firebase project created
- Anthropic API key

### 1. Clone the Repository

```bash
git clone https://github.com/QualityProductsEng/butter-torah-companion.git
cd butter-torah-companion
```

### 2. Backend Setup

```bash
cd backend
npm install

# Copy environment variables
cp .env.example .env
# Edit .env with your credentials

# Run database migrations (when Phase 1.2 is complete)
# npx prisma migrate dev

# Start development server
npm run dev
```

Backend will run on `http://localhost:3001`

### 3. Frontend Setup

```bash
cd frontend
npm install

# Copy environment variables
cp .env.example .env
# Edit .env with your Firebase config

# Start development server
npm run dev
```

Frontend will run on `http://localhost:5173`

## Development Workflow

1. **Backend changes**: Edit files in `backend/src/`, server auto-restarts with `tsx watch`
2. **Frontend changes**: Edit files in `frontend/src/`, Vite hot-reloads automatically
3. **Database changes**: Update `backend/prisma/schema.prisma`, run `npx prisma migrate dev`

## Project Phases

- ✅ **Phase 0**: Landing page & market research
- 🔄 **Phase 1.1**: Project setup (IN PROGRESS)
- ⬜ **Phase 1.2**: Database & schema
- ⬜ **Phase 1.3**: Authentication
- ⬜ **Phase 2**: Core chat functionality
- ⬜ **Phase 3**: Frontend UI
- ⬜ **Phase 4**: Personalization
- ⬜ **Phase 5**: Analytics & observability
- ⬜ **Phase 6**: Production deployment

See [IMPLEMENTATION_PLAN.md](./IMPLEMENTATION_PLAN.md) for full details.

## Commands Reference

### Backend
```bash
npm run dev      # Start dev server with hot reload
npm run build    # Compile TypeScript to JavaScript
npm run start    # Run compiled production build
```

### Frontend
```bash
npm run dev      # Start Vite dev server
npm run build    # Build for production
npm run preview  # Preview production build locally
```

## Environment Variables

### Backend (`backend/.env`)
- `DATABASE_URL`: PostgreSQL connection string
- `ANTHROPIC_API_KEY`: Claude API key
- `FIREBASE_PROJECT_ID`: Firebase project ID
- `PORT`: Server port (default 3001)

### Frontend (`frontend/.env`)
- `VITE_FIREBASE_API_KEY`: Firebase API key
- `VITE_FIREBASE_AUTH_DOMAIN`: Firebase auth domain
- `VITE_FIREBASE_PROJECT_ID`: Firebase project ID
- `VITE_FIREBASE_STORAGE_BUCKET`: Firebase storage bucket
- `VITE_FIREBASE_MESSAGING_SENDER_ID`: Firebase messaging sender ID
- `VITE_FIREBASE_APP_ID`: Firebase app ID
- `VITE_API_URL`: Backend API URL

## Contributing

This is a solo founder project focused on rapid validation. See [AGENTS.md](./AGENTS.md) for development guidelines.

## License

Proprietary - All rights reserved

---

**תורה חַבֵּר** • Torah Companion

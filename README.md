# UIGen - AI-Powered React Component Generator

An intelligent React component generator that transforms natural language descriptions into live, interactive React components using Claude AI.

## Prerequisites

- Node.js 18+
- npm

## Setup

1. **Optional** Edit `.env` and add your Anthropic API key:

```
ANTHROPIC_API_KEY=your-api-key-here
```

The project will run without an API key. Rather than using a LLM to generate components, static code will be returned instead.

2. Install dependencies and initialize database

```bash
npm run setup
```

This command will:

- Install all dependencies
- Generate Prisma client
- Run database migrations

## Running the Application

### Development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Usage

1. Sign up or continue as anonymous user
2. Describe the React component you want to create in the chat
3. View generated components in real-time preview
4. Switch to Code view to see and edit the generated files
5. Continue iterating with the AI to refine your components

## Features

- AI-powered component generation using Claude
- Live preview with hot reload
- Virtual file system (no files written to disk)
- Syntax highlighting and code editor
- Component persistence for registered users
- Export generated code

## Tech Stack

- Next.js 15 with App Router
- React 19
- TypeScript
- Tailwind CSS v4
- Prisma with SQLite
- Anthropic Claude AI
- Vercel AI SDK

## 🏗️ Architecture Overview

**UIGen** is a **React/Next.js AI code generator** that lets users create and preview React components through natural language chat with Claude AI.

## 📁 Project Structure

```
src/
├── app/           # Next.js App Router (pages, layouts, API routes)
├── components/    # React components (chat, preview, editor, auth)
├── lib/           # Core business logic & utilities
├── actions/       # Server actions for database operations
├── hooks/         # Custom React hooks
└── prisma/        # Database schema & migrations
```

## ⚡ Tech Stack

- **Frontend:** Next.js 15 + React 19 + TypeScript
- **Styling:** Tailwind CSS v4 + Radix UI components
- **AI:** Anthropic Claude API + Vercel AI SDK
- **Database:** Prisma ORM + SQLite
- **Code Processing:** Monaco Editor + Babel for JSX transformation
- **Testing:** Vitest + Testing Library

## 🧩 Key Moving Parts

### 1. Virtual File System
- **In-memory file management** - no real files, everything in memory
- **VirtualFileSystem class** handles CRUD operations
- **FileSystemContext** provides React access to file state
- **AI tools** (`str_replace_editor`, `file_manager`) modify files

### 2. AI Chat System
- **ChatInterface** component for user interaction
- **Streaming responses** from Claude API with tool calling
- **Real-time code generation** that updates the virtual file system
- **Mock provider** fallback when no API key is provided

### 3. Live Preview System
- **PreviewFrame** component renders generated React components
- **JSX → ES Modules** transformation using Babel
- **Blob URLs + Import Maps** for dynamic module loading
- **Tailwind CSS** integration in iframe preview

### 4. Authentication Flow
- **JWT-based auth** with HTTP-only cookies
- **Anonymous-first design** - users can work without registration
- **Work preservation** - anonymous work transfers to account on signup
- **Project persistence** for authenticated users

### 5. Database Layer
```sql
User ↔ Project (stores messages + virtual file system as JSON)
```

## 🔄 Data Flow

1. **User types prompt** → ChatContext processes input
2. **Claude AI responds** with streaming text + tool calls
3. **Tool calls modify** virtual file system (create/edit files)
4. **File changes trigger** preview regeneration
5. **JSX transforms** to ES modules via Babel
6. **Preview updates** in real-time via iframe
7. **Messages persist** to database (if authenticated)

## 🎯 Key Features

- **No file system required** - everything runs in memory
- **Real-time preview** of generated React components
- **Work without API key** using mock responses
- **Seamless auth flow** that preserves anonymous work
- **Streaming AI responses** with live code generation
- **Full TypeScript support** throughout

## 🚀 How It All Works Together

1. User chats with AI about what component they want
2. Claude generates code using specialized tools
3. Virtual file system updates with new/modified files
4. JSX gets transformed and loaded dynamically in preview
5. User sees their component render live
6. Conversation and files persist if user is authenticated

This creates a **seamless AI-powered development experience** where users can describe UI components in natural language and see them generated and previewed instantly.

## 🛠️ Detailed Component Architecture

### Core UI Components
- **`MainContent`** - Main layout with resizable panels
- **`ChatInterface`** - AI chat functionality
- **`PreviewFrame`** - Live component preview using iframe
- **`CodeEditor`** - Monaco-based code editor
- **`FileTree`** - Virtual file system browser

### Authentication Components
- **`AuthDialog`** - Login/signup modal
- **`SignInForm`** & **`SignUpForm`** - Authentication forms
- **`HeaderActions`** - User actions and project management

## 🔌 API Routes

### `/api/chat` - Core AI Chat Endpoint
- Handles streaming responses from Claude API
- Processes tool calls for file operations
- Saves conversation history to database
- Falls back to mock provider without API key

**Key Features:**
- Streaming text generation
- Tool calling for file system operations
- Message persistence for authenticated users
- Mock provider for demo mode

## 📊 State Management

### Context Providers
- **`FileSystemProvider`** - Manages virtual file system state
- **`ChatProvider`** - Handles AI chat state and streaming

### Key Hooks
- **`useFileSystem()`** - File operations and virtual FS access
- **`useChat()`** - Chat state and message handling
- **`useAuth()`** - Authentication workflow management

## 🔐 Authentication System

### JWT-based Authentication
- **Session creation** with 7-day expiry
- **HTTP-only cookies** for security
- **Middleware protection** for sensitive routes
- **Anonymous user support** with work preservation

### User Flow
- Anonymous users can create and preview components
- Authentication preserves anonymous work
- Project persistence requires user account
- Automatic project creation and routing

## 💾 Database Schema

### Models
```sql
User {
  id: String (cuid)
  email: String (unique)
  password: String (hashed)
  projects: Project[]
}

Project {
  id: String (cuid)
  name: String
  userId: String? (optional for anonymous)
  messages: String (JSON)
  data: String (JSON - virtual file system)
  user: User?
}
```

## 🧰 Tools & Utilities

### Core Libraries
- **`VirtualFileSystem`** - In-memory file system with full CRUD operations
- **`jsx-transformer`** - Babel-based JSX to ES modules transformation
- **`str-replace` & `file-manager` tools** - AI tooling for code generation
- **Anonymous work tracker** - Preserves work for non-authenticated users

### Key Transformations
- **JSX → ES Modules** with import map generation
- **Blob URLs** for dynamic module loading
- **Tailwind CSS** integration in preview iframe
- **Error boundary** handling for preview errors

## 🧪 Testing Infrastructure

### Test Setup
- **Vitest** with jsdom environment
- **React Testing Library** for component tests
- **Test coverage** for core functionality
- **TypeScript** support in tests

### Test Organization
- Component tests in `__tests__/` directories
- Integration tests for core features
- Mock implementations for external dependencies

## ⚙️ Configuration & Build

### Build Tools
- **Next.js** with Turbo mode for development
- **TypeScript** with strict mode enabled
- **ESLint** for code quality
- **PostCSS** with Tailwind CSS

### Environment Setup
- **Optional Anthropic API key** (falls back to mock)
- **Development-focused** configuration
- **SQLite** for simple local development

## 🏛️ Key Architectural Patterns

1. **Virtual File System** - Enables in-memory code generation and preview
2. **Streaming AI Responses** - Real-time code generation with tool calling
3. **Context-driven State** - React Context for global state management
4. **Dynamic Module Loading** - Blob URLs with import maps for live preview
5. **Anonymous-first Design** - Users can work without registration
6. **Progressive Enhancement** - Works without API key using mock provider


## 🤝 Contributing

1. Create a new branch from `main`
2. Make your changes
3. Write tests for new functionality
4. Create a Pull Request with a clear description

## 📄 License

This project is licensed under the MIT License.
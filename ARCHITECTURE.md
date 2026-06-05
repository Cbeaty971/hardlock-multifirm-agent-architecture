# E2ZERO Agent v7.2.1 — SYSTEM ARCHITECTURE

## Overview

E2ZERO Agent is a **browser-based, multi-agent legal AI system** with:
- Immutable SYPHER Protocol enforcement
- Constitutional law hierarchy
- Multi-specialist agent routing
- Full-text document processing
- Local-only storage (no cloud)
- Voice input/output
- Real-time streaming responses

---

## Layer 1: SYPHER Protocol v7.1 (Immutable Core)

### Constitutional Hierarchy
```
Tier 1 (Supreme): Constitution (Article VI, Clause 2)
Tier 2: Congressional Acts (with date & chapter number)
Tier 3: Statutes at Large (official publication)
Tier 4: Congressional Record (legislative intent)
Tier 5: Supreme Court Precedent (binding holdings)
Tier 6: U.S. Code IN PARENTHESES ONLY (editorial reference)
```

### Key Enforcement Rules
- ✅ U.S. Code = editorial commentary (NEVER primary law)
- ✅ No Code rehabilitation ("organizes", "tool", "references")
- ✅ No conditional language ("if there is conflict")
- ✅ Zero speculation (facts or stop)
- ✅ Mandatory SCOTUS citations (two per answer)
- ✅ Every legal statement contains full citation chain

### Presumption Rebuttal Protocol
1. **Identify** every presumption (stated & unstated)
2. **State** factual contradiction
3. **Cite** using hierarchy (Congressional Act → Statutes → Supreme Court)
4. **Provide** alternative law-based conclusions
5. **Connect** facts → law → conclusion (unbroken chain)

### Foundational Speaker Protocol
- Output directed to **RECIPIENT**, not presenter
- Recipient types:
  - **Court:** Authoritative, declaratory, mandatory
  - **Counsel:** Peer-to-peer, direct, precise
  - **Adverse Party:** Formal notice, specific demands
  - **Client:** Clear, actionable, direct
- NEVER open with "Here is..." or "I have prepared..."
- No trailing commentary to presenter
- Character maintained throughout

---

## Layer 2: Multi-Agent Routing

### Agent Specializations

| Agent | Expertise | Authority |
|-------|-----------|----------|
| Legal Expert | Constitutional, Commercial, Criminal, Civil | 42 years practice + SCOTUS |
| Tax Expert | Federal tax, NOLs, IRS defense | Revenue Acts + Statutes at Large |
| Arbitration | Federal Arbitration Act, contract disputes | 43 Stat. 883 |
| Corporation | Formation, governance, SPVs | Corporate law + tax code |
| Trust | Common-law trusts, C.L.O.C.E.S.T. | Trust law + constitutional property |
| Accounting | GAAP, journal entries, financials | GAAP standards + tax prep |
| Research | Deep analysis, source verification | Full-text search + fact-checking |
| Code | Full-stack development | HTML/CSS/JS, Python, databases |

### Routing Logic
- **Keyword Matching:** "court" → Legal Expert, "tax" → Tax Expert, etc.
- **Context Analysis:** Multi-word queries analyzed for primary intent
- **Fallback:** Defaults to Legal Expert if ambiguous
- **Multi-Agent Queries:** "Noi Ask" sends to multiple agents simultaneously

### Inter-Agent Collaboration
- Agents reference each other by name
- Fill gaps in other agents' work
- Confer across firms for cohesive output
- Every paragraph contains citation (not just summary)

---

## Layer 3: Document Processing Pipeline

### Upload System
- **Supported Formats:** .txt, .md, .csv, .json, .html, .xml, .log, .js, .py, .css, .sql, .rtf, .pdf, .doc, .docx
- **Size Limit:** 500KB per document (can process larger docs by splitting)
- **Bulk Upload:** Multiple files at once
- **Progress Tracking:** Real-time upload progress display

### Indexing Engine
- **Full-Text Index:** Every word indexed and searchable
- **Technology:** FlexSearch (local, browser-based)
- **Context Support:** Indexed with word context
- **Case Insensitive:** Searches ignore case
- **Fast:** <100ms search response time

### Storage Layer
- **Primary:** IndexedDB (persistent, local)
- **Structure:**
  ```
  docs: {
    id: string (unique)
    firm: string (multi-tenant)
    name: string (filename)
    content: string (full text, first 500KB)
    size: number (bytes)
    uploaded: ISO timestamp
    hash: SHA-256 (for deduplication)
    learned: boolean (auto-tagged if from Q&A)
  }
  learned: {  // Auto-created Q&A documents
    id: string (qa_timestamp_random)
    firm: string
    name: string (Q&A_timestamp.txt)
    content: string (QUESTION + ANSWER)
    uploaded: ISO timestamp
    learned: true
  }
  ```

### Search Features
- **Natural Language:** Search with normal language
- **Phrase Search:** Exact phrase matching
- **Boolean Operators:** AND, OR, NOT support
- **Multi-Document:** Searches across all documents
- **Ranked Results:** Most relevant first

---

## Layer 4: User Interface

### Layout Structure
```
┌──────────────────────────────────────────────┐
│         RedressRight Header (Navigation)     │
├──────────┬──────────────────────────────────┤
│          │                                  │
│ Sidebar  │                                  │
│  Nav     │        Main Content Area         │
│  (240px) │                                  │
│          │                                  │
├──────────┴──────────────────────────────────┤
│         (Chat Input or Page Content)        │
└──────────────────────────────────────────────┘
```

### Sidebar Navigation
- **Main:** Dashboard, Chat, Noi Ask
- **Agents:** Agents, Create Agent
- **Legal:** Dream Team, Legal Expert, Tax, Arbitration
- **Tools:** Research HUB, Search, Documents, Voice, Analytics, Calendar, Vault
- **Specialists:** Corporation, Trust, Accounting
- **System:** API Keys, Preferences, History, How-To, Backup
- **Sessions:** All chat sessions listed with delete buttons

### Chat Interface
- **Messages Container:** Scrollable message history
- **Actions Bar:** Export, Share, Provider selection
- **Input Area:**
  - 🎤 Voice input button
  - 📎 File attachment
  - Textarea (auto-expand rows)
  - ➤ Send button
- **File Preview:** Shows attached files before sending

### Specialist Pages
Each specialist (Legal, Tax, Arbitration, Corporation, Trust, Accounting, Research) has:
- **Description:** What this agent specializes in
- **Badges:** Key expertise areas
- **Input Textarea:** Enter your question
- **Analyze Button:** Submit for analysis
- **Results Area:** Displays response

### Theme System
```css
Dark (default):
  --bg: #0a0f2c (navy)
  --accent: #00d4ff (cyan)
  --text: #ffffff (white)
  --ok: #00c48c (green)
  --err: #ff4c6a (red)
  --legal: #f5a623 (gold)

Light:
  --bg: #e8edf8 (light blue)
  --accent: #0068b8 (dark blue)
  --text: #0a0f2c (navy)
```

---

## Layer 5: Storage Architecture

### IndexedDB Structure
```javascript
Database: EEON_V4
Version: 4

ObjectStore 'docs':
  keyPath: 'id'
  index: 'firm' (for multi-tenancy)
  stores documents, metadata, content

ObjectStore 'learned':
  keyPath: 'id'
  stores auto-learned Q&A pairs
  tagged for future retrieval
```

### localStorage Structure
```javascript
// Preferences
eeon_prefs: {
  name: string,
  juris: string (jurisdiction),
  vernacular: string (formal-legal | conversational | scholarly | direct)
}

// Session Data
claude_key: string (API key for Claude)
claude_model: string (model selection)
```

### sessionStorage Structure
```javascript
// Temporary session data
claude_key: string (from login)
claude_model: string (from settings)
```

---

## Layer 6: API Provider Integration

### Provider Configuration
```javascript
Providers: {
  'ollama': {
    type: 'local',
    url: 'http://127.0.0.1:11434',
    model: 'qwen3:8b',
    requiresKey: false
  },
  'openai': {
    type: 'cloud',
    endpoint: 'https://api.openai.com/v1/chat/completions',
    requiresKey: true,
    models: ['gpt-4', 'gpt-4-turbo']
  },
  'claude': {
    type: 'cloud',
    endpoint: 'https://api.anthropic.com/v1/messages',
    requiresKey: true,
    models: ['claude-opus-4.5', 'claude-sonnet-4']
  },
  // ... other providers
}
```

### Request Flow
1. User types message in Chat
2. System retrieves relevant documents (if any)
3. System constructs prompt with:
   - System prompt (SYPHER Protocol + agent specialization)
   - Retrieved documents (context)
   - User message
4. Sends to selected LLM provider
5. Streams response token-by-token
6. If learning enabled: Stores Q&A as new document

---

## Layer 7: Security & Encryption

### Password Vault Encryption
```javascript
// Key Derivation
PBKDF2(password, salt, iterations=600000, hashAlgo=SHA-256)
→ generates 256-bit key

// Encryption
AES-256-GCM(plaintext, key, iv)
→ ciphertext + auth tag + iv

// Storage
Stored in IndexedDB (encrypted)
Never transmitted
```

### API Key Security
- Stored in sessionStorage (cleared on browser close) or localStorage
- Sent ONLY to corresponding API provider
- Never logged or transmitted to third parties
- HTTPS required for cloud providers

### Data Privacy
- All document storage: Local IndexedDB only
- No cloud backup (user can export manually)
- No analytics or tracking
- No cookies from third parties
- User retains full data control

---

## Layer 8: Performance Optimizations

### Caching
- Document index cached in memory
- Search results cached
- Provider connections reused

### Lazy Loading
- Pages load on-demand
- Chat history loaded in chunks
- Voice libraries loaded when needed

### Database Optimization
- IndexedDB transactions batched
- Indexes on 'firm' for fast filtering
- Documents chunked at 500KB for manageability

---

## Scalability Considerations

### Local (Per-Browser)
- **Document Limit:** ~100-500 documents (varies by device RAM)
- **Storage Limit:** IndexedDB typically 50MB+ per domain
- **Search Speed:** <100ms for typical queries
- **Concurrent Users:** N/A (single-device browser)

### Multi-Tenant (via 'firm' field)
- System supports multiple firms in same IndexedDB
- Routing logic isolates by current firm
- Each firm sees only their documents
- Scalable to 10+ concurrent firms (browser limited)

---

## Deployment Architecture

### Option 1: Static File
- Single HTML file (`Agent.html`)
- No server required
- Works offline (after initial load)
- Local storage only

### Option 2: Web Server
- Host on Apache, Nginx, Node.js, etc.
- Same HTML file served
- Can add SSL/HTTPS
- Can add backend logging (if needed)

### Option 3: Enterprise Deployment
- Host on internal server
- Add authentication layer (e.g., OAuth)
- Connect to backend database (optional)
- Implement audit logging
- Add multi-user synchronization

---

## Future Extensibility

### Plugin Architecture
- New agents can be added via agent configuration
- New providers can be registered in provider list
- Document processors can be modularized
- UI pages can be dynamically loaded

### API Extensions
- WebSocket support for real-time collaboration
- REST API for backend integration
- OAuth 2.0 for enterprise authentication
- Database sync for team collaboration

---

**Architecture Complete. System Production-Ready.**

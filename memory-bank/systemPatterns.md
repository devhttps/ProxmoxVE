# System Patterns - ProxmoxVE Helper-Scripts

## Architecture Overview

### High-Level Architecture
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Backend API   │    │   Scripts       │
│   (Next.js)     │◄──►│   (Go + MongoDB)│◄──►│   (Bash)        │
│                 │    │                 │    │                 │
│ • Dashboard     │    │ • Data Collection│    │ • VM Creation   │
│ • Script Catalog│    │ • Analytics      │    │ • LXC Setup     │
│ • Search        │    │ • Status Updates │    │ • App Install   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Component Relationships
- **Frontend ↔ Backend**: REST API para dados e métricas
- **Scripts ↔ Backend**: POST requests para tracking de instalações
- **Frontend ↔ Scripts**: Links diretos para comandos de instalação

## Key Technical Decisions

### Backend Architecture
**Decision**: Go + MongoDB + Gorilla Mux
**Rationale**: 
- Go: Performance e concorrência para API
- MongoDB: Flexibilidade para dados de instalação
- Gorilla Mux: Roteamento robusto e middleware

**Pattern**: RESTful API com endpoints específicos
```go
/upload              - POST para novos dados
/data/paginated      - GET com paginação
/data/summary        - GET para estatísticas
/data/nsapp          - GET filtrado por aplicação
```

### Frontend Architecture
**Decision**: Next.js 15 + App Router + React 19
**Rationale**:
- Next.js: SSR/SSG para performance
- App Router: Roteamento moderno
- React 19: Latest features e performance

**Pattern**: Component-based architecture
```
src/
├── app/                 # App Router pages
├── components/          # Reusable components
├── lib/                 # Utilities and types
└── config/             # Configuration files
```

### Script Architecture
**Decision**: Bash scripts com funções utilitárias centralizadas
**Rationale**:
- Bash: Nativo no Linux/Proxmox
- Modularidade: Funções reutilizáveis
- Consistência: Padrões unificados

**Pattern**: Template-based script generation
```bash
source /dev/stdin <<<$(curl -fsSL https://raw.githubusercontent.com/.../core.func)
```

## Design Patterns

### 1. Error Handling Pattern
**Location**: Scripts e API
**Pattern**: Graceful degradation com cleanup
```bash
trap 'error_handler $LINENO "$BASH_COMMAND"' ERR
trap cleanup EXIT
```

### 2. Data Collection Pattern
**Location**: Scripts → API
**Pattern**: Async tracking com retry
```bash
post_update_to_api "status" "message"
```

### 3. UI State Management Pattern
**Location**: Frontend
**Pattern**: URL-based state com nuqs
```typescript
const [selectedScript, setSelectedScript] = useQueryState("id")
```

### 4. Configuration Pattern
**Location**: All components
**Pattern**: Environment-based configuration
```typescript
export const basePath = process.env.BASE_PATH || ""
```

## Component Relationships

### Frontend Components
```
App Layout
├── Navbar
│   ├── CommandMenu
│   ├── ThemeToggle
│   └── GitHubStarsButton
├── Main Content
│   ├── ScriptCatalog
│   ├── DataDashboard
│   └── ApplicationChart
└── Footer
```

### API Endpoints
```
DataModel (MongoDB Collection)
├── UploadJSON
├── UpdateStatus
├── GetPaginatedData
├── GetSummary
├── GetByNsapp
├── GetByDateRange
├── GetByStatus
├── GetByOS
└── GetErrors
```

### Script Categories
```
Scripts/
├── tools/pve/          # Proxmox VE management
├── tools/addon/         # Application installations
├── vm/                  # VM creation scripts
├── ct/                  # LXC container scripts
└── misc/                # Utility functions
```

## Data Flow Patterns

### Installation Flow
1. User copies installation command
2. Script downloads and executes
3. Script posts progress to API
4. API stores data in MongoDB
5. Frontend displays updated metrics

### Data Query Flow
1. Frontend requests data from API
2. API queries MongoDB
3. Data is paginated/filtered
4. JSON response sent to frontend
5. Frontend renders charts/tables

## Security Patterns

### API Security
- CORS configured for cross-origin requests
- Input validation on all endpoints
- No authentication (public metrics only)

### Script Security
- Downloads from trusted sources only
- Validation of system requirements
- Cleanup on failure/interruption

## Performance Patterns

### Frontend Performance
- Static generation where possible
- Image optimization with Next.js
- Lazy loading for heavy components
- Tailwind CSS for minimal bundle size

### Backend Performance
- Connection pooling for MongoDB
- Pagination for large datasets
- Timeout handling for long operations

### Script Performance
- Parallel downloads where possible
- Progress indicators for long operations
- Efficient error handling

## Monitoring Patterns

### Application Monitoring
- Umami analytics for frontend
- API logging for backend
- Script execution tracking

### Error Monitoring
- Structured error logging
- Error aggregation in dashboard
- User feedback collection

## Evolution Patterns

### Version Management
- Semantic versioning for releases
- Changelog maintenance
- Backward compatibility considerations

### Community Contribution
- Clear contribution guidelines
- Automated testing where possible
- Code review process


# Technical Context - ProxmoxVE Helper-Scripts

## Technology Stack

### Backend Stack
- **Language**: Go 1.23.2
- **Framework**: Gorilla Mux v1.8.1
- **Database**: MongoDB v1.17.2
- **Environment**: godotenv v1.5.1
- **CORS**: rs/cors v1.11.1

### Frontend Stack
- **Framework**: Next.js 15.5.2
- **Runtime**: React 19.0.0
- **Styling**: Tailwind CSS 3.4.17
- **UI Components**: Radix UI primitives
- **State Management**: TanStack Query 5.71.1
- **URL State**: nuqs 2.4.1
- **Charts**: Chart.js 4.4.8 + react-chartjs-2
- **Animations**: Framer Motion 11.18.2

### Scripts Stack
- **Language**: Bash
- **UI Framework**: whiptail (dialog boxes)
- **Utilities**: curl, jq, systemctl
- **Package Managers**: apt-get, apk

## Development Environment

### Prerequisites
- **Node.js**: 18+ (for frontend development)
- **Go**: 1.23+ (for backend development)
- **MongoDB**: 4.4+ (for data storage)
- **Proxmox VE**: 8.x or 9.x (for script execution)

### Frontend Development Setup
```bash
cd frontend
npm install
npm run dev          # Development server
npm run build        # Production build
npm run lint         # ESLint checking
npm run typecheck    # TypeScript checking
```

### Backend Development Setup
```bash
cd api
go mod download
go run main.go       # Development server
```

### Script Development
- Scripts run directly on Proxmox VE hosts
- No special development environment required
- Testing done on actual Proxmox installations

## Technical Constraints

### Proxmox VE Compatibility
- **Minimum Version**: Proxmox VE 8.0
- **Recommended**: Proxmox VE 8.4+ or 9.0
- **Architecture**: x86_64 only
- **OS**: Debian-based (Bookworm/Trixie)

### Browser Compatibility
- **Modern Browsers**: Chrome 90+, Firefox 88+, Safari 14+
- **Mobile**: iOS Safari 14+, Chrome Mobile 90+
- **Features**: ES2020+, CSS Grid, Flexbox

### Network Requirements
- **API Access**: HTTPS required for production
- **Script Downloads**: GitHub raw content access
- **MongoDB**: Network connectivity required

## Dependencies

### Critical Dependencies
```json
// Frontend - Critical
"next": "15.5.2"                    // Core framework
"react": "19.0.0"                   // UI library
"tailwindcss": "^3.4.17"           // Styling
"@tanstack/react-query": "^5.71.1" // State management
```

```go
// Backend - Critical
"github.com/gorilla/mux" "v1.8.1"     // HTTP router
"go.mongodb.org/mongo-driver" "v1.17.2" // Database
"github.com/rs/cors" "v1.11.1"        // CORS handling
```

### Optional Dependencies
- **Analytics**: Umami (self-hosted)
- **Monitoring**: Built-in logging
- **Testing**: Manual testing only

## Configuration Management

### Environment Variables
```bash
# Backend (.env)
MONGO_USER=username
MONGO_PASSWORD=password
MONGO_IP=10.10.10.18
MONGO_PORT=27017
MONGO_DATABASE=proxmox_data
```

```bash
# Frontend (next.config.mjs)
BASE_PATH=ProxmoxVE
```

### Build Configuration
- **Frontend**: Static export for GitHub Pages
- **Backend**: Single binary deployment
- **Scripts**: Direct execution on target systems

## Performance Considerations

### Frontend Performance
- **Bundle Size**: Optimized with Next.js
- **Images**: WebP format with fallbacks
- **Caching**: Static generation where possible
- **CDN**: GitHub Pages for static assets

### Backend Performance
- **Database**: Connection pooling
- **API**: Pagination for large datasets
- **Memory**: Efficient Go memory management
- **Concurrency**: Goroutines for parallel processing

### Script Performance
- **Downloads**: Parallel where possible
- **Progress**: Real-time feedback
- **Cleanup**: Automatic resource management
- **Error Handling**: Fast failure detection

## Security Considerations

### API Security
- **CORS**: Configured for specific origins
- **Input Validation**: All endpoints validated
- **Rate Limiting**: Not implemented (public metrics)
- **Authentication**: Not required (public data)

### Script Security
- **Source Validation**: Trusted GitHub repositories only
- **Download Integrity**: SHA checksums where available
- **Execution Context**: Limited privileges
- **Cleanup**: Automatic on failure

### Frontend Security
- **CSP**: Content Security Policy headers
- **XSS**: React's built-in protection
- **HTTPS**: Required for production
- **Dependencies**: Regular security updates

## Deployment Architecture

### Production Deployment
```
GitHub Pages (Frontend)
├── Static files served via CDN
├── Custom domain support
└── HTTPS enforced

Self-hosted Backend
├── Go binary on VPS/cloud
├── MongoDB database
└── Reverse proxy (nginx)

Scripts
├── Executed on Proxmox hosts
├── No installation required
└── Direct GitHub execution
```

### Development Deployment
- **Frontend**: Local development server (localhost:3000)
- **Backend**: Local Go server (localhost:8080)
- **Database**: Local MongoDB instance
- **Scripts**: Tested on development Proxmox

## Monitoring and Observability

### Application Monitoring
- **Frontend**: Umami analytics
- **Backend**: Structured logging
- **Scripts**: API tracking integration

### Error Tracking
- **Frontend**: Browser console + Umami
- **Backend**: Structured error logs
- **Scripts**: Error reporting to API

### Performance Monitoring
- **Frontend**: Core Web Vitals
- **Backend**: Response time tracking
- **Scripts**: Execution time logging

## Future Technical Considerations

### Scalability
- **Database**: MongoDB sharding if needed
- **API**: Load balancing for high traffic
- **CDN**: Global distribution for frontend

### Technology Evolution
- **Frontend**: React 19+ features adoption
- **Backend**: Go 1.24+ performance improvements
- **Scripts**: Bash alternatives evaluation

### Integration Opportunities
- **Proxmox API**: Direct integration possibilities
- **Monitoring**: Prometheus/Grafana integration
- **CI/CD**: Automated testing and deployment


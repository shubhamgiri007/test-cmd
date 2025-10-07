# AWS Security Scanner

A production-ready, AI-powered AWS security scanning application that demonstrates end-to-end security analysis with generative AI explanations and remediation guidance.

## Quick Start

### Prerequisites
- Node.js 18+ 
- npm or yarn

### Setup (≤ 5 minutes)

1. **Clone and install dependencies:**
```bash
git clone <repository-url>
cd aws-security-scanner

# Install API dependencies
cd api && npm install

# Install Web dependencies  
cd ../web && npm install
```

**Note**: The project is only ~1.2MB (excluding node_modules). Build artifacts and caches are properly gitignored.

2. **Start the application:**
```bash
# Terminal 1: Start API server
cd api && npm run dev

# Terminal 2: Start Web application
cd web && npm run dev
```

3. **Access the application:**
- Frontend: http://localhost:3000
- API: http://localhost:3001
- Health check: http://localhost:3001/health

## Architecture

### Backend API (`/api`)
- **Framework:** Node.js + TypeScript + Express
- **Database:** SQLite (in-memory for demo, easily configurable for production)
- **Security:** Helmet, CORS, input validation with Zod
- **Features:**
  - 5 AWS security detection rules
  - Deterministic AI explanations (no external API calls)
  - RESTful API with pagination and filtering
  - Comprehensive error handling

### Frontend (`/web`)
- **Framework:** Next.js 14 + React + TypeScript
- **Styling:** Tailwind CSS with custom design system
- **Components:** Modern, accessible UI components
- **Features:**
  - Real-time dashboard with severity tiles
  - Advanced findings table with search/filter
  - Modal-based finding details with AI explanations
  - Responsive design with dark mode support

### Security Rules Implemented
1. **S3 Public Access** (HIGH) - Detects public buckets without PublicAllowed tag
2. **Security Group Open Ports** (CRITICAL) - Flags unrestricted access to SSH/RDP/MySQL
3. **IAM Access Key Age** (MEDIUM) - Identifies keys older than 90 days
4. **RDS Encryption** (HIGH) - Detects unencrypted database instances
5. **EC2 IAM Role** (MEDIUM) - Finds instances without attached IAM roles

## Key Features

### AI-Powered Security Analysis
- **Deterministic explanations:** No external API dependencies, consistent results
- **Contextual remediation:** 3-step actionable guidance per finding
- **Resource-specific insights:** Tailored explanations based on resource type and configuration

### Production-Ready UI/UX
- **Performance:** Optimized for 200-500+ findings with client-side filtering
- **Accessibility:** Semantic HTML, keyboard navigation, ARIA labels
- **Polish:** Micro-interactions, loading states, error boundaries
- **Responsive:** Mobile-first design with 8pt grid system

### Developer Experience
- **Type Safety:** Full TypeScript coverage with strict mode
- **Testing:** Comprehensive unit tests for core logic
- **Error Handling:** Graceful degradation and user feedback
- **Documentation:** Clear API contracts and component interfaces

## Testing

```bash
# Run all tests
cd api && npm test

# Run tests in watch mode
cd api && npm run test:watch

# Test coverage
cd api && npm test -- --coverage
```

**Test Coverage:**
- Security rule detection logic (5 rules)
- API endpoint validation and error handling
- Resource normalization and data transformation
- AI explanation generation (non-empty, contextual)

## API Endpoints

### `POST /api/scan`
Scans AWS resources and returns finding count.
```json
{
  "resources": [
    {
      "type": "s3",
      "name": "logs-bucket", 
      "public": true,
      "account_id": "111111111111"
    }
  ]
}
```

### `GET /api/findings`
List findings with filtering and pagination.
```
GET /api/findings?severity=HIGH&resource_type=s3&page=1&limit=20
```

### `GET /api/findings/:id`
Get detailed finding with AI explanation and remediation steps.

## Design Choices & Trade-offs

### Architecture Decisions
- **SQLite over PostgreSQL:** Faster setup, sufficient for demo, easy production migration
- **In-memory database:** No persistence needed for demo, resets on restart
- **Deterministic AI:** No external dependencies, consistent results, faster execution
- **Client-side filtering:** Better UX for small-medium datasets, reduces server load

### UI/UX Trade-offs
- **Modal over separate page:** Better context preservation, faster navigation
- **Table over cards:** More data density, better for large datasets
- **Static mock data:** Demonstrates functionality without AWS integration complexity
- **Tailwind over custom CSS:** Faster development, consistent design system

### Security Considerations
- **Input validation:** Zod schemas prevent injection attacks
- **CORS configuration:** Restricts frontend origins
- **Helmet middleware:** Security headers and XSS protection
- **Error sanitization:** No sensitive data in error responses

## Development Timeline

**Time Spent:** ~8 hours
- Backend API & security rules: 3 hours
- Frontend UI & components: 3 hours  
- Testing & documentation: 2 hours

## Next Steps (1 Extra Day)

### Immediate Improvements
- **AWS Integration:** Real CloudFormation/Config scanning
- **Authentication:** JWT-based auth with role-based access
- **Persistence:** PostgreSQL with proper migrations
- **Real-time Updates:** WebSocket notifications for new findings

### Production Enhancements
- **Monitoring:** Prometheus metrics and health checks
- **Logging:** Structured logging with correlation IDs
- **Rate Limiting:** API throttling and abuse prevention
- **Caching:** Redis for frequently accessed data

### Advanced Features
- **Custom Rules:** User-defined security policies
- **Compliance Mapping:** SOC2, PCI-DSS, HIPAA frameworks
- **Remediation Automation:** Terraform/CloudFormation templates
- **Trend Analysis:** Historical data and security posture trends

## Configuration

### Environment Variables
```bash
# API
PORT=3001
FRONTEND_URL=http://localhost:3000

# Web  
NEXT_PUBLIC_API_URL=http://localhost:3001/api
```

### Database Configuration
The application uses SQLite by default. For production, update `src/database/database.ts`:
```typescript
const db = new Database('production.db'); // File-based
// or
const db = new Database(process.env.DATABASE_URL); // PostgreSQL
```

## 📝 License

MIT License - see LICENSE file for details.

# Web Application Development Guidelines

This document contains critical information about developing web applications with FastAPI backend and React frontend. Follow these guidelines precisely.

## Architecture Overview

### Backend: FastAPI
- Python 3.11+ with type hints
- Async/await for all I/O operations
- Pydantic for data validation
- SQLAlchemy for ORM (when needed)
- Alembic for database migrations

### Frontend: React
- TypeScript strict mode
- Functional components with hooks
- React Query for server state
- React Router for navigation
- CSS-in-JS or CSS Modules for styling

## Backend Development (FastAPI)

### Project Structure
```
backend/
├── app/
│   ├── api/
│   │   ├── v1/
│   │   │   ├── endpoints/
│   │   │   └── __init__.py
│   │   └── deps.py
│   ├── core/
│   │   ├── config.py
│   │   └── security.py
│   ├── models/
│   ├── schemas/
│   ├── services/
│   └── main.py
├── tests/
├── alembic/
└── requirements.txt
```

### Core Principles
1. API Design
   - RESTful endpoints
   - Consistent naming: `/api/v1/resources`
   - Use HTTP status codes correctly
   - Pagination for list endpoints
   - Proper error responses with detail

2. Data Validation
   - Pydantic models for all requests/responses
   - Validate at the edge (API layer)
   - Use validators for business logic
   - Return meaningful error messages

3. Security
   - JWT tokens for authentication
   - OAuth2 with Password flow
   - CORS configuration
   - Rate limiting
   - Input sanitization

### FastAPI Best Practices
```python
# Dependency injection for database sessions
async def get_db():
    async with SessionLocal() as session:
        yield session

# Pydantic models with examples
class UserCreate(BaseModel):
    email: EmailStr
    password: str

    class Config:
        schema_extra = {
            "example": {
                "email": "user@example.com",
                "password": "strongpassword"
            }
        }

# Proper error handling
@router.post("/users", response_model=User)
async def create_user(
    user: UserCreate,
    db: AsyncSession = Depends(get_db)
):
    try:
        return await user_service.create(db, user)
    except IntegrityError:
        raise HTTPException(
            status_code=400,
            detail="User with this email already exists"
        )
```

### Database Guidelines
- Use async SQLAlchemy
- Connection pooling configuration
- Proper indexing strategy
- Migration scripts for all schema changes
- Soft deletes where appropriate

## Frontend Development (React + TypeScript)

### Project Structure
```
frontend/
├── src/
│   ├── components/
│   │   ├── common/
│   │   └── features/
│   ├── hooks/
│   ├── pages/
│   ├── services/
│   ├── store/
│   ├── types/
│   ├── utils/
│   └── App.tsx
├── public/
└── package.json
```

### React Best Practices
1. Component Design
   - Small, focused components
   - Props interfaces for all components
   - Custom hooks for logic reuse
   - Memoization where needed
   - Error boundaries for fault tolerance

2. State Management
   - Local state for UI state
   - React Query for server state
   - Context for global UI state
   - Avoid prop drilling

3. TypeScript Integration
   ```typescript
   // Proper typing for components
   interface ButtonProps {
     onClick: () => void;
     children: React.ReactNode;
     variant?: 'primary' | 'secondary';
     disabled?: boolean;
   }

   export const Button: React.FC<ButtonProps> = ({
     onClick,
     children,
     variant = 'primary',
     disabled = false
   }) => {
     // Component implementation
   };
   ```

### API Integration
```typescript
// API service with proper typing
class ApiService {
  private baseURL = process.env.REACT_APP_API_URL;

  async request<T>(
    endpoint: string,
    options?: RequestInit
  ): Promise<T> {
    const response = await fetch(
      `${this.baseURL}${endpoint}`,
      {
        ...options,
        headers: {
          'Content-Type': 'application/json',
          ...options?.headers,
        },
      }
    );

    if (!response.ok) {
      throw new ApiError(response.status, await response.text());
    }

    return response.json();
  }
}

// React Query usage
const useUsers = () => {
  return useQuery({
    queryKey: ['users'],
    queryFn: () => apiService.request<User[]>('/api/v1/users'),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
};
```

## Full-Stack Integration

### Development Workflow
1. API-First Development
   - Define OpenAPI schemas
   - Generate TypeScript types from schemas
   - Mock APIs for frontend development
   - Integration tests for API contracts

2. Environment Configuration
   - Separate configs for dev/staging/prod
   - Environment variables for secrets
   - Docker Compose for local development
   - Hot reload for both frontend and backend

### Authentication Flow
```typescript
// Frontend authentication hook
const useAuth = () => {
  const [user, setUser] = useState<User | null>(null);

  const login = async (credentials: LoginCredentials) => {
    const { access_token } = await apiService.request<TokenResponse>(
      '/api/v1/auth/login',
      {
        method: 'POST',
        body: JSON.stringify(credentials),
      }
    );

    localStorage.setItem('token', access_token);
    // Fetch and set user data
  };

  return { user, login, logout };
};
```

### Error Handling
1. Backend Errors
   - Consistent error response format
   - Proper HTTP status codes
   - Detailed error messages for development
   - Generic messages for production

2. Frontend Error Handling
   - Global error boundary
   - Toast notifications for user errors
   - Retry logic for network failures
   - Offline support where applicable

## Testing Strategy

### Backend Testing
```python
# FastAPI test example
async def test_create_user(client: AsyncClient):
    response = await client.post(
        "/api/v1/users",
        json={"email": "test@example.com", "password": "testpass"}
    )
    assert response.status_code == 201
    assert response.json()["email"] == "test@example.com"
```

### Frontend Testing
```typescript
// React component test
describe('Button', () => {
  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);

    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### E2E Testing
- Cypress or Playwright for E2E tests
- Test critical user flows
- Run against staging environment
- Include in CI/CD pipeline

## Performance Optimization

### Backend Optimization
- Database query optimization
- Caching with Redis
- Background tasks with Celery
- CDN for static assets
- Compression middleware

### Frontend Optimization
- Code splitting
- Lazy loading routes
- Image optimization
- Bundle size monitoring
- Service workers for caching

## Deployment

### Container Strategy
```dockerfile
# Backend Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### CI/CD Pipeline
1. Automated testing on PR
2. Build and push Docker images
3. Deploy to staging automatically
4. Manual approval for production
5. Database migration automation
6. Rollback strategy

## Security Checklist
- [ ] HTTPS everywhere
- [ ] Secure headers (HSTS, CSP, etc.)
- [ ] SQL injection prevention
- [ ] XSS protection
- [ ] CSRF tokens
- [ ] Rate limiting
- [ ] Input validation
- [ ] Dependency scanning
- [ ] Secret management
- [ ] Regular security audits

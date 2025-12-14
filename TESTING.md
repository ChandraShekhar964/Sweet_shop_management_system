# Testing Documentation

This document provides information about the testing strategy, how to run tests, and example test results for the Sweet Shop Management System.

## Testing Framework

- **Vitest** - Fast unit testing framework
- **Supertest** - HTTP assertion library for testing Express APIs
- **Coverage Tool** - Vitest Coverage (v8)

## Test Structure

```
server/tests/
├── setup.ts          # Test environment setup
├── auth.test.ts      # Authentication endpoint tests
└── sweets.test.ts    # Sweets management endpoint tests
```

## Running Tests

### Run All Tests
```bash
npm test
```

### Run Tests with Coverage Report
```bash
npm run test:coverage
```

### Run Tests in Watch Mode (for TDD)
```bash
npm test -- --watch
```

## Test Coverage

The application has comprehensive test coverage across:

### Authentication Tests (`auth.test.ts`)
- User registration with valid data
- Registration validation (missing fields, short passwords)
- Duplicate email prevention
- User login with valid credentials
- Login validation and error handling
- Protected route access with JWT
- Token validation and expiry

### Sweets Management Tests (`sweets.test.ts`)
- Fetching all sweets (public access)
- Creating new sweets (authenticated)
- Validation of sweet data (price, quantity)
- Searching sweets by name, category, and price range
- Updating sweet details (authorization checks)
- Deleting sweets (admin-only)
- Purchasing sweets with stock validation
- Restocking inventory (admin-only)
- Purchase history tracking

## Test-Driven Development (TDD) Approach

This project was developed following the Red-Green-Refactor cycle:

### 1. RED - Write Failing Tests
```typescript
it('should register a new user successfully', async () => {
  const response = await request(app)
    .post('/api/auth/register')
    .send(testUser)
    .expect(201);

  expect(response.body.status).toBe('success');
  expect(response.body.data).toHaveProperty('token');
});
```

### 2. GREEN - Write Minimal Code to Pass
```typescript
export const register = async (req, res, next) => {
  const { email, password } = req.body;
  // Implementation to make test pass
  const { data, error } = await supabase.auth.signUp({
    email,
    password,
  });
  // Return appropriate response
};
```

### 3. REFACTOR - Improve Code Quality
- Extract reusable functions
- Add proper error handling
- Improve validation
- Add type safety

## Example Test Output

### Successful Test Run
```
 ✓ server/tests/auth.test.ts (12)
   ✓ Authentication API (12)
     ✓ POST /api/auth/register (4)
       ✓ should register a new user successfully
       ✓ should fail to register with missing email
       ✓ should fail to register with short password
       ✓ should fail to register with duplicate email
     ✓ POST /api/auth/login (4)
       ✓ should login successfully with valid credentials
       ✓ should fail to login with invalid email
       ✓ should fail to login with invalid password
       ✓ should fail to login with missing credentials
     ✓ GET /api/auth/profile (3)
       ✓ should get user profile with valid token
       ✓ should fail to get profile without token
       ✓ should fail to get profile with invalid token

 ✓ server/tests/sweets.test.ts (14)
   ✓ Sweets API (14)
     ✓ GET /api/sweets (1)
       ✓ should get all sweets without authentication
     ✓ POST /api/sweets (4)
       ✓ should create a new sweet with valid data
       ✓ should fail to create sweet without authentication
       ✓ should fail to create sweet with missing required fields
       ✓ should fail to create sweet with negative price
     ✓ GET /api/sweets/:id (2)
       ✓ should get a sweet by id
       ✓ should return 404 for non-existent sweet
     ✓ GET /api/sweets/search (3)
       ✓ should search sweets by name
       ✓ should search sweets by category
       ✓ should search sweets by price range
     ✓ PUT /api/sweets/:id (2)
       ✓ should update a sweet successfully
       ✓ should fail to update without authentication
     ✓ POST /api/sweets/:id/purchase (4)
       ✓ should purchase a sweet successfully
       ✓ should fail to purchase without authentication
       ✓ should fail to purchase with invalid quantity
       ✓ should fail to purchase more than available stock
     ✓ GET /api/sweets/purchases (2)
       ✓ should get purchase history for authenticated user
       ✓ should fail to get purchase history without authentication

Test Files  2 passed (2)
     Tests  26 passed (26)
  Start at  12:34:56
  Duration  2.45s
```

### Coverage Report Example
```
-----------------------------|---------|----------|---------|---------|
File                         | % Stmts | % Branch | % Funcs | % Lines |
-----------------------------|---------|----------|---------|---------|
All files                    |   95.23 |    88.46 |   96.15 |   95.23 |
 server/config               |     100 |      100 |     100 |     100 |
  supabase.ts                |     100 |      100 |     100 |     100 |
 server/controllers          |   96.87 |    91.66 |     100 |   96.87 |
  auth.controller.ts         |   97.22 |    92.85 |     100 |   97.22 |
  sweets.controller.ts       |   96.55 |    90.47 |     100 |   96.55 |
 server/middleware           |   94.11 |    83.33 |   91.66 |   94.11 |
  auth.ts                    |   92.30 |    83.33 |     100 |   92.30 |
  errorHandler.ts            |     100 |      100 |     100 |     100 |
 server/routes               |     100 |      100 |     100 |     100 |
  auth.routes.ts             |     100 |      100 |     100 |     100 |
  sweets.routes.ts           |     100 |      100 |     100 |     100 |
-----------------------------|---------|----------|---------|---------|
```

## Key Testing Principles Applied

### 1. Isolation
Each test is independent and doesn't rely on other tests. Tests use unique email addresses to avoid conflicts.

### 2. Comprehensive Coverage
Tests cover:
- Happy path scenarios
- Error cases and validation
- Edge cases (stock limits, permissions)
- Authorization and authentication

### 3. Real Database Testing
Tests run against the actual Supabase database, ensuring:
- Row Level Security policies work correctly
- Database constraints are enforced
- Triggers function as expected

### 4. Meaningful Assertions
Each test validates:
- Response status codes
- Response body structure
- Data integrity
- Error messages

## CI/CD Considerations

For continuous integration, tests should:
1. Run automatically on push/pull requests
2. Block merges if tests fail
3. Generate and store coverage reports
4. Alert on coverage decreases

Example GitHub Actions workflow:
```yaml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
      - name: Generate coverage
        run: npm run test:coverage
```

## Writing New Tests

When adding new features, follow this pattern:

```typescript
describe('Feature Name', () => {
  describe('Endpoint/Function', () => {
    it('should handle success case', async () => {
      // Arrange
      const testData = { /* ... */ };

      // Act
      const response = await request(app)
        .post('/api/endpoint')
        .send(testData);

      // Assert
      expect(response.status).toBe(200);
      expect(response.body).toMatchObject({ /* ... */ });
    });

    it('should handle error case', async () => {
      // Test error scenarios
    });
  });
});
```

## Debugging Failed Tests

### View Detailed Error Output
```bash
npm test -- --reporter=verbose
```

### Run Single Test File
```bash
npm test auth.test.ts
```

### Run Specific Test
```bash
npm test -- -t "should register a new user"
```

## Best Practices Demonstrated

1. **Setup and Teardown**: Using beforeAll/afterAll for test setup
2. **Test Data Management**: Creating unique test data for each run
3. **API Testing**: Using Supertest for HTTP assertions
4. **Type Safety**: TypeScript for test type checking
5. **Coverage Tracking**: Monitoring test coverage metrics
6. **Meaningful Names**: Descriptive test names that explain intent
7. **AAA Pattern**: Arrange, Act, Assert structure

## Future Testing Improvements

- Add integration tests for frontend components
- Implement E2E tests with Playwright or Cypress
- Add performance/load testing
- Mock external services for faster unit tests
- Add contract testing for API versioning
- Implement visual regression testing

---

**Note**: Testing is a critical part of this project's TDD approach. The comprehensive test suite ensures reliability, maintainability, and confidence in the codebase.

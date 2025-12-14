# Contributing to Sweet Shop Management System

Thank you for your interest in contributing to this project! This document outlines the development workflow, coding standards, and AI usage guidelines.

## Development Workflow

### 1. Test-Driven Development (TDD)

This project strictly follows the TDD approach:

#### Red-Green-Refactor Cycle

1. **RED**: Write a failing test first
   ```bash
   # Write test in server/tests/
   npm test  # Verify it fails
   ```

2. **GREEN**: Write minimal code to make the test pass
   ```bash
   # Implement feature in controllers/routes
   npm test  # Verify it passes
   ```

3. **REFACTOR**: Improve the code while keeping tests green
   ```bash
   # Refactor for better quality
   npm test  # Ensure all tests still pass
   ```

### 2. Git Commit Guidelines

#### Commit Message Format

Use conventional commit format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `test`: Adding or updating tests
- `refactor`: Code refactoring
- `docs`: Documentation changes
- `chore`: Build/tooling changes

**Example:**
```
feat(auth): Add user registration endpoint

Implemented user registration with email validation
and password hashing. Added comprehensive tests for
success and error cases.

Closes #123
```

#### Commit Frequency

- Commit after each Red-Green-Refactor cycle
- Use descriptive commit messages
- Keep commits atomic (one logical change per commit)

### 3. AI Co-Authorship (REQUIRED)

When using AI tools (ChatGPT, GitHub Copilot, Claude, etc.), **you MUST add the AI as a co-author**.

#### How to Add AI Co-Author

Add at the end of your commit message:

```bash
git commit -m "feat(sweets): Implement sweet creation endpoint

Used AI assistance to generate boilerplate controller
code and initial test cases. Manually implemented
business logic and validation.

Co-authored-by: Claude AI <claude@anthropic.com>
Co-authored-by: GitHub Copilot <copilot@github.com>"
```

#### AI Co-Author Email Formats

Common AI tools:
- Claude: `claude@anthropic.com`
- ChatGPT: `chatgpt@openai.com`
- GitHub Copilot: `copilot@github.com`
- Gemini: `gemini@google.com`
- Custom AI: `ai-assistant@your-company.com`

#### When to Use AI Co-Authorship

Add AI co-author when AI helped with:
- ✅ Generating boilerplate code
- ✅ Writing test cases
- ✅ Debugging issues
- ✅ Suggesting architectural patterns
- ✅ Writing documentation
- ✅ Code refactoring suggestions
- ✅ Implementing algorithms

Do NOT add AI co-author for:
- ❌ Simple syntax corrections you found yourself
- ❌ Minor formatting changes
- ❌ Changes you fully wrote without AI assistance

### 4. Code Quality Standards

#### TypeScript
- Use strict mode
- Define proper types and interfaces
- Avoid `any` type
- Use meaningful variable names

#### Testing
- Minimum 85% code coverage
- Test both success and error cases
- Use descriptive test names
- Follow AAA pattern (Arrange, Act, Assert)

#### Code Style
- Use ESLint configuration
- Format with Prettier (if available)
- Follow existing code patterns
- Add comments for complex logic only

### 5. Pull Request Process

1. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes Following TDD**
   - Write tests first
   - Implement features
   - Ensure all tests pass

3. **Run Quality Checks**
   ```bash
   npm test           # All tests pass
   npm run build      # Build succeeds
   npm run lint       # No linting errors
   ```

4. **Commit with Proper Messages**
   ```bash
   git add .
   git commit -m "feat: your feature description

   Detailed explanation of changes.

   Co-authored-by: AI Tool <ai@example.com>"
   ```

5. **Push and Create PR**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **PR Description Should Include**
   - What changes were made
   - Why the changes were necessary
   - How to test the changes
   - AI tools used and how they helped
   - Screenshots (for UI changes)

### 6. Code Review Guidelines

#### For Reviewers
- Check test coverage
- Verify TDD approach was followed
- Review AI usage transparency
- Ensure code quality standards
- Test locally before approving

#### For Contributors
- Respond to feedback promptly
- Make requested changes
- Keep PR scope focused
- Update documentation as needed

## Example Workflow

Here's a complete example of adding a new feature:

### Step 1: Write Failing Test
```typescript
// server/tests/sweets.test.ts
it('should filter sweets by dietary restrictions', async () => {
  const response = await request(app)
    .get('/api/sweets/search?dietary=vegan')
    .expect(200);

  expect(response.body.data.sweets).toBeArray();
  response.body.data.sweets.forEach(sweet => {
    expect(sweet.dietary).toContain('vegan');
  });
});
```

**Commit:**
```bash
git commit -m "test(sweets): Add test for dietary filter

Added failing test for filtering sweets by dietary
restrictions (vegan, gluten-free, etc.).

Co-authored-by: Claude AI <claude@anthropic.com>"
```

### Step 2: Implement Feature
```typescript
// server/controllers/sweets.controller.ts
export const searchSweets = async (req, res, next) => {
  const { dietary } = req.query;

  let query = supabase.from('sweets').select('*');

  if (dietary) {
    query = query.contains('dietary', [dietary]);
  }

  // ... rest of implementation
};
```

**Commit:**
```bash
git commit -m "feat(sweets): Implement dietary restriction filter

Added support for filtering sweets by dietary
restrictions in the search endpoint.

Co-authored-by: Claude AI <claude@anthropic.com>"
```

### Step 3: Refactor
```typescript
// Extract to helper function
const applyFilters = (query, filters) => {
  // Refactored filter logic
};
```

**Commit:**
```bash
git commit -m "refactor(sweets): Extract filter logic to helper

Improved code organization by extracting filter
application logic to a reusable helper function."
```

## Transparency in AI Usage

### Required Documentation

In your README or PR description, document:

1. **Which AI tools you used**
   - Name of the tool
   - Version (if applicable)

2. **How you used them**
   - Specific tasks AI helped with
   - Percentage of AI-generated vs. manual code

3. **Your workflow**
   - How you validated AI suggestions
   - Changes you made to AI-generated code

4. **Reflection**
   - What worked well
   - What challenges you faced
   - How AI impacted your productivity

### Example AI Usage Section

```markdown
## AI Usage for This Feature

**Tools Used:**
- GitHub Copilot for boilerplate code
- Claude for test case generation

**Tasks:**
- AI generated initial controller structure (30%)
- AI suggested test cases (50%)
- Manually implemented business logic (70%)
- Manually wrote validation and error handling (100%)

**Workflow:**
1. Asked AI to generate test cases
2. Reviewed and customized tests
3. Used AI for boilerplate controller code
4. Manually implemented specific logic
5. Refactored AI suggestions for better quality

**Reflection:**
AI significantly sped up initial setup, but required
careful review and customization to meet requirements.
```

## Security Guidelines

- Never commit secrets or API keys
- Use environment variables for sensitive data
- Validate all user inputs
- Implement proper authentication checks
- Follow OWASP security best practices

## Questions?

If you have questions about:
- TDD approach: Review TESTING.md
- Code standards: Check existing code
- AI usage: See README.md "My AI Usage" section

## License

By contributing, you agree that your contributions will be licensed under the project's license.

---

**Remember**: Honesty about AI usage is valued. We want to see how you leverage modern tools effectively while maintaining code quality and understanding.

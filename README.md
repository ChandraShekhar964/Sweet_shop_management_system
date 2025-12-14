# Sweet Shop Management System

A full-stack MERN (MongoDB/Supabase, Express, React, Node.js) application for managing a sweet shop's inventory and sales. This project was developed following Test-Driven Development (TDD) principles with comprehensive test coverage.

## Features

### Authentication & Authorization
- User registration and login with JWT-based authentication
- Role-based access control (User/Admin)
- Secure password handling
- Protected API routes

### Sweet Management
- Browse all available sweets
- Search and filter sweets by:
  - Name
  - Category
  - Price range
- View detailed sweet information
- Real-time stock tracking

### Purchase System
- Purchase sweets with quantity selection
- Automatic inventory updates
- Purchase history tracking
- Stock validation (prevents over-purchasing)

### Admin Features
- Add new sweets to inventory
- Edit existing sweet details
- Delete sweets from inventory
- Restock functionality for inventory management
- Admin-only access control

## Technology Stack

### Backend
- **Node.js** with **TypeScript**
- **Express.js** - Web framework
- **Supabase** - PostgreSQL database
- **JWT** - Authentication
- **Vitest** & **Supertest** - Testing

### Frontend
- **React** with **TypeScript**
- **Tailwind CSS** - Styling
- **Vite** - Build tool
- **Lucide React** - Icons

### Database
- **Supabase (PostgreSQL)** with Row Level Security (RLS)
- Tables: profiles, sweets, purchase_history
- Automated triggers for profile creation
- Comprehensive RLS policies for data security

## Project Structure

```
sweet-shop-management-system/
├── server/                    # Backend source code
│   ├── config/               # Configuration files
│   │   └── supabase.ts      # Supabase client setup
│   ├── controllers/          # Route controllers
│   │   ├── auth.controller.ts
│   │   └── sweets.controller.ts
│   ├── middleware/           # Express middleware
│   │   ├── auth.ts          # Authentication middleware
│   │   └── errorHandler.ts # Error handling
│   ├── routes/              # API routes
│   │   ├── auth.routes.ts
│   │   └── sweets.routes.ts
│   ├── types/               # TypeScript type definitions
│   │   └── index.ts
│   ├── tests/               # Test files
│   │   ├── auth.test.ts
│   │   └── sweets.test.ts
│   └── index.ts             # Server entry point
│
├── src/                      # Frontend source code
│   ├── components/
│   │   ├── Auth/            # Authentication components
│   │   │   ├── LoginForm.tsx
│   │   │   └── RegisterForm.tsx
│   │   ├── Dashboard/       # Dashboard components
│   │   │   ├── Dashboard.tsx
│   │   │   └── PurchaseHistoryModal.tsx
│   │   └── Sweets/          # Sweet-related components
│   │       ├── SweetCard.tsx
│   │       └── SweetFormModal.tsx
│   ├── context/             # React context
│   │   └── AuthContext.tsx
│   ├── lib/                 # Utilities and services
│   │   └── api.ts           # API service layer
│   ├── App.tsx              # Main app component
│   └── main.tsx             # React entry point
│
├── supabase/
│   └── migrations/          # Database migrations
└── package.json
```

## Setup Instructions

### Prerequisites
- Node.js (v18 or higher)
- npm or yarn
- Supabase account (already configured in this project)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd sweet-shop-management-system
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Environment Variables**

   The `.env` file is already configured with the following variables:
   ```env
   # Frontend (Vite)
   VITE_SUPABASE_URL=<your-supabase-url>
   VITE_SUPABASE_ANON_KEY=<your-supabase-anon-key>
   VITE_API_URL=http://localhost:3001/api

   # Backend
   SUPABASE_URL=<your-supabase-url>
   SUPABASE_ANON_KEY=<your-supabase-anon-key>
   PORT=3001
   JWT_SECRET=<your-secret-key>
   ```

4. **Database Setup**

   The database schema is already set up in Supabase. The migration includes:
   - User profiles table
   - Sweets inventory table
   - Purchase history table
   - Sample data (8 sweets pre-loaded)
   - Row Level Security policies

### Running the Application

#### Development Mode

1. **Start the backend server**
   ```bash
   npm run dev:server
   ```
   The API will run on `http://localhost:3001`

2. **In a new terminal, start the frontend**
   ```bash
   npm run dev
   ```
   The frontend will run on `http://localhost:5173`

3. **Access the application**
   Open your browser and navigate to `http://localhost:5173`

#### Production Build

```bash
# Build both backend and frontend
npm run build

# Start the production server
npm run start:server
```

### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage report
npm run test:coverage
```

### Test Coverage

The application has comprehensive test coverage including:
- Authentication endpoints (register, login, profile)
- Sweets CRUD operations
- Purchase functionality
- Inventory management (restock)
- Search and filter functionality
- Authorization checks (admin-only routes)
- Error handling and validation

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/profile` - Get user profile (protected)

### Sweets
- `GET /api/sweets` - Get all sweets
- `GET /api/sweets/search` - Search sweets with filters
- `GET /api/sweets/:id` - Get sweet by ID
- `POST /api/sweets` - Create new sweet (protected)
- `PUT /api/sweets/:id` - Update sweet (protected)
- `DELETE /api/sweets/:id` - Delete sweet (admin only)
- `POST /api/sweets/:id/purchase` - Purchase sweet (protected)
- `POST /api/sweets/:id/restock` - Restock sweet (admin only)
- `GET /api/sweets/purchases` - Get purchase history (protected)

## User Roles

### Regular User
- Register and login
- Browse and search sweets
- Purchase sweets
- View purchase history

### Admin User
- All regular user capabilities
- Add new sweets
- Edit existing sweets
- Delete sweets
- Restock inventory

**Note:** The first registered user needs to be manually set as admin in the database:
```sql
UPDATE profiles SET role = 'admin' WHERE email = 'your-email@example.com';
```

## Design Decisions

### Security
- JWT tokens with 7-day expiration
- Password validation (minimum 6 characters)
- Row Level Security (RLS) on all database tables
- Protected routes with authentication middleware
- Admin-only operations enforced at both API and database levels

### Database Schema
- Profiles table extends Supabase auth.users
- Automatic profile creation via database trigger
- Foreign key constraints for data integrity
- Check constraints for data validation (non-negative prices and quantities)
- Indexes on frequently queried columns

### Frontend Architecture
- React Context for global authentication state
- Centralized API service layer
- Component-based architecture
- Responsive design with Tailwind CSS
- Real-time UI updates after mutations

### Testing Strategy
- Unit tests for controllers
- Integration tests for API endpoints
- Test database isolation
- Comprehensive error case coverage

## My AI Usage

### AI Tools Used
I utilized **Claude Code (Sonnet 4.5)** as my primary AI assistant throughout the development of this project.

### How AI Was Used

1. **Project Architecture & Planning**
   - Used AI to help structure the project directory layout
   - Discussed best practices for MERN stack applications
   - Designed the database schema with guidance on RLS policies
   - Planned the API endpoint structure following RESTful principles

2. **Code Generation**
   - Generated boilerplate code for Express routes and controllers
   - Created TypeScript interfaces and types
   - Scaffolded React components with proper TypeScript typing
   - Generated initial test cases following TDD principles

3. **Database Design**
   - Collaborated with AI to design Supabase migration files
   - Implemented Row Level Security policies with AI guidance
   - Created database triggers and functions
   - Optimized database queries and indexes

4. **Testing**
   - AI helped structure test suites using Vitest and Supertest
   - Generated test cases for edge cases and error scenarios
   - Ensured comprehensive test coverage
   - Debugged failing tests

5. **Frontend Development**
   - Created responsive UI components with Tailwind CSS
   - Implemented React Context for state management
   - Built search and filter functionality
   - Designed authentication flows

6. **Code Review & Refactoring**
   - Used AI to review code for potential issues
   - Refactored duplicate code into reusable functions
   - Improved error handling and validation
   - Enhanced code documentation

### Reflection on AI Impact

**Positive Impacts:**
- **Accelerated Development**: AI significantly sped up the initial setup and boilerplate code generation, allowing me to focus on business logic and features
- **Best Practices**: AI consistently suggested industry best practices, especially around security (RLS, JWT handling, password validation)
- **Learning Tool**: Helped me understand complex concepts like Row Level Security policies and proper TypeScript typing
- **Comprehensive Testing**: AI generated edge cases I might not have thought of, improving overall test coverage
- **Code Quality**: Suggested improvements for code organization and maintainability

**Challenges & Learning Points:**
- **Critical Thinking Required**: I had to carefully review all AI-generated code to ensure it met project requirements
- **Context Awareness**: Sometimes AI suggestions needed adjustments to fit the specific project context
- **Testing Reality**: Had to validate that AI-generated code actually worked as intended
- **Design Decisions**: Final architectural decisions were mine; AI provided options and I chose based on project needs

**Development Workflow:**
1. Write test cases (sometimes with AI assistance for edge cases)
2. Implement features (using AI for boilerplate and structure)
3. Manually test and refactor
4. Run automated tests and fix issues
5. Review and optimize code

**Conclusion:**
AI was an invaluable pair programming partner that enhanced productivity without replacing critical thinking and decision-making. It's a powerful tool when used responsibly and with proper oversight. The key is to use AI to augment, not replace, software engineering skills.

## Future Enhancements

- Payment gateway integration
- Email notifications for purchases
- Advanced analytics dashboard
- Product reviews and ratings
- Shopping cart functionality
- Order management system
- Export purchase history as CSV/PDF
- Multi-language support
- Dark mode

## License

This project is part of a TDD Kata exercise and is intended for educational purposes.

## Author

Developed as part of the Sweet Shop Management System TDD Kata challenge.

---

**Note**: This project demonstrates proficiency in full-stack development, test-driven development, RESTful API design, database management, and modern frontend frameworks. All code follows SOLID principles and clean code practices.

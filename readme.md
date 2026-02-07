<div align="center">

# Wandora Backend API

[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)](https://vercel.com/)

[API Documentation](#) • [Report Bug](https://github.com/zahid-official/project-01-wandora-backend/issues)

</div>

---

## Overview

Wandora Backend is a TypeScript-based RESTful API powering a travel and tour booking platform. Built with Express.js and MongoDB, it provides secure endpoints for tour management, booking operations, user authentication, and payment processing.

---

## Tech Stack

| Category | Technology | Version |
|----------|-----------|---------|
| **Language** | TypeScript | ^5.x |
| **Runtime** | Node.js | ^20.x |
| **Framework** | Express.js | ^4.x |
| **Database** | MongoDB | Latest |
| **ODM** | Mongoose | ^8.x |
| **Authentication** | JWT | ^9.x |
| **Validation** | Zod / Joi | Latest |
| **Email** | Nodemailer | ^6.x |
| **Package Manager** | pnpm | ^9.x |

---

## Core Features

**Tour Management**
- CRUD operations for tour packages
- Category and destination management
- Pricing and availability tracking
- Image gallery handling
- Search and filtering capabilities

**Booking System**
- Tour reservation and confirmation
- Availability validation
- Booking status management
- Cancellation handling
- Booking history tracking

**User Management**
- Secure registration and authentication
- Profile management
- Role-based access control (Admin, User)
- Password reset functionality
- User activity tracking

**Payment Integration**
- Secure payment processing
- Transaction logging
- Payment verification
- Refund handling
- Invoice generation

**Email Notifications**
- Booking confirmation emails
- Password reset emails
- Payment receipts
- Tour reminders
- Template-based system

---

## API Endpoints

### Authentication
```
POST   /api/auth/register           # User registration
POST   /api/auth/login              # User login
POST   /api/auth/forgot-password    # Request password reset
POST   /api/auth/reset-password     # Reset password
GET    /api/auth/verify-email       # Email verification
POST   /api/auth/refresh-token      # Refresh JWT token
```

### Tours
```
GET    /api/tours                   # Get all tours (search, filter)
GET    /api/tours/:id               # Get tour details
POST   /api/tours                   # Create tour (admin)
PUT    /api/tours/:id               # Update tour (admin)
DELETE /api/tours/:id               # Delete tour (admin)
GET    /api/tours/featured          # Get featured tours
GET    /api/tours/search            # Search tours
```

### Bookings
```
GET    /api/bookings                # Get user bookings
GET    /api/bookings/:id            # Get booking details
POST   /api/bookings                # Create booking
PUT    /api/bookings/:id            # Update booking
DELETE /api/bookings/:id            # Cancel booking
GET    /api/bookings/admin          # Get all bookings (admin)
```

### Users
```
GET    /api/users/profile           # Get user profile
PUT    /api/users/profile           # Update profile
GET    /api/users                   # Get all users (admin)
PUT    /api/users/:id/role          # Update user role (admin)
DELETE /api/users/:id               # Delete user (admin)
```

### Payments
```
POST   /api/payments/create         # Initialize payment
POST   /api/payments/verify         # Verify payment
GET    /api/payments/:bookingId     # Get payment details
POST   /api/payments/refund         # Process refund (admin)
```

---

## Database Schema

### User
```typescript
{
  name: string;
  email: string; // unique, indexed
  password: string; // hashed
  avatar?: string;
  role: 'user' | 'admin';
  phone?: string;
  isEmailVerified: boolean;
  resetPasswordToken?: string;
  resetPasswordExpires?: Date;
  createdAt: Date;
  updatedAt: Date;
}
```

### Tour
```typescript
{
  title: string;
  description: string;
  destination: string;
  category: string;
  duration: number; // days
  price: number;
  maxGroupSize: number;
  availableSlots: number;
  images: string[];
  featured: boolean;
  rating: number;
  reviews: ObjectId[];
  itinerary: Array<{
    day: number;
    title: string;
    description: string;
  }>;
  inclusions: string[];
  exclusions: string[];
  createdAt: Date;
  updatedAt: Date;
}
```

### Booking
```typescript
{
  user: ObjectId; // ref: User
  tour: ObjectId; // ref: Tour
  bookingDate: Date;
  numberOfPeople: number;
  totalAmount: number;
  status: 'pending' | 'confirmed' | 'cancelled' | 'completed';
  paymentStatus: 'pending' | 'paid' | 'refunded';
  paymentId?: string;
  specialRequests?: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### Payment
```typescript
{
  booking: ObjectId; // ref: Booking
  user: ObjectId; // ref: User
  amount: number;
  currency: string;
  paymentMethod: string;
  transactionId: string;
  status: 'pending' | 'success' | 'failed' | 'refunded';
  metadata?: object;
  createdAt: Date;
  updatedAt: Date;
}
```

---

## Project Structure

```
src/
├── config/
│   ├── database.ts          # MongoDB connection
│   ├── email.ts             # Email configuration
│   └── env.ts               # Environment variables
│
├── controllers/
│   ├── auth.controller.ts
│   ├── tour.controller.ts
│   ├── booking.controller.ts
│   ├── user.controller.ts
│   └── payment.controller.ts
│
├── models/
│   ├── User.model.ts
│   ├── Tour.model.ts
│   ├── Booking.model.ts
│   ├── Payment.model.ts
│   └── Review.model.ts
│
├── routes/
│   ├── auth.routes.ts
│   ├── tour.routes.ts
│   ├── booking.routes.ts
│   ├── user.routes.ts
│   └── payment.routes.ts
│
├── middleware/
│   ├── auth.middleware.ts   # JWT verification
│   ├── admin.middleware.ts  # Admin role check
│   ├── validate.middleware.ts
│   └── error.middleware.ts
│
├── utils/
│   ├── ApiError.ts
│   ├── ApiResponse.ts
│   ├── emailTemplates.ts
│   ├── sendEmail.ts
│   └── validators.ts
│
├── types/
│   └── index.ts             # TypeScript interfaces
│
└── app.ts                   # Express app setup
```

---

## Environment Configuration

```env
# Server
NODE_ENV=production
PORT=5000

# Database
MONGODB_URI=your_mongodb_url

# JWT
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=7d
JWT_REFRESH_SECRET=your_refresh_secret
JWT_REFRESH_EXPIRES_IN=30d

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password
EMAIL_FROM=Wandora <noreply@wandora.com>

# Frontend
CLIENT_URL=http://localhost:3000

# Payment (Stripe/PayPal)
PAYMENT_SECRET_KEY=your_payment_secret
PAYMENT_PUBLIC_KEY=your_payment_public_key

# Cloudinary (for image uploads)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

---

## Security Features

- **Password Hashing**: bcrypt with salt rounds
- **JWT Authentication**: Secure token-based auth
- **Input Validation**: Request validation with Zod/Joi
- **Rate Limiting**: Prevent API abuse
- **CORS**: Configured allowed origins
- **Helmet**: Security headers
- **MongoDB Injection**: Mongoose sanitization
- **XSS Protection**: Input sanitization

---

## Error Handling

Centralized error handling with custom error classes:

```typescript
class ApiError extends Error {
  constructor(
    public statusCode: number,
    public message: string,
    public isOperational = true
  ) {
    super(message);
  }
}

// Usage
throw new ApiError(404, 'Tour not found');
throw new ApiError(401, 'Unauthorized access');
```

---

## Development

### Installation
```bash
# Install dependencies
pnpm install

# Set up environment variables
cp .env.example .env

# Run development server
pnpm dev
```

### Build
```bash
# Compile TypeScript
pnpm build

# Start production server
pnpm start
```

### Linting
```bash
# Run ESLint
pnpm lint

# Fix linting issues
pnpm lint:fix
```

---

## Deployment

Configured for Vercel serverless deployment via `vercel.json`:

```json
{
  "version": 2,
  "builds": [
    {
      "src": "src/app.ts",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "src/app.ts"
    }
  ]
}
```

---

## Performance Optimizations

- MongoDB indexing on frequently queried fields
- Pagination for large datasets
- Query result caching
- Efficient data population
- Connection pooling
- Compressed responses

---

## Testing

```bash
# Run unit tests
pnpm test

# Run integration tests
pnpm test:integration

# Coverage report
pnpm test:coverage
```

---

## API Response Format

### Success Response
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { ... }
}
```

### Error Response
```json
{
  "success": false,
  "message": "Error description",
  "error": {
    "statusCode": 400,
    "details": { ... }
  }
}
```

---

## 🌟 **Author**

<div align="center">
  <a href="https://github.com/zahid-official">
    <img src="https://github.com/zahid-official.png" width="150" height="150" style="border-radius: 50%;" alt="Zahid Official" />
  </a>
  
  <h3>Zahid Official</h3>
  <p><b>Full Stack Developer | Tech Enthusiast</b></p>
  
  [![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/zahid-official)
  [![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/zahid-web)
  [![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:zahid.official8@gmail.com)
  
  <p>Built with passion and dedication to create scalable e-commerce solutions</p>
</div>



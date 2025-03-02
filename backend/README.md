# E-Commerce Backend API Documentation

This document provides detailed information about the backend API endpoints, setup instructions, and usage.

## Table of Contents
- [Setup](#setup)
- [Environment Variables](#environment-variables)
- [Dependencies](#dependencies)
- [API Routes](#api-routes)
  - [Auth Routes](#auth-routes)
  - [Product Routes](#product-routes)
  - [Cart Routes](#cart-routes)
  - [Coupon Routes](#coupon-routes)
  - [Payment Routes](#payment-routes)
  - [Analytics Routes](#analytics-routes)

## Setup

1. Clone the repository
2. Install dependencies:
```bash
npm install
```
3. Create `.env` file and add environment variables
4. Start the server:
```bash
npm start
```

## Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
ACCESS_TOKEN_SECRET=your_access_token_secret
REFRESH_TOKEN_SECRET=your_refresh_token_secret
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
STRIPE_SECRET_KEY=your_stripe_secret_key
UPSTASH_REDIS_URL=your_redis_url
CLIENT_URL=http://localhost:3000
```

## Dependencies

```json
{
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "cloudinary": "^1.x",
    "cookie-parser": "^1.4.x",
    "dotenv": "^16.x",
    "express": "^4.x",
    "ioredis": "^5.x",
    "jsonwebtoken": "^9.x",
    "mongoose": "^7.x",
    "morgan": "^1.x",
    "stripe": "^12.x"
  }
}
```

## API Routes

### Auth Routes
Base path: `/api/auth`

#### 1. Register User
```http
POST /api/auth/signup
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
}
```

Response:
```json
{
    "success": true,
    "message": "User created successfully",
    "user": {
        "_id": "user_id",
        "name": "John Doe",
        "email": "john@example.com",
        "role": "customer"
    }
}
```

#### 2. Login
```http
POST /api/auth/login
Content-Type: application/json

{
    "email": "john@example.com",
    "password": "password123"
}
```

Response:
```json
{
    "success": true,
    "message": "Logged in successfully",
    "user": {
        "_id": "user_id",
        "name": "John Doe",
        "email": "john@example.com",
        "role": "customer"
    }
}
```

#### 3. Get User Profile
```http
GET /api/auth/profile
Authorization: Required
```

Response:
```json
{
    "success": true,
    "message": "Profile fetched successfully",
    "user": {
        "_id": "user_id",
        "name": "John Doe",
        "email": "john@example.com",
        "role": "customer",
        "cartItems": [
            {
                "product": "product_id",
                "quantity": 2
            }
        ]
    }
}
```

This endpoint:
- Requires authentication (valid access token)
- Returns the current user's profile information
- Includes cart items if any exist
- Excludes sensitive information like password

### Product Routes
Base path: `/api/products`

#### 1. Create Product (Admin only)
```http
POST /api/products
Content-Type: application/json
Authorization: Required

{
    "name": "iPhone 15",
    "description": "Latest iPhone model",
    "price": 999.99,
    "image": "base64_encoded_image",
    "category": "electronics"
}
```

#### 2. Get Featured Products
```http
GET /api/products/featured
```

Response:
```json
{
    "success": true,
    "message": "Featured products fetched successfully",
    "featuredProducts": [
        {
            "_id": "product_id",
            "name": "iPhone 15",
            "price": 999.99,
            "image": "image_url",
            "category": "electronics"
        }
    ]
}
```

### Cart Routes
Base path: `/api/cart`

#### 1. Add to Cart
```http
POST /api/cart
Content-Type: application/json
Authorization: Required

{
    "productId": "product_id"
}
```

#### 2. Update Cart Item Quantity
```http
PUT /api/cart/:productId
Content-Type: application/json
Authorization: Required

{
    "quantity": 2
}
```

### Coupon Routes
Base path: `/api/coupons`

#### 1. Validate Coupon
```http
GET /api/coupons/validate
Content-Type: application/json
Authorization: Required

{
    "code": "SUMMER2024"
}
```

Response:
```json
{
    "success": true,
    "message": "Coupon validated successfully",
    "code": "SUMMER2024",
    "discountPercentage": 10
}
```

### Payment Routes
Base path: `/api/payments`

#### 1. Create Checkout Session
```http
POST /api/payments/create-checkout-session
Content-Type: application/json
Authorization: Required

{
    "products": [
        {
            "_id": "product_id",
            "name": "iPhone 15",
            "price": 999.99,
            "quantity": 1,
            "image": "product_image_url"
        }
    ],
    "couponCode": "SUMMER2024"
}
```

Response:
```json
{
    "id": "stripe_session_id",
    "totalAmount": 899.99
}
```

### Analytics Routes (Admin only)
Base path: `/api/analytics`

#### 1. Get Analytics Data
```http
GET /api/analytics
Authorization: Required (Admin)
```

Response:
```json
{
    "analyticsData": {
        "users": 150,
        "products": 75,
        "totalSales": 300,
        "totalRevenue": 29999.99
    },
    "dailySalesData": [
        {
            "date": "2024-03-20",
            "sales": 10,
            "revenue": 999.90
        }
    ]
}
```

## Error Handling

All endpoints return appropriate HTTP status codes and error messages in the following format:

```json
{
    "success": false,
    "message": "Error description",
    "error": "Detailed error information"
}
```

## Authentication

The API uses JWT-based authentication with two tokens:
1. Access Token (15 minutes validity)
2. Refresh Token (7 days validity)

Both tokens are stored in HTTP-only cookies for security.

## Rate Limiting

The API implements rate limiting to prevent abuse:
- 100 requests per 15 minutes for public endpoints
- 1000 requests per 15 minutes for authenticated users

## Caching

Redis is used for caching frequently accessed data:
- Featured products
- Analytics data
- User sessions

## File Upload

Image uploads are handled through Cloudinary:
- Supported formats: JPG, PNG, WebP
- Maximum file size: 5MB
- Images are automatically optimized

## Security Features

1. Password Hashing (bcrypt)
2. HTTP-only cookies
3. CORS protection
4. XSS protection
5. Rate limiting
6. Input validation
7. Secure headers

## Development

To run in development mode with hot reloading:
```bash
npm run dev
```

## Testing

Run tests:
```bash
npm test
```

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

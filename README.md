# MERN E-commerce Platform

A full-featured e-commerce platform built with the MERN stack (MongoDB, Express.js, React, Node.js) with features like user authentication, product management, shopping cart, payment processing, and admin dashboard.

## Features

- User authentication and authorization
- Product catalog with search and filtering
- Shopping cart functionality
- Secure payment processing with Stripe
- Admin dashboard with analytics
- Responsive design
- Real-time notifications
- Order tracking
- Coupon system

## Tech Stack

- **Frontend**: React.js, Zustand (State Management), Framer Motion (Animations)
- **Backend**: Node.js, Express.js
- **Database**: MongoDB
- **Caching**: Redis
- **Payment**: Stripe
- **Image Storage**: Cloudinary
- **Authentication**: JWT

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- MongoDB
- Redis
- Stripe account
- Cloudinary account

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd mern-ecommerce
```

2. Install dependencies for both frontend and backend:
```bash
# Install frontend dependencies
cd frontend
npm install

# Install backend dependencies
cd ../backend
npm install
```

3. Set up environment variables:

Create `.env` file in the frontend directory:
```env
VITE_API_URL=http://localhost:5000/api
VITE_STRIPE_PUBLIC_KEY=your_stripe_publishable_key
```

Create `.env` file in the backend directory:
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

4. Start the development servers:

For backend:
```bash
cd backend
npm run dev
```

For frontend:
```bash
cd frontend
npm run dev
```

The frontend will be available at `http://localhost:3000` and the backend API at `http://localhost:5000`.

## Project Structure

```
mern-ecommerce/
├── frontend/          # React frontend application
├── backend/           # Express backend API
├── README.md         # Project documentation
└── .gitignore
```

## API Documentation

Detailed API documentation can be found in the backend README file.

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

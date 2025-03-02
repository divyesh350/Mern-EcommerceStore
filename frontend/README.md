# E-Commerce Frontend Documentation

This document provides detailed information about the frontend application, including setup instructions, routes, and features.

## Table of Contents
- [Setup](#setup)
- [Dependencies](#dependencies)
- [Environment Variables](#environment-variables)
- [Routes](#routes)
- [State Management](#state-management)
- [Components](#components)
- [Features](#features)

## Setup

1. Clone the repository
2. Install dependencies:
```bash
npm install
```
3. Create `.env` file and add environment variables
4. Start the development server:
```bash
npm run dev
```

## Dependencies

```json
{
  "dependencies": {
    "axios": "^1.x",
    "@stripe/stripe-js": "^2.x",
    "framer-motion": "^10.x",
    "lucide-react": "^0.x",
    "react": "^18.x",
    "react-confetti": "^6.x",
    "react-dom": "^18.x",
    "react-hot-toast": "^2.x",
    "react-router-dom": "^6.x",
    "recharts": "^2.x",
    "zustand": "^4.x"
  }
}
```

## Environment Variables

Create a `.env` file in the root directory:

```env
VITE_API_URL=http://localhost:5000/api
VITE_STRIPE_PUBLIC_KEY=your_stripe_publishable_key
```

## Routes

### Public Routes

#### 1. Home Page
```javascript
Path: "/"
Component: <Home />

Features:
- Featured products slider
- Categories section
- New arrivals
- Special offers
```

#### 2. Products Page
```javascript
Path: "/products"
Component: <Products />

Features:
- Product grid
- Filtering by category
- Sorting options
- Search functionality

Example state:
{
    products: [],
    filters: {
        category: null,
        minPrice: 0,
        maxPrice: 1000,
        sort: 'newest'
    }
}
```

#### 3. Product Details
```javascript
Path: "/products/:id"
Component: <ProductDetails />

Features:
- Product images gallery
- Description
- Add to cart button
- Related products

Example:
<ProductDetails 
    product={{
        _id: "123",
        name: "iPhone 15",
        price: 999.99,
        description: "Latest iPhone model",
        images: ["url1", "url2"],
        category: "electronics"
    }}
/>
```

### Protected Routes

#### 1. Cart Page
```javascript
Path: "/cart"
Component: <Cart />
Auth: Required

Features:
- Cart items list
- Quantity adjustment
- Remove items
- Apply coupon
- Checkout button

Example state:
{
    cartItems: [
        {
            product: {
                _id: "123",
                name: "iPhone 15",
                price: 999.99
            },
            quantity: 2
        }
    ],
    totalAmount: 1999.98,
    coupon: null
}
```

#### 2. Profile Page
```javascript
Path: "/profile"
Component: <Profile />
Auth: Required

Features:
- User information
- Order history
- Address management

Example:
<Profile 
    user={{
        name: "John Doe",
        email: "john@example.com",
        orders: [
            {
                _id: "order123",
                items: [],
                total: 999.99,
                status: "delivered"
            }
        ]
    }}
/>
```

### Admin Routes

#### 1. Dashboard
```javascript
Path: "/admin"
Component: <Dashboard />
Auth: Admin only

Features:
- Sales analytics
- User statistics
- Recent orders
- Stock management

Example:
<Dashboard 
    analytics={{
        totalSales: 29999.99,
        totalOrders: 150,
        totalUsers: 75,
        recentOrders: []
    }}
/>
```

## State Management

Using Zustand for state management:

### 1. Cart Store
```javascript
import create from 'zustand';

const useCartStore = create((set) => ({
    items: [],
    addItem: (product) => 
        set((state) => ({
            items: [...state.items, product]
        })),
    removeItem: (productId) =>
        set((state) => ({
            items: state.items.filter(item => item._id !== productId)
        })),
    clearCart: () => set({ items: [] })
}));
```

### 2. Auth Store
```javascript
const useAuthStore = create((set) => ({
    user: null,
    isAuthenticated: false,
    login: (userData) => 
        set({ user: userData, isAuthenticated: true }),
    logout: () => 
        set({ user: null, isAuthenticated: false })
}));
```

## Components

### 1. Product Card
```javascript
<ProductCard
    product={{
        _id: "123",
        name: "iPhone 15",
        price: 999.99,
        image: "product_image_url"
    }}
    onAddToCart={() => {}}
/>
```

### 2. Cart Item
```javascript
<CartItem
    item={{
        product: {
            _id: "123",
            name: "iPhone 15",
            price: 999.99
        },
        quantity: 2
    }}
    onUpdateQuantity={() => {}}
    onRemove={() => {}}
/>
```

## Features

### 1. Authentication
- JWT-based authentication
- Protected routes
- Role-based access control

### 2. Shopping Cart
- Add/remove items
- Update quantities
- Apply coupons
- Persistent cart storage

### 3. Checkout
```javascript
// Stripe integration
import { loadStripe } from '@stripe/stripe-js';

const stripePromise = loadStripe(import.meta.env.VITE_STRIPE_PUBLIC_KEY);

const handleCheckout = async () => {
    const stripe = await stripePromise;
    const response = await axios.post('/api/payments/create-checkout-session', {
        items: cartItems
    });
    await stripe.redirectToCheckout({
        sessionId: response.data.id
    });
};
```

### 4. Notifications
```javascript
import toast from 'react-hot-toast';

// Success notification
toast.success('Item added to cart!');

// Error notification
toast.error('Something went wrong');
```

### 5. Animations
```javascript
import { motion } from 'framer-motion';

// Page transition
<motion.div
    initial={{ opacity: 0 }}
    animate={{ opacity: 1 }}
    exit={{ opacity: 0 }}
>
    {children}
</motion.div>
```

## Development

To run in development mode:
```bash
npm run dev
```

## Building for Production

```bash
npm run build
```

## Testing

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

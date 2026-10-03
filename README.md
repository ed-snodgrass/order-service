# Order Service

Order Service is a sample e-commerce service responsible for managing the lifecycle of customer orders.

The service provides APIs for creating and retrieving orders, processing payments, maintaining customer information, and sending order notifications.

## Overview

Order Service coordinates several parts of the order workflow and integrates with external services to complete order processing.

```text
                    ┌─────────────────┐
                    │     Client      │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  Order Service  │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │  Stripe  │   │ HubSpot  │   │ SendGrid │
        └──────────┘   └──────────┘   └──────────┘
              │
              │
        ┌─────┴─────┐
        │  Payment  │
        │ Processing│
        └───────────┘
```

## Responsibilities

Order Service is responsible for:

- Creating customer orders
- Retrieving existing orders
- Processing payments
- Updating customer information
- Maintaining order status
- Persisting order data
- Sending order confirmation notifications

## Order Workflow

A typical order follows this workflow:

```text
Create Order
     │
     ▼
Validate Order
     │
     ▼
Process Payment
     │
     ▼
Update Customer
     │
     ▼
Persist Order
     │
     ▼
Send Confirmation
     │
     ▼
Order Complete
```

If payment processing fails, the order is not completed and an error is returned to the caller.

## External Dependencies

Order Service integrates with several external systems.

### Stripe

Stripe is used to process customer payments.

The service sends payment information to Stripe and uses the resulting payment status to determine whether the order can be completed.

### HubSpot

HubSpot is used to maintain customer information associated with an order.

When an order is processed, customer information may be created or updated in HubSpot.

### SendGrid

SendGrid is used to send order confirmation emails.

After an order has been successfully processed, the customer receives a confirmation containing information about the order.

### Database

Order information is persisted in the application's database and can be retrieved through the Order API.

## API

### Create Order

```http
POST /orders
```

Example request:

```json
{
  "customer": {
    "email": "customer@example.com",
    "firstName": "Jane",
    "lastName": "Smith"
  },
  "items": [
    {
      "productId": "product-123",
      "quantity": 2,
      "price": 19.99
    }
  ],
  "paymentToken": "payment-token"
}
```

Example response:

```json
{
  "id": "order-123",
  "status": "PAID",
  "total": 39.98
}
```

### Get Order

```http
GET /orders/{orderId}
```

Example response:

```json
{
  "id": "order-123",
  "status": "PAID",
  "total": 39.98
}
```

## Order Status

Orders can have the following statuses:

| Status | Description |
|---|---|
| `PENDING` | The order has been created but payment has not completed. |
| `PAID` | Payment completed successfully. |
| `PAYMENT_FAILED` | Payment processing failed. |
| `COMPLETED` | Order processing completed successfully. |

## Project Structure

The application is organized around the primary responsibilities of the order workflow.

```text
src/
├── controllers/
│   └── OrderController
│
├── services/
│   ├── OrderService
│   ├── PaymentService
│   ├── CustomerService
│   └── NotificationService
│
├── repositories/
│   └── OrderRepository
│
└── models/
    ├── Order
    ├── Customer
    └── OrderItem
```

### OrderController

Exposes the Order Service HTTP API.

### OrderService

Coordinates the overall order workflow.

### PaymentService

Handles payment processing through Stripe.

### CustomerService

Maintains customer information through HubSpot.

### NotificationService

Sends order notifications through SendGrid.

### OrderRepository

Provides persistence operations for orders.

## Running the Service

Install the project dependencies:

```bash
npm install
```

Configure the required environment variables:

```text
STRIPE_API_KEY
HUBSPOT_API_KEY
SENDGRID_API_KEY
DATABASE_URL
```

Start the application:

```bash
npm start
```

The service will be available at:

```text
http://localhost:3000
```

## Running Tests

Run the automated test suite with:

```bash
npm test
```

## Development

Start the application in development mode:

```bash
npm run dev
```

## Purpose

Order Service is a sample application used to demonstrate common application development and integration patterns within an e-commerce order-processing workflow.

It is also intended to provide a realistic service that can be used by development tools, architectural analysis tools, and automated software engineering workflows.

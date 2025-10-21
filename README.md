# DoDays Bruno API Collection

Bruno API collection for the DoDays platform. This repository contains all API endpoint definitions for testing and development, organised by resource type.

## What is Bruno?

[Bruno](https://www.usebruno.com/) is a modern API client similar to Postman or Insomnia. Unlike cloud-based alternatives, Bruno stores API collections as `.bru` files in your git repository, making them easy to version control and share with your team.

## Prerequisites

- [Bruno](https://www.usebruno.com/downloads) installed on your machine
- Access to DoDays API environments (local, staging, or production)

## Getting Started

### 1. Open the Collection

1. Launch Bruno
2. Click "Open Collection"
3. Select this repository's root directory

### 2. Select an Environment

Choose your target environment from the environment dropdown:

- **Local** - `https://api.dodays.test` (requires local API setup)
- **Staging** - `https://api.dodays-staging.co.uk`
- **Production** - `https://api.dodays.co.uk`

### 3. Authenticate

1. Navigate to `auth/token.bru`
2. Update the request body with valid credentials (or use test data):
   ```json
   {
     "email": "customer@dodays.test",
     "password": "password"
   }
   ```
3. Send the request
4. The bearer token will automatically be saved to your environment as `{{token}}`
5. All subsequent authenticated requests will use this token

## Project Structure

The collection is organised by API resource (not by endpoint type):

```
auth/              - Authentication (login, register, password reset)
account/           - User account management
bookings/          - Booking creation, payment, and management
booking-sessions/  - Session management within bookings
classes/           - Class scheduling and information
students/          - Student profile management
providers/         - Service provider information
venues/            - Venue data
catch-ups/         - Makeup/catch-up session management
subscriptions/     - Recurring subscription management
payments/          - Payment history and management
payment-intents/   - Stripe payment intent handling
vouchers/          - Voucher/discount code management
environments/      - Bruno environment configurations
```

## Environment Variables

Each environment uses these variables:

- `{{protocol}}` - HTTP protocol (https)
- `{{host}}` - API host domain
- `{{version}}` - API version (e.g., v1)
- `{{token}}` - Bearer token for authentication (auto-saved after login)
- `{{payment_intent}}` - Stripe payment intent ID (auto-saved during payment flows)

## Common Workflows

### Testing Payment Flows

1. Create a payment intent via `payment-intents/create.bru`
2. The payment intent ID is automatically saved to `{{payment_intent}}`
3. Use a test payment method ID (e.g., `pm_123456`) when creating bookings/subscriptions
4. Submit the booking/subscription request

### Working with Resources

Most resources support standard CRUD operations:

- List resources (GET collection)
- Get single resource (GET by ID or handle)
- Create resource (POST)
- Update resource (PUT/PATCH)
- Delete resource (DELETE)

Resources can be identified by:

- Numeric IDs: `class_id: 1`, `student_id: 1`
- String handles: `providers/dodays-demo`

## Bruno File Format

Each `.bru` file contains:

```
meta {
  name: Request Name
  type: http
  seq: 1
}

get {
  url: {{protocol}}://{{host}}/{{version}}/endpoint
}

auth:bearer {
  token: {{token}}
}

body:json {
  {
    "key": "value"
  }
}

script:post-response {
  // JavaScript to run after response
  bru.setEnvVar("token", res.body.token);
}
```

## Contributing

When adding new endpoints:

1. Place the `.bru` file in the appropriate resource directory
2. Use environment variables for URLs (`{{protocol}}`, `{{host}}`, `{{version}}`)
3. Include authentication where required
4. Add post-response scripts for variables that should be saved (tokens, IDs, etc.)
5. Use descriptive names and include example request bodies

## Support

For issues with the DoDays API, contact the development team.
For issues with Bruno, visit [Bruno's documentation](https://docs.usebruno.com/).

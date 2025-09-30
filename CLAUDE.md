# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Bruno API collection for the DoDays API. Bruno is an API client similar to Postman/Insomnia, using `.bru` files stored in git rather than cloud storage.

## Project Structure

This repository is organized by API resource (not by endpoint type). Each directory represents a different API resource and contains `.bru` files for individual API requests:

```
auth/           - Authentication endpoints (login, register, password reset)
account/        - User account management
bookings/       - Booking creation, payment, and management
booking-sessions/ - Individual session management within bookings
classes/        - Class scheduling and information
students/       - Student profile management
providers/      - Service provider information
venues/         - Venue data
catch-ups/      - Makeup/catch-up session management
subscriptions/  - Recurring subscription management
payments/       - Payment history and management
payment-intents/ - Stripe payment intent handling
vouchers/       - Voucher/discount code management
environments/   - Bruno environment configurations (local, staging, production)
```

## Bruno File Format

Each `.bru` file contains:

- `meta` block: Request metadata (name, type, sequence)
- HTTP method block: URL using environment variables like `{{protocol}}://{{host}}/{{version}}/path`
- `auth` block: Authentication method (bearer, none)
- `body:json` block: Request payload
- `script:post-response` block: JavaScript to run after response (e.g., saving tokens to environment)

## Environment Variables

Three environments are configured in `environments/`:

- `local.bru`: Local development (http://api.dodays.test)
- `staging.bru`: Staging (https://api.dodays-staging.co.uk)
- `production.bru`: Production (https://api.dodays.co.uk)

All environments use:

- `{{protocol}}`, `{{host}}`, `{{version}}` for URL construction
- `{{token}}` - Secret variable for bearer authentication (set via auth/token.bru post-response script)
- `{{payment_intent}}` - Stripe payment intent ID

## Authentication Flow

1. Use `auth/token.bru` to obtain a bearer token by providing email/password
2. The post-response script automatically saves the token to the environment
3. Subsequent authenticated requests use `auth:bearer` with `{{token}}`

## Common Patterns

### Test Data

Test credentials in `auth/token.bru` use fake data:

- Email: `customer@dodays.test`
- Password: `password`

### Resource Identification

Most resources use either:

- Numeric IDs (e.g., `class_id: 1`, `student_id: 1`)
- String handles/slugs (e.g., `providers/dodays-demo`)

### Payment Integration

Bookings and subscriptions integrate with Stripe:

- Create payment intents via `payment-intents/`
- Store intent ID in `{{payment_intent}}` environment variable
- Submit with booking/subscription requests using test payment method IDs (e.g., `pm_123456`)

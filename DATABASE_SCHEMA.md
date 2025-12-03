# SmartEco Rwanda - Data Storage Schema

## Overview

SmartEco uses a **unified data storage system** that captures and stores all user interactions across three channels:
- 📱 **Mobile App (PWA)**
- 📞 **USSD (*384*22#)**
- 💬 **WhatsApp Bot**

All data is synchronized and stored in a consistent format, enabling cross-channel user experience and comprehensive analytics.

---

## Database Design

### Storage Type: **Document-Based (JSON/NoSQL)**

For production deployment, we recommend:
- **Primary**: MongoDB or Firebase Firestore (for scalability)
- **Cache**: Redis (for real-time tracking data)
- **Analytics**: Google BigQuery or AWS Redshift

For the demo/MVP, we use **localStorage** with the same schema structure.

---

## Collections/Tables

### 1. `users` - User Profiles

```json
{
  "id": "id_1701234567890_abc123def",
  "created_at": "2024-12-02T10:30:00.000Z",
  "updated_at": "2024-12-02T15:45:00.000Z",
  "name": "Jean Baptiste",
  "email": "jean@example.com",
  "phone": "+250788123456",
  "district": "gasabo",
  "address": "KG 123 St, Kimironko",
  "customer_type": "residential | business",
  "business_name": "Restaurant XYZ",
  "points": 2450,
  "tier": "Eco Starter | Eco Warrior | Eco Champion | Eco Legend",
  "referral_code": "ECOJB2024",
  "referred_by": "user_id or null",
  "subscription": {
    "plan": "basic | standard | premium",
    "price": 2250,
    "status": "active | expired | cancelled",
    "started_at": "2024-11-01T00:00:00.000Z",
    "next_billing": "2024-12-01T00:00:00.000Z"
  },
  "stats": {
    "total_pickups": 24,
    "streak_days": 12,
    "waste_diverted_kg": 48.5,
    "co2_saved_kg": 23.2
  },
  "preferences": {
    "notifications": true,
    "sms_reminders": true,
    "language": "en | rw | fr"
  }
}
```

**Indexes**: `email` (unique), `phone` (unique), `referral_code` (unique)

---

### 2. `bins` - Smart Bin Registry

```json
{
  "id": "id_1701234567891_bin001",
  "user_id": "id_1701234567890_abc123def",
  "type": "organic | recyclable | general | ewaste | glass",
  "code": "BIN-ORG-A1B2",
  "qr_code_url": "https://api.smarteco.rw/qr/BIN-ORG-A1B2",
  "fill_level": 45,
  "last_collected": "2024-11-30T09:15:00.000Z",
  "location": {
    "lat": -1.9403,
    "lng": 29.8739
  },
  "status": "active | maintenance | replaced",
  "created_at": "2024-10-15T00:00:00.000Z"
}
```

**Indexes**: `user_id`, `code` (unique), `type`

---

### 3. `pickups` - Scheduled & Completed Pickups

```json
{
  "id": "id_1701234567892_pck001",
  "reference": "ECO-847291",
  "user_id": "id_1701234567890_abc123def",
  "waste_type": "organic | recyclable | general | ewaste | mixed",
  "scheduled_date": "2024-12-02",
  "scheduled_time": "09:00",
  "time_slot": "08:00-10:00",
  "status": "scheduled | in_progress | completed | cancelled | missed",
  "collector": {
    "id": "collector_001",
    "name": "Emmanuel Kwizera",
    "phone": "+250788000001",
    "vehicle": "Truck-ECO-42",
    "rating": 4.9
  },
  "tracking": {
    "eta_minutes": 12,
    "distance_km": 1.2,
    "current_location": {
      "lat": -1.9450,
      "lng": 29.8700
    },
    "route_progress": 65
  },
  "completion": {
    "arrived_at": "2024-12-02T09:15:00.000Z",
    "completed_at": "2024-12-02T09:25:00.000Z",
    "weight_kg": 5.2,
    "bin_scanned": "BIN-ORG-A1B2",
    "photo_url": "https://storage.smarteco.rw/pickups/..."
  },
  "points_earned": 52,
  "rating_given": 5,
  "feedback": "Great service!",
  "channel": "app | ussd | whatsapp",
  "created_at": "2024-12-01T14:30:00.000Z"
}
```

**Indexes**: `user_id`, `status`, `scheduled_date`, `collector.id`, `channel`

---

### 4. `transactions` - Payments & Redemptions

```json
{
  "id": "id_1701234567893_txn001",
  "reference": "TXN-20241202-001",
  "user_id": "id_1701234567890_abc123def",
  "type": "payment | redemption | refund | bonus",
  "amount": 2250,
  "currency": "RWF",
  "method": "mtn_momo | airtel_money | points | bank",
  "status": "pending | completed | failed | reversed",
  "description": "Monthly subscription payment",
  "metadata": {
    "momo_reference": "MOMO123456789",
    "phone_used": "+250788123456"
  },
  "channel": "app | ussd | whatsapp",
  "created_at": "2024-12-02T10:00:00.000Z",
  "completed_at": "2024-12-02T10:00:30.000Z"
}
```

**Indexes**: `user_id`, `type`, `status`, `reference` (unique), `channel`

---

### 5. `points_history` - Points Ledger

```json
{
  "id": "id_1701234567894_pts001",
  "user_id": "id_1701234567890_abc123def",
  "amount": 100,
  "type": "credit | debit",
  "balance_after": 2550,
  "category": "pickup | referral | bonus | redemption | adjustment",
  "description": "Organic waste pickup #ECO-847291",
  "reference_id": "id_1701234567892_pck001",
  "reference_type": "pickup | transaction | promotion",
  "channel": "app | ussd | whatsapp | system",
  "created_at": "2024-12-02T09:30:00.000Z"
}
```

**Indexes**: `user_id`, `type`, `category`, `created_at`

---

### 6. `notifications` - User Notifications

```json
{
  "id": "id_1701234567895_ntf001",
  "user_id": "id_1701234567890_abc123def",
  "type": "success | warning | info | alert",
  "category": "pickup | payment | points | system | promotion",
  "title": "Pickup Confirmed",
  "message": "Your collector Emmanuel is on the way! ETA: 12 minutes",
  "action_url": "/tracking/ECO-847291",
  "read": false,
  "read_at": null,
  "push_sent": true,
  "sms_sent": false,
  "created_at": "2024-12-02T09:00:00.000Z"
}
```

**Indexes**: `user_id`, `read`, `type`, `created_at`

---

### 7. `ussd_sessions` - USSD Interaction Logs

```json
{
  "id": "id_1701234567896_ussd001",
  "user_id": "id_1701234567890_abc123def",
  "phone": "+250788123456",
  "session_code": "*384*22#",
  "screens_visited": ["main", "schedule", "schedule_date", "schedule_time", "schedule_confirm"],
  "actions": [
    { "screen": "main", "input": "1", "time": "2024-12-02T10:00:00.000Z" },
    { "screen": "schedule", "input": "1", "time": "2024-12-02T10:00:15.000Z" },
    { "screen": "schedule_date", "input": "1", "time": "2024-12-02T10:00:30.000Z" }
  ],
  "outcome": "pickup_scheduled | points_checked | payment_made | abandoned",
  "duration_seconds": 45,
  "carrier": "MTN | Airtel",
  "created_at": "2024-12-02T10:00:00.000Z",
  "ended_at": "2024-12-02T10:00:45.000Z"
}
```

**Indexes**: `user_id`, `phone`, `outcome`, `created_at`

---

### 8. `whatsapp_sessions` - WhatsApp Conversation Logs

```json
{
  "id": "id_1701234567897_wa001",
  "user_id": "id_1701234567890_abc123def",
  "phone": "+250788123456",
  "messages": [
    { "type": "received", "text": "Welcome to SmartEco!", "time": "2024-12-02T10:00:00.000Z" },
    { "type": "sent", "text": "Schedule Pickup", "time": "2024-12-02T10:00:05.000Z" },
    { "type": "received", "text": "What type of waste?", "time": "2024-12-02T10:00:07.000Z" }
  ],
  "message_count": 12,
  "intents_detected": ["schedule_pickup", "check_points"],
  "outcome": "pickup_scheduled | inquiry | support_request",
  "satisfaction_rating": 5,
  "created_at": "2024-12-02T10:00:00.000Z",
  "ended_at": "2024-12-02T10:05:00.000Z"
}
```

**Indexes**: `user_id`, `phone`, `outcome`, `created_at`

---

### 9. `collectors` - Collector/Driver Profiles

```json
{
  "id": "collector_001",
  "name": "Emmanuel Kwizera",
  "phone": "+250788000001",
  "email": "emmanuel@smarteco.rw",
  "photo_url": "https://storage.smarteco.rw/collectors/...",
  "vehicle": {
    "id": "vehicle_001",
    "plate": "RAD 123 A",
    "type": "truck",
    "capacity_kg": 500
  },
  "zones": ["gasabo", "kicukiro"],
  "status": "active | inactive | on_break",
  "current_location": {
    "lat": -1.9450,
    "lng": 29.8700,
    "updated_at": "2024-12-02T09:00:00.000Z"
  },
  "stats": {
    "total_pickups": 847,
    "on_time_rate": 98,
    "average_rating": 4.9,
    "total_weight_kg": 4235
  },
  "created_at": "2024-01-15T00:00:00.000Z"
}
```

---

### 10. `analytics` - Aggregated Metrics

```json
{
  "id": "analytics_global",
  "period": "all_time | daily | weekly | monthly",
  "date": "2024-12-02",
  "metrics": {
    "total_users": 5000,
    "active_users": 3500,
    "total_pickups": 25000,
    "waste_diverted_kg": 125000,
    "co2_saved_kg": 62500,
    "trees_equivalent": 5000,
    "recycling_rate": 67,
    "revenue_rwf": 11250000,
    "points_distributed": 2500000,
    "points_redeemed": 1800000
  },
  "by_channel": {
    "app": { "pickups": 15000, "users": 3000 },
    "ussd": { "pickups": 7000, "users": 1500 },
    "whatsapp": { "pickups": 3000, "users": 500 }
  },
  "by_waste_type": {
    "organic": { "kg": 75000, "pickups": 15000 },
    "recyclable": { "kg": 30000, "pickups": 6000 },
    "general": { "kg": 15000, "pickups": 3000 },
    "ewaste": { "items": 500, "pickups": 500 }
  },
  "updated_at": "2024-12-02T23:59:59.000Z"
}
```

---

## Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER CHANNELS                             │
├─────────────────┬─────────────────┬─────────────────────────────┤
│   📱 Mobile App  │   📞 USSD       │   💬 WhatsApp               │
│   (PWA/Native)  │   (*384*22#)    │   (Bot API)                 │
└────────┬────────┴────────┬────────┴────────┬────────────────────┘
         │                 │                 │
         ▼                 ▼                 ▼
┌─────────────────────────────────────────────────────────────────┐
│                    API GATEWAY / BACKEND                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │ REST API    │  │ USSD Gateway│  │ WhatsApp Business API   │  │
│  │ (Node.js)   │  │ (Africa's   │  │ (Twilio/Meta)          │  │
│  │             │  │  Talking)   │  │                         │  │
│  └──────┬──────┘  └──────┬──────┘  └───────────┬─────────────┘  │
│         │                │                     │                 │
│         └────────────────┴─────────────────────┘                 │
│                          │                                       │
│                    ┌─────▼─────┐                                │
│                    │ Service   │                                │
│                    │ Layer     │                                │
│                    └─────┬─────┘                                │
└──────────────────────────┼──────────────────────────────────────┘
                           │
         ┌─────────────────┼─────────────────┐
         ▼                 ▼                 ▼
┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│  MongoDB    │   │   Redis     │   │  BigQuery   │
│  (Primary)  │   │   (Cache)   │   │ (Analytics) │
│             │   │             │   │             │
│ - Users     │   │ - Sessions  │   │ - Reports   │
│ - Pickups   │   │ - Tracking  │   │ - Metrics   │
│ - Bins      │   │ - Real-time │   │ - ML Data   │
│ - Trans.    │   │   location  │   │             │
└─────────────┘   └─────────────┘   └─────────────┘
```

---

## API Endpoints (RESTful)

### Authentication
- `POST /api/auth/register` - Create new user
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - End session
- `POST /api/auth/verify-otp` - Phone verification

### Users
- `GET /api/users/me` - Get current user
- `PUT /api/users/me` - Update profile
- `GET /api/users/me/stats` - Get user statistics

### Pickups
- `POST /api/pickups` - Schedule pickup
- `GET /api/pickups` - List user pickups
- `GET /api/pickups/:id` - Get pickup details
- `PUT /api/pickups/:id/cancel` - Cancel pickup
- `GET /api/pickups/:id/track` - Real-time tracking

### Bins
- `GET /api/bins` - List user bins
- `POST /api/bins/:code/scan` - Log QR scan
- `PUT /api/bins/:id/report` - Report full bin

### Points
- `GET /api/points/balance` - Get balance
- `GET /api/points/history` - Transaction history
- `POST /api/points/redeem` - Redeem rewards

### Payments
- `POST /api/payments/initiate` - Start payment
- `POST /api/payments/callback` - MoMo callback
- `GET /api/payments/history` - Payment history

---

## Security Considerations

1. **Authentication**: JWT tokens with refresh mechanism
2. **Encryption**: All data encrypted at rest (AES-256)
3. **API Security**: Rate limiting, CORS, input validation
4. **Phone Verification**: OTP for sensitive operations
5. **Data Privacy**: GDPR-compliant data handling
6. **Audit Logging**: All data modifications logged

---

## Deployment Recommendations

### For MVP/Pilot (< 10,000 users):
- **Database**: MongoDB Atlas (M10 cluster)
- **Cache**: Redis Cloud (free tier)
- **Hosting**: Vercel/Netlify (frontend), Railway (backend)
- **Cost**: ~$50-100/month

### For Scale (> 100,000 users):
- **Database**: MongoDB Atlas (M30+) or AWS DocumentDB
- **Cache**: AWS ElastiCache
- **Hosting**: AWS ECS/EKS or Google Cloud Run
- **Analytics**: BigQuery + Looker
- **Cost**: ~$500-2000/month

---

## Data Retention Policy

| Data Type | Retention Period | Action |
|-----------|------------------|--------|
| User Profiles | Indefinite | Archive on deletion |
| Pickups | 3 years | Archive |
| Transactions | 7 years | Archive (tax compliance) |
| Points History | 3 years | Archive |
| Notifications | 90 days | Delete |
| USSD Sessions | 1 year | Anonymize |
| WhatsApp Sessions | 1 year | Anonymize |
| Analytics | Indefinite | Aggregate |

---

*Document Version: 1.0*
*Last Updated: December 2024*
*SmartEco Rwanda - Engineering Team*

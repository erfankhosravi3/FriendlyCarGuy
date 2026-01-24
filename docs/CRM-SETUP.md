# Friendly Car Guy CRM - Complete Setup Guide

## Table of Contents
1. [Airtable Schema](#airtable-schema)
2. [Security Setup](#security-setup)
3. [Deployment Instructions](#deployment-instructions)
4. [Configuration Checklist](#configuration-checklist)

---

# Airtable Schema

## Overview

This CRM is designed for automotive sales with the following capabilities:
- Instant caller ID with full customer context
- Two-way SMS messaging
- Call logging with recordings and transcriptions
- Inventory matching based on customer preferences
- Deal pipeline tracking
- Automated follow-up reminders
- Commission and ROI tracking

---

## Table 1: Contacts

The heart of the CRM. Every person you interact with.

### Basic Info
| Field | Type | Notes |
|-------|------|-------|
| Name | Text | Full name |
| Phone | Phone | Primary - used for matching incoming calls/texts |
| Phone 2 | Phone | Secondary/work number |
| Email | Email | |
| Address | Long Text | For paperwork/delivery |
| Preferred Contact | Single Select | Options: Call, Text, Email |
| Language | Single Select | Options: English, Spanish, Other |

### Lead Info
| Field | Type | Notes |
|-------|------|-------|
| Source | Link to Sources | Where they came from |
| Referred By | Link to Contacts | If referral, link to referring contact |
| Status | Single Select | Options: New, Working, Hot, Sold, Lost, Be-Back, Service Customer |
| Temperature | Rating (1-5) | How hot is this lead |
| Lost Reason | Single Select | Options: Price, Trade Value, Credit, Bought Elsewhere, No Response, Other |

### Preferences
| Field | Type | Notes |
|-------|------|-------|
| Looking For | Single Select | Options: New, Used, Certified, Any |
| Body Style | Multiple Select | Options: Sedan, SUV, Truck, Van, Coupe, Hatchback |
| Makes | Multiple Select | Options: Toyota, Honda, Ford, Chevrolet, etc. |
| Models | Text | Specific models interested in |
| Budget Min | Currency | |
| Budget Max | Currency | |
| Payment Target | Currency | Monthly payment goal |
| Timeline | Single Select | Options: Today, This Week, This Month, 2-3 Months, Just Looking |
| Must Haves | Long Text | Features they need |
| Deal Breakers | Long Text | What they won't accept |

### Trade-In
| Field | Type | Notes |
|-------|------|-------|
| Has Trade | Checkbox | |
| Trade Year | Number | |
| Trade Make | Text | |
| Trade Model | Text | |
| Trade Mileage | Number | |
| Trade Condition | Single Select | Options: Excellent, Good, Fair, Poor |
| Trade Payoff | Currency | Amount owed |
| Trade Estimate | Currency | Your estimate |

### Financial
| Field | Type | Notes |
|-------|------|-------|
| Credit Situation | Single Select | Options: Excellent, Good, Fair, Challenged, Unknown |
| Pre-Approved | Checkbox | |
| Down Payment | Currency | Cash available |
| Co-Buyer | Text | Co-signer name if needed |

### History
| Field | Type | Notes |
|-------|------|-------|
| Previous Customer | Checkbox | Bought from you before |
| Previous Purchase | Date | When |
| Previous Vehicle | Text | What they bought |

### System
| Field | Type | Notes |
|-------|------|-------|
| Created | DateTime | Auto-created timestamp |
| Last Contact | DateTime | Auto-updated on call/text |
| Next Follow Up | Date | Pulled from Tasks |
| Owner | Text | Your name (for future multi-user) |
| Notes | Long Text | General notes |
| Tags | Multiple Select | Options: VIP, Cash Buyer, Family Friend, Fleet, Employee |

---

## Table 2: Calls

Every phone interaction logged automatically.

| Field | Type | Notes |
|-------|------|-------|
| Contact | Link to Contacts | Linked contact |
| Phone | Phone | For unknown callers |
| Direction | Single Select | Options: Inbound, Outbound |
| Status | Single Select | Options: Answered, Missed, Voicemail, No Answer |
| Duration | Number | Seconds |
| Recording URL | URL | Link to recording file |
| Transcription | Long Text | AI transcribed content |
| Summary | Long Text | AI-generated call summary |
| Sentiment | Single Select | Options: Positive, Neutral, Negative |
| Call Notes | Long Text | Your manual notes |
| Follow Up Created | Checkbox | Did this call create a task? |
| Timestamp | DateTime | When call happened |
| Twilio SID | Text | Twilio reference ID |

---

## Table 3: Messages

Every SMS logged automatically.

| Field | Type | Notes |
|-------|------|-------|
| Contact | Link to Contacts | Linked contact |
| Phone | Phone | For unknown contacts |
| Direction | Single Select | Options: Inbound, Outbound |
| Body | Long Text | Message content |
| Status | Single Select | Options: Received, Sent, Delivered, Read, Failed |
| Template Used | Link to Templates | If sent from template |
| Timestamp | DateTime | When sent/received |
| Twilio SID | Text | Twilio reference ID |

---

## Table 4: Tasks

Follow-ups and to-dos.

| Field | Type | Notes |
|-------|------|-------|
| Contact | Link to Contacts | Related contact |
| Deal | Link to Deals | If related to active deal |
| Type | Single Select | Options: Follow-up Call, Follow-up Text, Send Info, Appointment Reminder, Credit App, Paperwork, Delivery, Post-Sale Follow-up, Other |
| Title | Text | Quick description |
| Description | Long Text | Details |
| Due Date | Date | |
| Due Time | Text | Optional time |
| Priority | Single Select | Options: Low, Normal, High, Urgent |
| Status | Single Select | Options: Pending, In Progress, Completed, Skipped |
| Outcome | Long Text | What happened |
| Created | DateTime | |
| Completed | DateTime | |
| Auto Generated | Checkbox | Created by system vs manual |

---

## Table 5: Appointments

Scheduled meetings and test drives.

| Field | Type | Notes |
|-------|------|-------|
| Contact | Link to Contacts | |
| Type | Single Select | Options: Test Drive, Showroom Visit, Phone Call, Delivery, Service, Other |
| Vehicle | Link to Inventory | Specific vehicle if applicable |
| Date | Date | |
| Time | Text | |
| Duration | Number | Minutes |
| Status | Single Select | Options: Scheduled, Confirmed, Arrived, Completed, No Show, Cancelled, Rescheduled |
| Reminder Sent | Checkbox | |
| Location | Text | |
| Notes | Long Text | |
| Outcome | Long Text | What happened |
| Created | DateTime | |

---

## Table 6: Deals

Active sales pipeline tracking.

### Stage
| Field | Type | Notes |
|-------|------|-------|
| Contact | Link to Contacts | |
| Vehicle | Link to Inventory | |
| Stage | Single Select | Options: Prospect, Test Drive, Negotiation, Pencil, F&I, Delivery, Sold, Dead |
| Stage Changed | DateTime | When stage last changed |
| Days in Stage | Formula | `DATETIME_DIFF(NOW(), {Stage Changed}, 'days')` |

### Numbers
| Field | Type | Notes |
|-------|------|-------|
| Asking Price | Currency | |
| Negotiated Price | Currency | |
| Trade Allowance | Currency | |
| Trade ACV | Currency | Actual cash value |
| Down Payment | Currency | |
| Monthly Payment | Currency | |
| Term | Number | Months |
| Rate | Percent | Interest rate |

### Details
| Field | Type | Notes |
|-------|------|-------|
| Objections | Long Text | What's holding them back |
| Competition | Text | Other dealers/vehicles considering |
| Decision Maker | Text | Who makes the final call |
| Hot Button | Long Text | What will close this deal |

### Dates
| Field | Type | Notes |
|-------|------|-------|
| Created | DateTime | |
| Expected Close | Date | |
| Actual Close | DateTime | |
| Delivery Date | Date | |

### Outcome
| Field | Type | Notes |
|-------|------|-------|
| Won | Checkbox | |
| Lost Reason | Single Select | Options: Price, Trade, Credit, Bought Elsewhere, No Response |
| Lost To | Text | Competitor details |

### Money
| Field | Type | Notes |
|-------|------|-------|
| Front Gross | Currency | Profit on vehicle |
| Back Gross | Currency | F&I profit |
| Total Gross | Formula | `{Front Gross} + {Back Gross}` |
| Commission | Formula | `{Total Gross} * 0.25` (adjust your rate) |

---

## Table 7: Inventory

All available vehicles.

### Identity
| Field | Type | Notes |
|-------|------|-------|
| Stock Number | Text | Primary identifier |
| VIN | Text | 17-character VIN |

### Vehicle
| Field | Type | Notes |
|-------|------|-------|
| Year | Number | |
| Make | Text | |
| Model | Text | |
| Trim | Text | |
| Type | Single Select | Options: New, Pre-Owned, Certified Pre-Owned |

### Specs
| Field | Type | Notes |
|-------|------|-------|
| Body Style | Single Select | Options: Sedan, SUV, Truck, Coupe, Van, Hatchback |
| Exterior Color | Text | |
| Interior Color | Text | |
| Mileage | Number | |
| Fuel Type | Single Select | Options: Gas, Hybrid, Electric, Plug-in Hybrid, Diesel |
| Engine | Text | |
| Transmission | Single Select | Options: Automatic, Manual, CVT |
| Drivetrain | Single Select | Options: FWD, RWD, AWD, 4WD |

### Pricing
| Field | Type | Notes |
|-------|------|-------|
| MSRP | Currency | New only |
| Internet Price | Currency | Listed price |
| Invoice | Currency | Your cost (new) |
| ACV | Currency | Your cost (used) |
| Min Gross | Currency | Floor for negotiation |

### Marketing
| Field | Type | Notes |
|-------|------|-------|
| Features | Long Text | Key selling points |
| Description | Long Text | Listing description |
| Image URL | URL | Main photo |
| Image Gallery | Long Text | Multiple URLs, one per line |
| Listing URL | URL | Website link |
| Video URL | URL | Walk-around video |

### Status
| Field | Type | Notes |
|-------|------|-------|
| Status | Single Select | Options: Available, Pending, Sold, Incoming, Hold |
| Days on Lot | Formula | `DATETIME_DIFF(NOW(), {Date Added}, 'days')` |
| Hot Deal | Checkbox | Manager special |

### Tracking
| Field | Type | Notes |
|-------|------|-------|
| Date Added | Date | |
| Last Updated | DateTime | |
| Sold Date | Date | |
| Sold To | Link to Contacts | |
| Sold Price | Currency | |

---

## Table 8: Sources

Track where leads come from for ROI analysis.

| Field | Type | Notes |
|-------|------|-------|
| Name | Text | Source name |
| Type | Single Select | Options: Walk-in, Phone-in, Internet, Referral, Repeat, Be-Back, Service Drive |
| Channel | Single Select | Options: Organic, Paid, Social, Direct |
| Active | Checkbox | Still using this source |
| Cost | Currency | Monthly/total ad spend |
| Contacts | Link to Contacts | All leads from source (auto-populated) |
| Deals Won | Rollup | Count of sold deals |
| Revenue | Rollup | Total gross from source |
| ROI | Formula | `({Revenue} - {Cost}) / {Cost} * 100` |

---

## Table 9: Templates

Quick message templates for SMS.

| Field | Type | Notes |
|-------|------|-------|
| Name | Text | Quick reference |
| Category | Single Select | Options: Greeting, Follow-up, Appointment, Info Request, Negotiation, Post-Sale, Holiday, Be-Back |
| Body | Long Text | Message with {placeholders} |
| Use Count | Number | Times used |
| Last Used | DateTime | |
| Active | Checkbox | |

### Starter Templates

**Greeting**
| Name | Body |
|------|------|
| After Visit | Hi {Name}, thanks for coming in today! I enjoyed meeting you. Let me know if you have any questions about the {Vehicle}. - Erfan |
| After Call | Hey {Name}, great talking with you! Let me know if you have any other questions. |
| New Lead | Hi {Name}, this is Erfan from Camelback Toyota. I saw you were interested in a {Vehicle}. I'd love to help - what questions can I answer? |

**Follow-up**
| Name | Body |
|------|------|
| Day After | Hey {Name}, just checking in after our conversation yesterday. Still thinking about the {Vehicle}? Happy to answer any questions. |
| 3 Day | Hi {Name}, wanted to follow up - the {Vehicle} you liked is still available. Any questions I can help with? |
| Week | Hey {Name}, hope you're doing well! Still have that {Vehicle} - let me know if you'd like to take another look. |
| Price Drop | Great news {Name}! The {Vehicle} you looked at just had a price drop. Want me to send you the updated numbers? |

**Appointment**
| Name | Body |
|------|------|
| Confirm | Hi {Name}, confirming your appointment for {Date} at {Time}. See you then! |
| Reminder | Hey {Name}, just a reminder about your visit tomorrow at {Time}. Looking forward to seeing you! |
| No Show | Hi {Name}, I missed you today! No worries - let me know when you'd like to reschedule. |
| Directions | Here's directions to Camelback Toyota: [link]. I'll meet you in front when you arrive! |

**Info Request**
| Name | Body |
|------|------|
| Credit App | Here's the link to get pre-approved: [link]. Takes 2 minutes and won't hurt your credit. Let me know when you're done! |
| Send Photos | Here's some more photos of the {Vehicle}: [link]. Let me know what you think! |
| Trade Quote | To get you an accurate trade value, can you send me a few pics of your {Trade}? Front, back, interior, and odometer. |

**Post-Sale**
| Name | Body |
|------|------|
| Thank You | {Name}, congratulations on your new {Vehicle}! It was a pleasure working with you. Enjoy the ride! |
| Review Ask | Hey {Name}, hope you're loving the new {Vehicle}! If you have a minute, I'd really appreciate a Google review: [link]. Thanks! |
| 30 Day Check | Hey {Name}! It's been about a month - how's the {Vehicle} treating you? Any questions? |
| Service Reminder | Hi {Name}, just a heads up - your {Vehicle} is probably due for its first service soon. Want me to help you schedule it? |

**Be-Back**
| Name | Body |
|------|------|
| Check In | Hi {Name}, it's Erfan from Camelback Toyota. I know it's been a while - just wondering if you're still in the market? |
| New Inventory | Hey {Name}, we just got some new inventory that matches what you were looking for. Want me to send you some options? |

---

## Table 10: Activity Feed

Unified log of all interactions (auto-populated by n8n).

| Field | Type | Notes |
|-------|------|-------|
| Contact | Link to Contacts | |
| Type | Single Select | Options: Call, Text, Appointment, Task, Deal Update, Note |
| Summary | Text | What happened |
| Details | Long Text | Full content |
| Linked Record | Text | ID of related call/message/etc |
| Timestamp | DateTime | |
| Auto | Checkbox | System generated |

---

## Recommended Views

### Contacts
- **All Contacts** - Default grid view
- **Hot Leads** - Filter: Status = "Hot" OR Temperature >= 4
- **Needs Follow-up** - Filter: Next Follow Up <= TODAY()
- **New This Week** - Filter: Created >= 7 days ago
- **By Source** - Grouped by Source field
- **Lost Leads** - Filter: Status = "Lost" (for be-back campaigns)

### Tasks
- **My Tasks Today** - Filter: Due Date = TODAY(), Status = "Pending"
- **Overdue** - Filter: Due Date < TODAY(), Status = "Pending"
- **This Week** - Filter: Due Date is within 7 days
- **Completed** - Filter: Status = "Completed"

### Deals
- **Active Pipeline** - Filter: Stage != "Sold" AND Stage != "Dead"
- **Pipeline Board** - Kanban view grouped by Stage
- **Won This Month** - Filter: Won = true, Actual Close is this month
- **Lost Analysis** - Filter: Stage = "Dead", grouped by Lost Reason

### Inventory
- **Available** - Filter: Status = "Available"
- **New Cars** - Filter: Type = "New", Status = "Available"
- **Used Cars** - Filter: Type = "Pre-Owned" OR Type = "Certified", Status = "Available"
- **Under $25k** - Filter: Internet Price < 25000, Status = "Available"
- **SUVs** - Filter: Body Style = "SUV", Status = "Available"
- **Trucks** - Filter: Body Style = "Truck", Status = "Available"
- **Aged Inventory** - Filter: Days on Lot > 60, Status = "Available"

---

## Relationships Diagram

```
                         ┌───────────┐
                         │  Sources  │
                         └─────┬─────┘
                               │
                               ▼
┌───────────┐           ┌───────────┐           ┌───────────┐
│ Referrals │──────────▶│ CONTACTS  │◀──────────│ Templates │
│ (self-    │           └─────┬─────┘           └───────────┘
│  link)    │                 │
└───────────┘    ┌────────────┼────────────┬────────────┐
                 │            │            │            │
                 ▼            ▼            ▼            ▼
           ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────────┐
           │  Calls  │  │Messages │  │  Tasks  │  │Appointments │
           └─────────┘  └─────────┘  └────┬────┘  └──────┬──────┘
                 │            │           │              │
                 └────────────┴─────┬─────┴──────────────┘
                                    ▼
                             ┌─────────────┐
                             │Activity Feed│
                             └─────────────┘

           ┌───────────┐           ┌───────────┐
           │ Inventory │◀─────────▶│   Deals   │
           └─────┬─────┘           └─────┬─────┘
                 │                       │
                 └───────────┬───────────┘
                             ▼
                       ┌───────────┐
                       │ CONTACTS  │
                       └───────────┘
```

---

# Security Setup

## Overview

This app handles sensitive customer data (names, phones, financial info). Security is critical.

### Threat Model
- **Unauthorized access** - Someone else accessing your CRM
- **Data interception** - Man-in-the-middle attacks
- **Session hijacking** - Stolen login sessions
- **API abuse** - Unauthorized API calls
- **Data breach** - Exposure of customer data

---

## Authentication

### Primary: Passkey (WebAuthn)
- Uses Face ID / Touch ID on iPhone
- Most secure - can't be phished
- No password to remember or steal
- Cryptographic proof of identity

### Backup: Magic Link
- Email a login link to erfan@friendlycarguy.com
- Link expires in 15 minutes
- One-time use only

### Emergency: PIN Code
- 6-digit PIN
- Only works from previously-authenticated devices
- Rate limited (5 attempts, then locked for 1 hour)

### Session Management
- JWT tokens with 24-hour expiration
- Refresh tokens rotate on each use
- Sessions invalidated on logout
- Auto-logout after 24 hours of inactivity
- Maximum 3 active sessions

---

## Transport Security

### HTTPS Everywhere
- All traffic encrypted with TLS 1.3
- Automatic HTTPS redirect
- HSTS header with 1-year max-age
- Preload list eligible

### Headers
```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=()
```

### Content Security Policy
```
Content-Security-Policy:
  default-src 'self';
  script-src 'self';
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  font-src 'self' https://fonts.gstatic.com;
  img-src 'self' data: https:;
  connect-src 'self' https://api.airtable.com https://*.twilio.com https://*.n8n.cloud;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self';
```

---

## API Security

### Authentication
- Every API request requires Bearer token
- Token validated on every request
- Invalid tokens return 401 immediately

### Authorization
- User can only access their own data
- No admin endpoints exposed to frontend
- Principle of least privilege

### Rate Limiting
- 100 requests per minute per user
- 10 login attempts per hour per IP
- Exceeded limits return 429

### Input Validation
- All inputs sanitized
- SQL injection prevention (Airtable handles this)
- XSS prevention on all text fields

---

## Data Security

### What's Stored Where

| Data | Location | Encryption |
|------|----------|------------|
| Customer contacts | Airtable | At-rest encryption |
| Call recordings | Twilio/Cloudflare R2 | At-rest + transit |
| Messages | Airtable | At-rest encryption |
| Session tokens | Browser memory | Not persisted |
| Refresh tokens | HttpOnly cookie | Encrypted |
| API keys | n8n environment | Never exposed |

### What's NOT Stored in Browser
- Airtable API key
- Twilio credentials
- Customer financial details
- Full credit card numbers
- Social security numbers

### Data Retention
- Call recordings: 90 days (configurable)
- Messages: Indefinite
- Activity logs: 1 year
- Deleted contacts: 30-day soft delete, then permanent

---

## Credential Management

### Never Expose
These credentials stay in n8n only:
- Airtable API key
- Twilio Account SID
- Twilio Auth Token
- Push notification keys

### Environment Variables (n8n)
```
AIRTABLE_API_KEY=pat_xxxxx
AIRTABLE_BASE_ID=appxxxxx
TWILIO_ACCOUNT_SID=ACxxxxx
TWILIO_AUTH_TOKEN=xxxxx
TWILIO_PHONE_NUMBER=+16029057670
JWT_SECRET=<random-64-char-string>
VAPID_PUBLIC_KEY=xxxxx
VAPID_PRIVATE_KEY=xxxxx
```

### Rotation Schedule
- JWT secret: Every 6 months
- API keys: Annually or if compromised
- Twilio auth token: Annually

---

## Infrastructure Security

### Vercel (Recommended)
- Automatic DDoS protection
- Edge network (fast + secure)
- Automatic SSL certificates
- SOC 2 Type 2 compliant
- GDPR compliant

### n8n Cloud
- SOC 2 compliant
- Data encrypted at rest
- Regular security audits
- EU or US data residency options

### Airtable
- SOC 2 Type 2 compliant
- Data encrypted at rest (AES-256)
- HTTPS only
- Regular penetration testing

---

## Incident Response

### If You Suspect a Breach
1. **Immediately** change your Airtable API key
2. Rotate Twilio auth token
3. Invalidate all active sessions (change JWT secret)
4. Review Airtable audit logs
5. Review n8n execution logs
6. Check Twilio usage logs for anomalies

### Contact
- Airtable security: security@airtable.com
- Twilio security: security@twilio.com
- Vercel security: security@vercel.com

---

## Security Checklist

### Initial Setup
- [ ] Enable 2FA on GitHub account
- [ ] Enable 2FA on Vercel account
- [ ] Enable 2FA on Airtable account
- [ ] Enable 2FA on Twilio account
- [ ] Enable 2FA on n8n account
- [ ] Enable 2FA on GoDaddy account
- [ ] Use unique, strong passwords (password manager recommended)
- [ ] Set up Passkey authentication in app
- [ ] Configure backup email for magic links
- [ ] Set 6-digit backup PIN

### Deployment
- [ ] Verify HTTPS is enforced
- [ ] Check security headers (securityheaders.com)
- [ ] Verify CSP is active
- [ ] Test rate limiting
- [ ] Confirm API keys not in frontend code
- [ ] Review Vercel environment variables

### Ongoing
- [ ] Monthly: Review active sessions
- [ ] Monthly: Check Twilio usage for anomalies
- [ ] Quarterly: Review Airtable access logs
- [ ] Quarterly: Update dependencies
- [ ] Annually: Rotate API keys
- [ ] Annually: Security review

---

# Deployment Instructions

## Step 1: Create Vercel Account
1. Go to vercel.com
2. Sign up with GitHub
3. Authorize Vercel to access your repos

## Step 2: Import Project
1. Click "Add New Project"
2. Select `FriendlyCarGuy` repository
3. Configure:
   - **Framework Preset**: Other
   - **Root Directory**: `app`
   - **Build Command**: (leave empty)
   - **Output Directory**: (leave empty)
4. Click Deploy

## Step 3: Add Custom Domain
1. Go to Project Settings → Domains
2. Add `app.friendlycarguy.com`
3. Vercel will show DNS instructions
4. In GoDaddy DNS:
   - Add CNAME record: `app` → `cname.vercel-dns.com`
5. Wait for DNS propagation (up to 48 hours, usually minutes)
6. Vercel auto-provisions SSL certificate

## Step 4: Configure Environment Variables
In Vercel Project Settings → Environment Variables:
```
N8N_WEBHOOK_URL=https://your-n8n-instance.com/webhook/
JWT_SECRET=<generate-random-64-char-string>
```

## Step 5: Set Up n8n
1. Sign up at n8n.cloud (or self-host)
2. Create workflows for:
   - Twilio incoming call webhook
   - Twilio incoming SMS webhook
   - Twilio call status webhook
   - Contact API (CRUD)
   - Messages API
   - Calls API
   - Auth endpoints
3. Add credentials:
   - Airtable API key
   - Twilio credentials
4. Activate workflows

## Step 6: Configure Twilio
1. Update TwiML Bin to point to n8n webhook
2. Configure SMS webhook to n8n
3. Enable call recording (recordings go to n8n → storage)

## Step 7: Test Everything
1. Call Twilio number → verify forwarding + logging
2. Text Twilio number → verify appears in app
3. Send text from app → verify delivery
4. Check contact lookup works
5. Verify recordings are saved
6. Test push notifications

---

# Configuration Checklist

## Accounts Needed
- [ ] GitHub (you have this)
- [ ] Vercel (free tier)
- [ ] Airtable (free tier works, Pro recommended)
- [ ] Twilio (you have this)
- [ ] n8n Cloud (free tier) or self-hosted
- [ ] Cloudflare R2 (optional, for recording storage)

## DNS Records (GoDaddy)
- [ ] `card` CNAME → `erfankhosravi3.github.io` (existing)
- [ ] `app` CNAME → `cname.vercel-dns.com` (new)

## Twilio Configuration
- [ ] Phone number: +1 (602) 905-7670
- [ ] Voice webhook → n8n endpoint
- [ ] SMS webhook → n8n endpoint
- [ ] Call recording enabled
- [ ] Voicemail TwiML Bin configured

## Airtable Setup
- [ ] Create base "Friendly Car Guy CRM"
- [ ] Create all 10 tables with fields
- [ ] Set up linked record relationships
- [ ] Create recommended views
- [ ] Generate API key (Personal Access Token)

## n8n Workflows
- [ ] Incoming call handler
- [ ] Call complete handler (save recording)
- [ ] Incoming SMS handler
- [ ] Outgoing SMS sender
- [ ] Contact API (get, create, update)
- [ ] Messages API
- [ ] Calls API
- [ ] Auth endpoints (login, verify, refresh)

## App Configuration
- [ ] Update API base URL to n8n
- [ ] Configure push notification keys
- [ ] Set up authentication flow
- [ ] Test on iPhone Safari
- [ ] Add to Home Screen
- [ ] Verify standalone mode works

---

# Support

## Useful Links
- Twilio Console: https://console.twilio.com
- Airtable: https://airtable.com
- Vercel Dashboard: https://vercel.com/dashboard
- n8n Cloud: https://n8n.cloud
- GitHub Repo: https://github.com/erfankhosravi3/FriendlyCarGuy

## Troubleshooting

### Calls not forwarding
1. Check Twilio Console → Phone Numbers → Voice Configuration
2. Verify TwiML Bin is selected and saved
3. Check Twilio error logs

### SMS not appearing in app
1. Check n8n workflow execution logs
2. Verify Twilio SMS webhook is configured
3. Check Airtable for new records

### App not loading
1. Check Vercel deployment status
2. Verify DNS is pointing correctly
3. Check browser console for errors

### Push notifications not working
1. Verify HTTPS (required for push)
2. Check notification permissions in browser
3. Verify VAPID keys are configured
4. iOS requires app added to Home Screen

---

*Document created: January 2026*
*Last updated: January 2026*

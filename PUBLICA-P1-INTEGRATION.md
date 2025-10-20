# P1 Publica Integration Guide

## Overview
This guide explains how P1's Publica instance can integrate with AdNexus SSP for Server-Side Ad Insertion (SSAI).

## Quick Start

### 1. Access the SSP Management UI
Navigate to http://localhost:3000 (or your production URL)

### 2. Generate Publica Tags
1. Go to **Tag Generator** section
2. Select **"P1 (Publica Instance)"** as Publisher
3. Select **"Publica"** as Device Type
4. Configure:
   - Site ID: Your Publica site identifier
   - Ad Size: 1920x1080 (Full HD) or other CTV sizes
   - Floor Price: Minimum CPM (e.g., $2.50)
   - PMP Deal ID: Optional for fixed pricing deals

### 3. Integration Endpoints

#### SSAI Endpoint (Server-Side Ad Insertion)
```
POST https://ssp.ad.nexus/publica/ssai
```

**Request Body:**
```json
{
  "publisher_id": "p1-publica",
  "site_id": "your-site-id",
  "content_id": "$$CONTENT_ID$$",
  "device_id": "$$DEVICE_ID$$",
  "ip": "$$IP_ADDRESS$$",
  "ua": "$$USER_AGENT$$",
  "floor_price": 2.50,
  "deal_id": "PMP-2024-P1",  // Optional
  "params": {
    "size": "1920x1080",
    "content_genre": "$$CONTENT_GENRE$$",
    "content_rating": "$$CONTENT_RATING$$",
    "content_language": "$$CONTENT_LANGUAGE$$",
    "duration": $$DURATION$$
  }
}
```

**Response:**
```json
{
  "ad_break_id": "uuid",
  "duration": 30,
  "vast_url": "https://ssp.ad.nexus/publica/vast?...",
  "ads": [
    {
      "id": "ad-id",
      "duration": 30,
      "media_url": "https://cdn.example.com/video.mp4",
      "click_url": "https://ssp.ad.nexus/publica/click?...",
      "title": "Ad Title",
      "advertiser": "BidsCube",
      "cpm": 5.50
    }
  ],
  "tracking_urls": {
    "impression": ["https://ssp.ad.nexus/publica/pixel/impression?..."],
    "click": ["https://ssp.ad.nexus/publica/click?..."],
    "complete": ["https://ssp.ad.nexus/publica/pixel/complete?..."],
    "quartile_1": ["https://ssp.ad.nexus/publica/pixel/q1?..."],
    "quartile_2": ["https://ssp.ad.nexus/publica/pixel/q2?..."],
    "quartile_3": ["https://ssp.ad.nexus/publica/pixel/q3?..."]
  },
  "cache_buster": "1234567890"
}
```

#### VAST Endpoint (Direct VAST Tag)
```
GET https://ssp.ad.nexus/publica/vast
```

**Query Parameters:**
- `pub`: Publisher ID (p1-publica)
- `site`: Site ID
- `content_id`: Content identifier
- `device_id`: Device ID
- `ip`: IP address
- `ua`: User agent
- `floor`: Floor price (CPM)
- `deal`: Optional PMP deal ID

**Example URL:**
```
https://ssp.ad.nexus/publica/vast?pub=p1-publica&site=site-001&content_id=$$CONTENT_ID$$&device_id=$$DEVICE_ID$$&ip=$$IP_ADDRESS$$&ua=$$USER_AGENT$$&floor=2.50&deal=PMP-2024-P1
```

## Configuration Steps for P1's Publica Instance

### Step 1: Update Ad Decisioning Endpoint
Replace your current ad decisioning endpoint in Publica with:
```
https://ssp.ad.nexus/publica/ssai
```

### Step 2: Configure Macro Mapping
Map your Publica macros to our expected parameters:

| Publica Macro | AdNexus Parameter |
|---------------|------------------|
| `[CONTENT_ID]` | `content_id` |
| `[DEVICE_ID]` | `device_id` |
| `[IP_ADDRESS]` | `ip` |
| `[USER_AGENT]` | `ua` |
| `[CONTENT_GENRE]` | `params.content_genre` |
| `[CONTENT_RATING]` | `params.content_rating` |
| `[CONTENT_LANGUAGE]` | `params.content_language` |
| `[DURATION]` | `params.duration` |

### Step 3: Set Floor Pricing
Configure your minimum CPM floor price (recommended: $2.50 for CTV)

### Step 4: Enable PMP Deals (Optional)
For fixed-price private marketplace deals:
1. Contact AdNexus to set up a PMP deal
2. You'll receive a Deal ID (e.g., "PMP-2024-P1")
3. Configure this Deal ID in your Publica settings

## Features

### Server-Side Ad Insertion (SSAI)
- ✅ Manifest manipulation support
- ✅ HLS and DASH compatible
- ✅ Frame-accurate ad insertion
- ✅ Client-side ad avoidance prevention

### BidsCube Integration
When P1 is configured as a priority publisher:
- Direct connection to BidsCube DSP
- Access to premium demand sources
- Higher fill rates and CPMs
- Revenue share optimization

### Real-Time Bidding
- OpenRTB 2.5 compliant
- Sub-100ms response times
- Parallel partner bidding
- First-price auction

### Tracking & Analytics
- Impression tracking
- Click tracking
- Video completion tracking
- Quartile reporting (25%, 50%, 75%)
- Real-time analytics dashboard

## Testing

### Test SSAI Request
```bash
curl -X POST https://ssp.ad.nexus/publica/ssai \
  -H "Content-Type: application/json" \
  -d '{
    "publisher_id": "p1-publica",
    "site_id": "test-site",
    "content_id": "test-content",
    "device_id": "test-device",
    "ip": "192.168.1.1",
    "ua": "Mozilla/5.0 (Smart TV)",
    "floor_price": 2.50
  }'
```

### Test VAST Request
```bash
curl "https://ssp.ad.nexus/publica/vast?pub=p1-publica&site=test-site&content_id=test&device_id=test&ip=192.168.1.1&ua=SmartTV&floor=2.50"
```

## Monitoring

### Health Check
```bash
curl https://ssp.ad.nexus/health
```

### Metrics Endpoint
```bash
curl https://ssp.ad.nexus/metrics
```

## Support

For integration support or to set up PMP deals:
- Email: support@ad.nexus
- Dashboard: https://ssp.ad.nexus
- Documentation: https://docs.ad.nexus/publica

## Revenue Optimization Tips

1. **Set Appropriate Floor Prices**: CTV inventory typically commands $15-30 CPMs
2. **Enable Header Bidding**: Allow multiple DSPs to compete
3. **Use PMP Deals**: Guarantee revenue with fixed-price deals
4. **Optimize Ad Pod Length**: 60-90 seconds optimal for CTV
5. **Pass Device/Content Signals**: Better targeting = higher CPMs

## Compliance

- COPPA compliant
- GDPR ready (EU)
- CCPA compliant (California)
- TAG certified against fraud
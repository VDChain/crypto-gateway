<div align="center">

<img src="https://raw.githubusercontent.com/VDChain/brand-assets/main/logo_256.png" width="80" height="80" alt="VDChain Logo" />

# Crypto Payment Gateway

### Non-Custodial Cryptocurrency Payment Gateway — Coming Soon

[![Status](https://img.shields.io/badge/Status-Coming%20Soon-F59E0B?style=for-the-badge)](https://github.com/VDChain)
[![Chain](https://img.shields.io/badge/Chain%20ID-882022-3B82F6?style=for-the-badge)](https://vdscan.io)
[![Non%20Custodial](https://img.shields.io/badge/Non--Custodial-Architecture-22C55E?style=for-the-badge)](https://github.com/VDChain)

**[VDChain Ecosystem](https://github.com/VDChain)** · **[Explorer](https://vdscan.io)**

</div>

---

## Overview

VDChain Crypto Payment Gateway is a non-custodial cryptocurrency payment infrastructure being developed as part of the VDChain ecosystem. It enables merchants and businesses to accept cryptocurrency payments directly into their own wallets — with zero custodial risk, real-time on-chain detection, and instant webhook notifications.

Unlike traditional payment gateways that hold customer funds, this gateway operates on a fully non-custodial model — funds flow directly from the customer to the merchant wallet on-chain. The gateway only monitors and notifies, never holding or controlling any funds.

---

## Key Principles

### Non-Custodial by Design
Customer Wallet
↓  (direct on-chain transfer)
Merchant Wallet
↓  (gateway monitors blockchain)
Webhook Notification → Merchant Backend

The gateway never holds, moves, or controls funds at any point. It only listens to the blockchain and notifies your system when a payment is detected and confirmed.

---

## Planned Features

### For Merchants
- Accept crypto payments directly into your own wallet
- No funds held by gateway — full self-custody
- Real-time payment detection and confirmation alerts
- Payment expiry and underpayment handling
- Simple merchant dashboard for payment management
- Transaction history and reporting

### For Developers
- Clean REST API — integrate in minutes
- Webhook system with HMAC-SHA256 signature verification
- Payment lifecycle events — created, pending, confirmed, expired, failed
- Idempotent API design — safe to retry requests
- Detailed error responses for easy debugging
- SDKs planned for Node.js, PHP, and Python

### Payment Lifecycle
POST /payments (create payment)
↓
Payment address generated
↓
Customer sends crypto to address
↓
Gateway detects transaction on-chain
↓
Webhook: payment.pending
↓
Required confirmations reached
↓
Webhook: payment.confirmed
↓
Funds in merchant wallet

### Webhook Events
| Event | Description |
|-------|-------------|
| `payment.created` | New payment invoice created |
| `payment.pending` | Transaction detected, awaiting confirmations |
| `payment.confirmed` | Payment fully confirmed on-chain |
| `payment.expired` | Payment window closed without payment |
| `payment.underpaid` | Payment received but amount insufficient |

### Webhook Security
Every webhook request includes an `X-Signature` header for verification:
```javascript
const crypto = require('crypto')

function verifyWebhook(payload, signature, secret) {
  const expected = crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex')
  return `sha256=${expected}` === signature
}

app.post('/webhook', (req, res) => {
  const sig = req.headers['x-signature']
  if (!verifyWebhook(req.body, sig, process.env.WEBHOOK_SECRET)) {
    return res.status(401).send('Unauthorized')
  }
  const { event, data } = req.body
  if (event === 'payment.confirmed') {
    // Fulfill order
  }
  res.status(200).send('OK')
})
```

### API Integration Example
```javascript
// Create a payment
const payment = await fetch('https://gateway.example/api/v1/payments', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-API-Key': 'YOUR_API_KEY'
  },
  body: JSON.stringify({
    amount: '10.00',
    currency: 'USDT',
    network: 'VDChain',
    order_id: 'ORDER_123',
    webhook_url: 'https://yourapp.com/webhook',
    expires_in: 900 // 15 minutes
  })
}).then(r => r.json())

console.log(payment.result.pay_address)
// → 0xMerchantWalletAddress
console.log(payment.result.expires_at)
// → 2026-07-01T12:15:00Z
```

---

## Development Status

| Module | Status |
|--------|--------|
| Core Payment Engine | In Development |
| On-Chain Detection | In Development |
| Webhook Delivery System | In Development |
| Merchant Dashboard | In Development |
| REST API | In Development |
| Multi-Chain Support | Planned |
| Developer SDKs | Planned |
| Hosted Checkout Page | Planned |

---

## Stay Updated

Follow VDChain for launch announcements:

- GitHub: https://github.com/VDChain
- Explorer: https://vdscan.io

---

<div align="center">

**Built on VDChain Mainnet · Chain ID: 882022 · Powered by VDC**

</div>

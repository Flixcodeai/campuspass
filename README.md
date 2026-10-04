# CampusPass

**On-chain event tickets for student initiatives, paid with Solana Pay.**

Built for *Build an MVP with Solana at WHU* (Superteam Germany, WHU Hackathon 2026).

## Problem

Student initiatives at universities like WHU run dozens of events per semester (networking nights, pitch evenings, parties). Ticketing today means:

- PayPal / bank transfers matched by hand in Excel
- Payment fees of ~3% (PayPal / Stripe / Eventbrite) and payouts that take days
- Screenshot "tickets" that are easy to fake or share
- No live overview of sales at the door

## Solution

CampusPass turns a Solana payment into the ticket itself:

1. The organizer creates an event in 30 seconds (name, price, capacity, payout wallet) and gets a share link.
2. Students pay via **Solana Pay** QR (Phantom / Solflare mobile) or the Phantom browser extension.
3. Each payment carries a unique **reference key** plus an **on-chain memo** `CP1|<eventId>|<name>`.
4. CampusPass finds the transaction on-chain and issues a QR ticket.
5. At the door, the organizer scans the ticket. CampusPass verifies **recipient, amount and event directly on the Solana blockchain**.
6. The organizer dashboard reads sales and revenue live from the chain.

No backend, no database. Money goes straight to the initiative's wallet in about one second for a fraction of a cent.

## Solana features used

| Feature | Usage |
|---|---|
| Solana Pay (transfer request) | QR / deep-link payment with `amount`, `reference`, `memo` |
| SPL Memo program | Binds payment to event + attendee, on-chain |
| Reference keys + `getSignaturesForAddress` | Detects the payment for a specific checkout |
| `getTransaction` (jsonParsed) | Verifies ticket validity at check-in |
| Wallet adapter (Phantom) | Desktop payment + organizer wallet connect |

## Target users

- **Organizers:** student initiatives and clubs (first users: WHU student initiatives)
- **Attendees:** students who already use or are curious about crypto wallets

## Run it

It is a single static file.

```bash
# any static server
npx serve .
# or just open index.html in a browser
```

Live demo: GitHub Pages (see repo "About" link).

### Test on devnet

1. Install Phantom, enable *Settings → Developer Settings → Testnet Mode* (Solana Devnet).
2. Get devnet SOL at https://faucet.solana.com
3. Organizer tab → create event with your wallet → open share link → buy ticket → check-in tab → verify.

## Roadmap

- USDC / EURC payments (SPL token transfers, stable prices)
- Tickets as compressed NFTs (resale with royalty caps, POAP-style attendance proof)
- On-chain check-in state (Anchor program) instead of per-device check-in
- Email login via embedded wallets for non-crypto students
- Multi-initiative dashboard for university student unions

## Tech

Vanilla HTML/CSS/JS · `@solana/web3.js` · Solana JSON-RPC (devnet) · qrcodejs · html5-qrcode

## License

MIT

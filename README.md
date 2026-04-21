# 🍑 PlumSwap – Never miss a discount again.

**PlumSwap** is a free and open-source, peer-to-peer platform for scanning, storing, or exchanging discount & credit coupons.  
Users own their data, share directly with others, and support multiple coupon formats including **QR codes, barcodes, alphanumeric codes, and images**.  
Now with optional **bitcoin and lightning payments** for swapping coupons.

---

## ✨ Features
- 📱 **Multi-code support**: QR, barcode, alphanumeric, images  
- 🔒 **Private & secure**: No central server, encrypted peer-to-peer exchange  
- 🌍 **Cross-platform**: Works on Android, iOS, Linux, Windows, and macOS  
- 📦 **Offline-first**: Coupons stored locally, with optional encrypted backup  
- ⚡ **Decentralized**: Powered by bitcoin, lightning, and P2P protocols like libp2p/WebRTC, IPFS.  
- 💱 **Bitcoin Integration**: Swap coupons for bitcoin via on-chain or lightning payments  

---

## 🏗️ Architecture Overview
  
  PlumSwap is a peer-to-peer coupon exchange platform built as a Progressive Web App (PWA). Users can list, trade, and give away coupons using a fully
  decentralized identity model — no passwords, no accounts. Identity is based on client-side ed25519 keypairs with cryptographic signature verification.

  Stack

  - Backend: Python / FastAPI
  - Database: SQLite via SQLAlchemy ORM (swap-ready for PostgreSQL)
  - Frontend: Single-page vanilla HTML/CSS/JS PWA
  - Auth: ed25519 keypair (client-side) + signature verification (server-side)
  - Real-time: WebSocket signaling server (WebRTC-ready)
  - Port: 8002

  Identity & Security

  - Keypair generation: ed25519 via Web Crypto API (client-side)
  - peer_id: SHA256(public_key)[:16] — deterministic, no registration required
  - Request signing: ed25519(canonical_json + "." + timestamp) with 5-minute replay protection window
  - Private key never leaves the client device

  API Structure (/api/v1)

  - /peers — POST, GET /:id, GET /:id/ratings — Peer registration, profiles, reputation
  - /listings — GET, POST, PUT /:id, DELETE /:id — Coupon listing CRUD with search/filter
  - /exchanges — GET, POST, PUT /:id, POST /:id/complete — Exchange workflow (request → accept/reject → complete + rate)
  - /ws/signal/{peer_id} — WebSocket — Real-time signaling for peer discovery
  - /signal/online — GET — Online peer count
  - /config — GET — App constants (categories, formats, countries, ICE servers)

  Data Model

  - Peers — identity, display name, reputation score (0–100), trade count
  - Listings — coupon details (title, store, category, format, value, expiry), exchange type (free/trade/paid), tags, country
  - Exchanges — workflow states: pending → accepted/rejected → completed/failed/cancelled
  - Ratings — 1–5 score per completed exchange, feeds into reputation system

  Reputation System

  - Starting score: 100
  - Successful exchange: +5
  - Failed/disputed exchange: -15
  - Range: 0–100

  Configuration

  - 9 categories: retail, food, travel, entertainment, health, tech, grocery, beauty, other
  - 4 coupon formats: QR, barcode, alphanumeric, image
  - 3 exchange types: free, trade, paid
  - 6 supported countries: US, JP, EU, UK, CN, VE
  - Max 50 listings per peer, 8 tags per listing

  Frontend

  - Single-file PWA (index.html) with service worker (sw.js) for offline support
  - Orange (#C05A00) accent — intentional Bitcoin/crypto branding
  - IBM Plex Sans / Mono typography
  - Mobile-first responsive design
    
---

## 🤝 Contributing
We welcome contributions from developers, designers, and Bitcoin enthusiasts!  
Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to get started.  

---

## 📜 License
This project is licensed under the [MIT License](LICENSE).  
You’re free to use, modify, and share PlumSwap — see the license file for details.

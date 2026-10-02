# 🚗 ParkFlow AI - Smart Parking Allocation System

A full-stack intelligent parking management and slot allocation system built with **Node.js**, **Express**, **SQLite**, and a modern, responsive **Tailwind CSS** interactive user interface.

---

## 🌟 Key Highlights

- **Dual-Interface System**:
  - **Driver Portal**: Real-time spot discovery, smart allocation preview, self-service check-in, digital parking pass with QR code, live duration & cost ticker, and 1-click checkout with itemized receipts.
  - **Staff & Admin Operations Dashboard**: Multi-floor visual parking map, gate attendant rapid check-in desk, live active yard monitor with instant search, analytics & capacity meters, and dynamic hourly pricing rules.
- **Smart Allocation Engine**:
  - Automatically matches vehicles with the best available bay based on dimensions, physical constraints, walking distance to elevators/entrances, and amenities.
  - **Electric Vehicles (EV)**: Prioritizes bays with Level 2 fast charging stations.
  - **Accessible / Disabled**: Allocates lowest walking-distance spots with zero-step ramp access.
  - **Compact / Two-Wheelers**: Allocated compact bays first to preserve standard and SUV bays.
  - **SUV / Large Trucks**: Restricts access to wide bays with high vertical clearance.
- **Real-Time Database Persistence**:
  - Backed by **SQLite** (`data/parking.db`), automatically preserving all bays, sessions, transactions, and system settings across restarts without external database setup.

---

## 📁 Project Architecture

```
parking-allocation/
├── data/
│   └── parking.db            # Persistent SQLite database file
├── server/
│   ├── server.js             # Express app, RESTful API endpoints, static server
│   ├── database.js           # SQLite wrapper with auto-seeding and disk persistence
│   └── allocation.js         # Smart Allocation Engine algorithm & compatibility matrix
├── public/
│   ├── index.html            # Dual-interface UI (Driver Portal + Staff Dashboard)
│   ├── app.js                # Reactive front-end state management, live sync & timers
│   └── styles.css            # Custom parking bay styling, animations & print media
├── package.json              # Project scripts and dependencies
└── README.md                 # System documentation
```

---

## 🚀 Getting Started

### 1. Launch the Server
From the project directory:
```bash
npm start
```
*(or run `node server/server.js`)*

### 2. Access the Application
Open your web browser and navigate to:
```
http://localhost:3000
```

---

## 🚘 Feature Tour

### 1. Driver Portal
- **Smart Spot Finder**:
  - Input License Plate, Driver Name, and select vehicle type (Sedan, EV, Accessible, SUV, Compact).
  - Toggle EV Fast Charger preference or select a preferred floor.
  - View **live algorithmic recommendation preview** showing the assigned bay, walking score, and rationale.
  - 1-click **Confirm Check-In** issues a **Digital Ticket & QR Pass**.
- **Active Pass & Exit Kiosk**:
  - Search by License plate or Ticket ID.
  - Real-time ticker tracks elapsed parking time and accrued cost by the second.
  - Integrated checkout with simulated payment (Card, Apple Pay, UPI, Cash) and printable itemized receipts.
  - Automatic **15-minute free grace period**.

### 2. Staff & Admin Operations
- **Interactive Multi-Floor Map**:
  - Switch between **Floor 1 (Ground)**, **Floor 2 (Level 1)**, and **Floor 3 (Rooftop)**.
  - Real-time status indicators:
    - 🟢 **Available**
    - 🔴 **Occupied** (displays parked plate and duration)
    - 🟡 **Reserved**
    - ⚪ **Maintenance**
  - Click any bay to inspect parked driver details, process checkout, or lock the spot for maintenance.
- **Gate Express Check-In Desk**:
  - Rapid entry tool for attendant booths with automated slot suggestion or manual attendant override.
- **Active Yard Monitor**:
  - Real-time table of all currently parked vehicles with instant search and 1-click checkout.
- **Analytics & Pricing Configuration**:
  - Occupancy rate %, revenue summary, and capacity breakdown by category.
  - Live pricing editor to adjust hourly rates and grace period on the fly.
  - Demo reset button to restore initial sample state.

---

## 📡 RESTful API Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/slots` | List all parking slots (filters: `floor`, `type`, `status`) |
| `POST` | `/api/slots/allocate` | Run allocation algorithm preview without creating a ticket |
| `POST` | `/api/slots/:id/status` | Update bay status (`available`, `maintenance`, `reserved`) |
| `POST` | `/api/tickets/checkin` | Check in a vehicle (auto-assigns or uses manual bay) |
| `POST` | `/api/tickets/reserve` | Reserve a bay in advance |
| `GET` | `/api/tickets/active` | List all active parking sessions with live elapsed cost |
| `POST` | `/api/tickets/checkout` | Check out vehicle, calculate duration, and generate receipt |
| `GET` | `/api/stats` | Facility KPIs: capacity, occupancy %, revenue, breakdowns |
| `GET` | `/api/settings` | Retrieve current pricing rules and grace period |
| `POST` | `/api/settings` | Update pricing rules in SQLite database |
| `POST` | `/api/admin/reset` | Reset and re-seed the parking lot for demonstrations |

---

## 📄 License
MIT License. Built for modern, congestion-free parking facilities.

<div align="center">

<img src="atlas%20v2/public/logo.png" alt="Atlas Charge Plus+ logo" width="84" />

# Atlas Charge Plus+

On-demand mobile EV charging for London.<br />
Clients book a 150 kW rapid charge from wherever they are parked, and a charging truck comes to them.<br />
One platform for clients, technicians, dispatchers and administrators, on the web, Android and Android Automotive.

<br />

![React](https://img.shields.io/badge/React-18-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Vite](https://img.shields.io/badge/Vite-5-646CFF?style=flat-square&logo=vite&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-3ECF8E?style=flat-square&logo=supabase&logoColor=white)
![Capacitor](https://img.shields.io/badge/Capacitor-8-119EFF?style=flat-square&logo=capacitor&logoColor=white)
![Android Automotive](https://img.shields.io/badge/Android_Automotive-Car_App_Library-3DDC84?style=flat-square&logo=android&logoColor=white)
![Leaflet](https://img.shields.io/badge/Leaflet-1.9-199900?style=flat-square&logo=leaflet&logoColor=white)
![Gemini](https://img.shields.io/badge/Google_Gemini-AI_analytics-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)

[**Live demo**](https://atlas-charge-plus.vercel.app) &nbsp;·&nbsp; [Android APK](apk-server/atlas-charge-plus.apk) &nbsp;·&nbsp; [Screenshots](#screenshots) &nbsp;·&nbsp; [Getting started](#getting-started)

<br />

<img src="docs/screenshots/cover.png" alt="Atlas Charge Plus+ home page" width="900" />

</div>

## Overview

Atlas Charge Plus+ manages a fleet of mobile DC charging trucks across Greater London. A client drops a pin, picks a vehicle and a charge package, and the platform dispatches the nearest available truck. The technician drives to the car, plugs in, and the client follows the session live, from the truck's arrival to the final invoice.

The project has three front ends on a single Supabase backend:

| Layer | What it does |
|---|---|
| **Web app** | React SPA with a public site and four role-based workspaces (client, technician, dispatcher, super admin) |
| **Android app** | The same app packaged with Capacitor 8, using native GPS, notifications and status bar control |
| **Android Automotive app** | In-vehicle screens for the technician, built with the Google Car App Library: job queue, job details, navigation and charging |
| **Backend** | Supabase: PostgreSQL with row-level security on every table, authentication, realtime updates |

## Features

**Client**
- Request a mobile rapid charge by dropping a pin on the map
- Register vehicles with battery size, connector (CCS Combo 2, Tesla NACS, Type 2, CHAdeMO) and charge port location
- Compare tariffs, rush-hour multipliers and kWh packages before booking
- Follow the order live: technician assignment, truck en route, charging telemetry, then an itemised VAT invoice
- Manage payment methods, invoices, profile, referral code and notification settings

**Technician**
- Queue of open requests, with one-tap claim and navigation to the client's car
- Job flow from arrival to charging to completion
- Live charging telemetry: output power, energy delivered, truck buffer and vehicle state of charge

**Fleet dispatcher**
- Live fleet map with every mobile unit and active dispatch
- Truck status, stored energy and depot management
- Technician duty status, licences and truck assignment
- Dispatch queue with manual assignment

**Super admin**
- Revenue, energy dispensed, fleet readiness and customer satisfaction at a glance
- Natural-language and voice questions about the fleet, answered by Google Gemini over live aggregates
- Package demand insights and GPS breadcrumb history for every truck
- Full control over tariffs, packages, connectors and UI labels

## Screenshots

<sub>Captured from a local build of the web app at 1440 × 900 using demo data.</sub>

### Public website

<img src="docs/screenshots/home-how-it-works.png" alt="How it works" width="100%" />

<table>
  <tr>
    <td width="50%"><img src="docs/screenshots/home-features.png" alt="Features" /><p align="center"><b>Why Atlas Charge+</b></p></td>
    <td width="50%"><img src="docs/screenshots/tariffs.png" alt="Tariffs and packages" /><p align="center"><b>Tariffs and charge packages</b></p></td>
  </tr>
  <tr>
    <td><img src="docs/screenshots/connectors.png" alt="Connector standards" /><p align="center"><b>Charging hardware and connector standards</b></p></td>
    <td><img src="docs/screenshots/reviews.png" alt="Customer reviews" /><p align="center"><b>Customer reviews</b></p></td>
  </tr>
  <tr>
    <td colspan="2"><img src="docs/screenshots/login.png" alt="Sign in with demo roles" /><p align="center"><b>Sign in, with one-click demo accounts for each role</b></p></td>
  </tr>
</table>

### Client workspace

<table>
  <tr>
    <td width="50%"><img src="docs/screenshots/client-vehicles.png" alt="EV garage" /><p align="center"><b>EV garage</b></p></td>
    <td width="50%"><img src="docs/screenshots/client-wallet.png" alt="Payment methods" /><p align="center"><b>Payment methods</b></p></td>
  </tr>
  <tr>
    <td><img src="docs/screenshots/client-invoices.png" alt="Invoices" /><p align="center"><b>Itemised VAT invoices</b></p></td>
    <td><img src="docs/screenshots/client-profile.png" alt="Profile" /><p align="center"><b>Profile, referrals and notifications</b></p></td>
  </tr>
</table>

### Fleet operations console

<table>
  <tr>
    <td width="50%"><img src="docs/screenshots/fleet-units.png" alt="Mobile charging units" /><p align="center"><b>Mobile charging units</b></p></td>
    <td width="50%"><img src="docs/screenshots/fleet-technicians.png" alt="Technicians" /><p align="center"><b>Field technicians</b></p></td>
  </tr>
  <tr>
    <td colspan="2"><img src="docs/screenshots/fleet-dispatches.png" alt="Dispatch queue" /><p align="center"><b>Active dispatches and assignment</b></p></td>
  </tr>
</table>

### Analytics

<table>
  <tr>
    <td width="50%"><img src="docs/screenshots/analytics-ai.png" alt="AI statistics assistant" /><p align="center"><b>AI statistics assistant</b></p></td>
    <td width="50%"><img src="docs/screenshots/analytics-insights.png" alt="Executive insights" /><p align="center"><b>Executive insights and package demand</b></p></td>
  </tr>
</table>

## Architecture

```mermaid
flowchart LR
    subgraph Clients
        W[Web app<br/>React + Vite]
        A[Android app<br/>Capacitor]
        C[Android Automotive<br/>Car App Library]
    end
    subgraph Supabase
        DB[(PostgreSQL<br/>row-level security)]
        AU[Auth]
        RT[Realtime]
    end
    W --> AU & DB & RT
    A --> AU & DB & RT
    C -->|REST| DB
    W -->|analytics questions| G[Google Gemini]
    A -. CarDataBridge .-> C
```

| Area | Technologies |
|---|---|
| Web | React 18, Vite 5, React Router 6, Context API, Leaflet 1.9, Lucide icons, custom CSS design system |
| Mobile | Capacitor 8 (Android SDK 24+), Geolocation, Local Notifications, Status Bar, Splash Screen |
| In-vehicle | Android Automotive OS, Google Car App Library, Java, `CarDataBridge` plugin syncing the driver session |
| Backend | Supabase: PostgreSQL, GoTrue auth, Realtime, row-level security with `SECURITY DEFINER` role helpers |
| AI | Google Gemini API, Web Speech API for voice input |
| Security | Cloudflare Turnstile on sign-in and sign-up, role-based routes, RLS on all 16 tables |
| Hosting | Vercel (web), APK distribution page in `apk-server/` |

## Project structure

```
Atlas-Charge-Plus/
├── atlas v2/
│   ├── android/              # Capacitor Android project, including the Android Automotive screens
│   ├── public/               # Static assets, logo and hero images
│   ├── src/
│   │   ├── components/       # Layout, maps, telemetry widgets, AI assistant, shared UI
│   │   ├── context/          # Auth, data and order state
│   │   ├── pages/            # 14 pages across the public site and the four roles
│   │   ├── services/         # Supabase API services, Gemini stats, native bridges
│   │   └── styles/           # Design system
│   ├── supabase/schema.sql   # Tables, indexes, role helpers and RLS policies
│   └── test_system_suite.mjs # End-to-end checks against Supabase for all four roles
├── apk-server/               # Prebuilt Android APKs and download page
└── docs/screenshots/         # Images used in this README
```

## Getting started

**Requirements**: Node.js 18+, npm 9+, a Supabase project, and Android Studio for the native builds.

```bash
cd "atlas v2"
npm install
npm run dev        # http://localhost:3000
npm run build      # production build in dist/
```

**Configuration** (`atlas v2/.env`):

| Variable | Purpose |
|---|---|
| `VITE_SUPABASE_URL` | Supabase project URL |
| `VITE_SUPABASE_ANON_KEY` | Supabase anonymous key |
| `VITE_GEMINI_API_KEY` | Optional. Enables the AI assistant (a key can also be entered in the app) |

**Database**: run `atlas v2/supabase/schema.sql` in the Supabase SQL editor to create the 16 tables, role helper functions and row-level security policies.

**Android and Android Automotive**:

```bash
npx cap sync android
npx cap open android   # build and run from Android Studio
```

## Database and security

| Table | Contents |
|---|---|
| `users`, `client_profiles`, `driver_profiles` | Accounts, roles, referral codes, licences, duty status and ratings |
| `fleet_trucks`, `truck_connectors`, `connector_types` | Trucks, stored energy, GPS position and mounted connectors |
| `customer_vehicles` | Client EVs, battery size, connector and port location |
| `charge_packages`, `pricing_tariffs` | kWh packages, callout fees, per-kWh rates and peak multipliers |
| `orders`, `order_telemetry_logs`, `truck_gps_breadcrumbs` | Order state machine with 8 statuses, charging telemetry and GPS history |
| `payment_methods`, `order_invoices`, `order_reviews` | Payments, invoices and customer reviews |
| `system_ui_labels` | Editable UI labels |

Row-level security is enabled on every table. Clients only see their own vehicles, orders, payment methods and invoices. Technicians see the jobs assigned to them and open requests. Dispatchers and super admins get operational access through `SECURITY DEFINER` helper functions (`is_dispatcher()`, `is_super_admin()`, `is_staff()`).

## Contributors

Engineering graduation project at **Euromed University of Fes (UEMF)**, **EIDIA** (School of Digital Engineering and Artificial Intelligence), in collaboration with **Atlas E-Mobility Group**.

- **Yassine Aitaouicha**
- **Haytam Hannoun**
- **Brahim Nakkar**

University supervisor: **Mr. Taha Ait Tchakoucht**

## License

Copyright 2025/2026 Atlas E-Mobility Group. All rights reserved.

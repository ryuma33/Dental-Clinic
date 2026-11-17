# Pearl Dental Care: clickable app prototype

An iPhone-style prototype of a dental clinic app, built in the same style as `park4u`. Everything is in **one file**, `index.html` (HTML + CSS + JavaScript).

## How to open it

**Easiest:** double-click `index.html` and it opens in your browser.

**Like a real website (recommended):** run a tiny local server in this folder:

```bash
python -m http.server 5183
```

Then open <http://localhost:5183>. On a laptop it shows inside an iPhone frame. On a phone it fills the screen.

> The map needs an internet connection: it loads the free **Leaflet** library and **OpenStreetMap** tiles. No API key is needed. Without internet, everything else still works, but the map area shows a message instead.

## Screens and how they connect

```
Tab bar:  Home · Appointments · Clinic

Home ─ Book Now ─► Choose a Dentist ─► Date & Time ─► Patient Details ─► Pick-up & Drop ─► Review Booking ─► Booking Confirmed
  │                 (or tap a dentist on Home to skip straight to Date & Time)   │
  │                                                                              └─► Choose Location (map picker)
  └─ Next appointment ─► Appointment Details ─► Reschedule (Date & Time) / Cancel

Appointments tab ─► Upcoming | Past & Cancelled ─► Appointment Details
Clinic tab ─► map + address + hours ─► Directions / Open in Maps (Google or Apple)
```

| # | Screen | What you can do |
|---|--------|-----------------|
| 1 | Home | Greeting, "Book Now", your next appointment, the dentists, clinic location, call reception |
| 2 | Choose a Dentist | 5 dentists with initials, speciality, experience, ★ rating |
| 3 | Date & Time | Next 14 days (Sunday closed). Booked and past slots are crossed out and can't be tapped |
| 4 | Patient Details | Name, mobile, age, gender, reason, notes. Pre-filled with sample values; you can edit them |
| 5 | Pick-up & Drop | Optional switch, pick-up and drop locations, "drop at the same place", live price breakdown |
| 6 | Choose Location | Drag or tap the map under the pin, search a place / type an address, "Use My Location" |
| 7 | Review Booking | Everything together: charges, **total**, mock payment (UPI / card / pay at clinic) |
| 8 | Booking Confirmed | Booking ID and pick-up time |
| 9 | Appointments | Upcoming and Past & Cancelled lists |
| 10 | Appointment Details | View everything, **Reschedule** (choose a new slot) or **Cancel** (asks first) |
| 11 | Clinic | Map with the clinic pin, address, hours, phone, Directions, Open in Maps |

## Prices

- Consultation: **₹500**
- Pick-up & drop: **₹99 base fee + ₹15 per km for each leg**
  - leg 1 = pick-up → clinic, leg 2 = clinic → drop
  - km = straight-line distance × 1.3, rounded to 0.1 km (an estimate of road distance)
  - Example: pick-up 2 km away + drop 0.6 km away → ₹99 + ₹30 + ₹9 = **₹138**

## Payment is a MOCK

No money moves. The app never asks for real card numbers, UPI IDs or PINs. The "UPI" and "Visa •••• 4242" options are fixed demo values.

## Where data is saved

Bookings and your patient details are saved in the browser's **localStorage** under the key `pearldental.v1`. All reads and writes are inside `try/catch`, so in private mode the app still works but forgets everything on reload.
To start fresh, open the browser console (F12) and run `localStorage.removeItem('pearldental.v1')`, then reload.

## Assumptions (marked `// ASSUMPTION` in the code)

1. Clinic name **Pearl Dental Care**, address (Mullakkal Rd, Alappuzha 688011), phone and map position are made up.
2. The 5 dentists, their specialities, ratings and review counts are sample data. All charge ₹500.
3. Opening hours: Mon–Sat, 9:30 AM–1 PM and 3 PM–7:30 PM, 30-minute slots, closed Sunday.
4. Other patients' bookings are simulated (about 1 in 3 slots is taken). Your own bookings also block slots.
5. Road distance ≈ straight-line distance × 1.3 (real routing would need an API key).
6. One ₹99 base fee per booking, then ₹15/km on each leg.
7. Pick-up & drop only within 25 km of the clinic.
8. The driver picks you up 45 minutes before the appointment.
9. Address search uses a built-in list of ~18 Alappuzha places with approximate coordinates (no geocoding service). You can also type any address and drag the pin to it. A dropped pin is named after the nearest known place.
10. "Use My Location" uses real GPS if you allow it **and** you're within 25 km of the clinic. Otherwise it uses a demo spot in Kommady, Alappuzha.
11. No login screen. A sample patient (Rahul Varghese) pre-fills the form, and whatever you type is remembered for the next booking.
12. Mobile number must be a 10-digit Indian mobile (starts with 6–9).
13. Cancelling is free at any time. Online (mock) payments show as "Refunded (mock)".
14. Payment choices: demo UPI, demo card, or pay at clinic.
15. One completed past visit is pre-loaded so the "Past" list isn't empty.

## Built with

- Plain HTML/CSS/JavaScript, iOS dark-mode colours, the same structure as `park4u`
- [Leaflet 1.9.4](https://leafletjs.com) from cdnjs + © OpenStreetMap contributors (tiles darkened with a CSS filter)
- Inter font from Google Fonts (SF Pro on Apple devices)

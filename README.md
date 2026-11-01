# 🎂 Bakers Gallery — Online Food Ordering Website

A mobile-first, fully animated ordering website for **Bakers Gallery** (SNR Shopping Centre, Indapur, Pune 413106). Customers browse the menu, add items to a cart, enter their details, **verify their phone with an OTP**, choose **online or cash** payment, and the **full order + customer details are delivered to the café owner on WhatsApp**.

---

## ✨ Features

- 🎨 **Full café vibe** — warm bakery palette, animated preloader (coffee cup), rising steam, floating emoji, film grain, smooth scrolling, scroll-reveal, hover/tilt cards, ripple buttons, fly-to-cart, confetti on success.
- 🍰 **Bakery menu** — 14 categories, ~85 items (cakes, pastries, pizza, burgers, cold coffee & more) with ₹ prices and veg/non-veg tags.
- 🛒 **Cart** — add/remove/quantity, persists in the browser, totals update live.
- 📞 **OTP phone verification** — 6-digit OTP (SMS when configured, demo mode otherwise).
- 💳 **Payments** — Cash on Delivery, or **UPI** (Razorpay when configured).
- 💬 **WhatsApp automation** — the moment a customer places an order, the backend sends the full order + customer details to the café owner's WhatsApp automatically (no customer redirect).
- 📱 **Mobile-first**, accessible (`prefers-reduced-motion` respected).

---

## 🚀 Run it

### Option A — Just open it (no setup)
Double-click **`index.html`**. The menu, cart, cash checkout and WhatsApp order all work immediately.
> OTP + online payment fall back to a clearly-labelled **demo mode** (the OTP is shown on screen) until you run the backend with real keys.

### Option B — Full experience (backend for OTP/payments)
Requires **Node.js 18+**. No npm install needed — zero dependencies.

```bash
node server/index.js
```

Then open **http://localhost:3000**.

---

## 🌍 Go live on Render (free)

The site has a Node backend, so it needs a Node host (not static-only hosting). Easiest free option is **Render**.

### 1. Push to GitHub
- Install **Git for Windows**: https://git-scm.com/download/win
- Create a new empty repo on github.com (e.g. `bakers-gallery`, **public**, no README).
- In PowerShell, run:
  ```bash
  cd "C:\Users\rajku\Documents\Bakers"
  git init
  git add -A
  git commit -m "Bakers Gallery website"
  git branch -M main
  git remote add origin https://github.com/YOUR_USERNAME/bakers-gallery.git
  git push -u origin main
  ```
  (`server/.env` and `server/orders.jsonl` are git-ignored, so your secrets stay local.)

### 2. Deploy on Render
1. Go to **render.com** → **Sign in with GitHub**.
2. **New + → Web Service** → connect GitHub → select the `bakers-gallery` repo.
3. Settings:
   - **Build command:** *(leave empty)*
   - **Start command:** `node server/index.js`
4. Add **Environment Variables**:
   ```
   WHATSAPP_PHONE_ID = 1384258484773339
   WHATSAPP_TOKEN    = <your access token>
   WHATSAPP_OWNER    = 917972385812
   ```
5. **Create Web Service** → wait for the deploy → open the live URL.

> ⚠️ Free tier: the app **sleeps after ~15 min of inactivity** (first visit may take ~30–60s to wake). The `orders.jsonl` file doesn't persist between deploys — your **WhatsApp** is the source of truth. For 24/7 + persistent orders, upgrade to a paid plan (and later move orders to a database).

---

## 🔧 Go live (optional)

Copy `server/.env.example` to `server/.env` and fill in:

| Service | What you get | Keys needed |
|---|---|---|
| **MSG91** (SMS) | Real OTP sent to the customer's phone | `MSG91_AUTH_KEY`, `MSG91_TEMPLATE_ID` |
| **Razorpay** | Full online payment (UPI/cards/netbanking) | `RAZORPAY_KEY_ID`, `RAZORPAY_KEY_SECRET` |
| **UPI fallback** | Zero-setup UPI payment via deep-link | `UPI_ID` |
| **WhatsApp Cloud API** | Auto-send each order to the owner's WhatsApp | `WHATSAPP_TOKEN`, `WHATSAPP_PHONE_ID`, `WHATSAPP_OWNER` |

The server loads these from environment variables, so you can also export them directly instead of a `.env` file.

> **WhatsApp automation setup:** create a free Meta developer app → add *WhatsApp* → copy the **permanent access token** and **phone-number ID**, and set `WHATSAPP_OWNER` to the number that should receive orders (international format, e.g. `919503819919`).
>
> **⚠️ Sandbox mode:** a new Meta app can only message numbers that are in its *allowed list*. In the Meta developer console → *WhatsApp* → *API Setup* → **Recipient phone numbers** → **Add phone number**, add the number that will receive orders and verify it (a code is sent to that WhatsApp). After that, orders arrive automatically. To message *any* number (full production), complete business verification and set an approved `WHATSAPP_TEMPLATE`.

---

## ✏️ Edit cafe details & menu

- **Cafe info** (name, phone, address, hours, WhatsApp number, UPI ID) → `js/config.js`
- **Menu items & prices** → `js/data.js`

Orders placed while the backend is running are appended to `server/orders.jsonl`.

---

## 📁 Structure

```
index.html        Home (animated hero, features, favourites, gallery)
menu.html         Full menu (search, category chips, steppers)
checkout.html     Details → OTP → Payment → Confirmation
css/styles.css    Theme + all animations
js/config.js      Cafe info
js/data.js        Menu data
js/cart.js        Cart logic
js/menu.js        Menu rendering
js/checkout.js    Checkout wizard
js/common.js      Helpers + motion
server/index.js   OTP / payment / order-log backend (zero-dep)
assets/           Logo, dish photos, gallery
```

---

## ⚠️ Notes

- The **UPI ID** `7972385812@upi` is a placeholder — replace it with the café's real UPI ID in `js/config.js` (and `server/.env.example`).
- The **phone number** `+91 79723 85812` is the order-receiving number — edit it in `js/config.js` if it changes.
- The current **logo** (`assets/logo.svg`) is a placeholder badge — drop in the café's official logo (as `assets/logo.png` or `logo.svg`) anytime. The menu prices are representative and editable in `js/data.js`.
- **Orders are auto-notified to the owner via the WhatsApp Cloud API** (no customer redirect). Until `WHATSAPP_TOKEN`/`WHATSAPP_PHONE_ID` are set, orders are still logged to `server/orders.jsonl` and the customer sees a "please call to confirm" note.

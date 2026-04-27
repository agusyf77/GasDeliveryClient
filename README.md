# 🛒 Gas Delivery — Client App

> **Customer-facing** Android application of the **Gas Delivery Vlora** system. Customers can sign up, browse gas products by category, build a cart, place orders, and track order status in real time.

[![Platform](https://img.shields.io/badge/platform-Android-3DDC84)]()
[![Language](https://img.shields.io/badge/language-Java-orange)]()
[![Firebase](https://img.shields.io/badge/backend-Firebase-yellow)]()
[![SQLite](https://img.shields.io/badge/local-SQLite-blue)]()
[![Course](https://img.shields.io/badge/course-HCI-purple)]()

---

## 📌 Overview

This is the **customer-side companion** to the [GasDeliveryServer](https://github.com/denisvreshtazi/GasDeliveryServer) app. Together they form a small two-app delivery system designed during a Human-Computer Interaction course, using **Vlorë (Vlora), Albania** as the case study:

- **Customers** order gas through `GasDeliveryClient` (this repo)
- **Workers** receive and fulfill orders through `GasDeliveryServer`

This app handles registration, login, browsing, cart management, order placement, and real-time order status updates.

![logo](logo.png)

## 🛠️ Tech Stack

- **Android Studio** (Java)
- **Firebase Realtime Database** — accounts, products, orders
- **SQLite** — local cart cache (per-user)
- **Material Design** components
- **Drawer.io** — wireframes & UI mock-ups

## 🧱 Architecture

```
┌────────────────────────────────────────────────┐
│            GasDeliveryClient (Customer)        │
│                                                │
│  MainActivity                                  │
│       │                                        │
│       ├─▶ SignUp                               │
│       │                                        │
│       └─▶ SignIn ─▶ Home (categories)          │
│                       │                        │
│                       ├─▶ ProductList          │
│                       │       │                │
│                       │       └─▶ Add to Cart  │
│                       │           (SQLite)     │
│                       │                        │
│                       ├─▶ Cart  ─▶ Place Order │
│                       │              (Firebase)│
│                       │                        │
│                       └─▶ OrderStatus          │
└────────────────────────────────────────────────┘
                    ▲
                    │ Firebase Realtime DB
                    ▼
┌────────────────────────────────────────────────┐
│            GasDeliveryServer (Worker)          │
└────────────────────────────────────────────────┘
```

## 🗂️ Project Structure

### Activities
Located at `/app/src/main/java/com/example/gasdelivery/`

| Activity | Layout | Description |
|---|---|---|
| `MainActivity.java` | `layout/activity_main.xml` | Landing page — Sign Up or Sign In choice. |
| `SignUp.java` | `layout/activity_sign_up.xml` | Register a new customer in Firebase. |
| `SignIn.java` (typo `SignIp` in README) | `layout/activity_sign_in.xml` | Login. On success → `Home`. |
| `Home.java` | `layout/activity_home.xml` | Lists gas product categories from Firebase. Drawer nav + floating cart button. Categories rendered via `CategoryViewHolder` (`layout/menu_item.xml`). |
| `ProductList.java` | `layout/activity_product_list.xml` | All products of the selected category. Each product rendered via `ProductViewHolder` (`layout/product_item.xml`). "Add to cart" → SQL insert. |
| `Cart.java` | `layout/activity_cart.xml` | Local cart contents (loaded from SQLite, not Firebase). Items via `CartAdapter` (`layout/cart_layout.xml`). "Place Order" opens a dialog (`layout/order_fill_time_address.xml`); confirmation pushes the order to Firebase. |
| `OrderStatus.java` | `layout/order_adapter.xml` | List of the customer's orders, each rendered via `OrderViewHolder`. |

### Models (`com.example.gasdelivery.model`)
- **`Order`** — order properties
- **`User`** — user properties
- **`Request`** — request properties
- **`Category`** — category properties
- **`Product`** — product properties

### ViewHolders / Adapters
- **`CartAdapter`** — cart items
- **`CategoryViewHolder`** — category card
- **`ProductViewHolder`** — product card
- **`OrderViewHolder`** — order summary

### Common
- **`Common.java`** — holds the currently logged-in customer for use across activities.

### Database
- **`Database.java`** — local SQLite DB for cart contents. Each user has a unique cart, **filtered by the user's phone number** (`UsersPhone`).

### Resources

| Path | Contents |
|---|---|
| `app/src/main/res/layout/` | All activity & item XML layouts |
| `app/src/main/res/layout/menu/` | Navigation drawer layouts |
| `app/src/main/res/drawable/` | Icons & images |

### Documents
- `Needefinding.pdf` — needfinding study and personas
- `client test.pdf` — client-side usability tests
- `worker test.pdf` — worker-side usability tests
- `HCI_Vreshtazi.pdf` — full coursework report

## 🚀 Setup & Run

### Prerequisites
- **Android Studio** (Arctic Fox or newer recommended)
- **Android SDK** matching the project's `compileSdkVersion`
- A **Firebase project** with the Realtime Database enabled

### Steps

1. Clone the repo:
   ```bash
   git clone https://github.com/denisvreshtazi/GasDeliveryClient.git
   ```
2. Open the project in **Android Studio** and let Gradle sync.
3. Add your **`google-services.json`** under `app/` (download from your Firebase console).
4. Connect a device or start an emulator.
5. **Run** ▶️.

## 🛒 Cart & Ordering Flow

1. Login with your phone number
2. Browse categories on the home screen
3. Tap a category → see all its products
4. **Add to Cart** → product is inserted into the local SQLite DB, scoped to your phone
5. Open the cart from the floating button → review and adjust
6. Tap **Place Order** → fill in delivery time and address in the dialog
7. Confirmation → the order is uploaded to Firebase, where the worker app picks it up

## 🔗 Related Project

➡️ **Worker-side app:** [GasDeliveryServer](https://github.com/denisvreshtazi/GasDeliveryServer)

## 👤 Author

**Denis Vreshtazi** — [GitHub](https://github.com/denisvreshtazi)

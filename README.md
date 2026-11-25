# Junior - Le Terrible - Client (Android)

![Kotlin](https://img.shields.io/badge/Kotlin-2.0-7F52FF?logo=kotlin&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-Material%203-4285F4?logo=jetpackcompose&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-MVVM-4CAF50)
![Authentication](https://img.shields.io/badge/Auth-Required%20for%20Purchase-orange)
![Payment](https://img.shields.io/badge/Payment-Stripe%20Test%20Mode-635BFF?logo=stripe&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Progress-orange)

Android e-commerce application for **Junior - Le Terrible** manga bookshop.

Customers can browse manga series, search and filter the catalogue, 
add volumes to their cart, complete secure payments and track their orders.

The application is developed with **Kotlin**, **Jetpack Compose**, **Material 3** and the **MVVM architecture**.

> **System:** `WPF Admin` manages the store → `Android Client` sells to customers → `SQL Server` is the shared database.

---
## 🏗️ System Architecture

Junior - Le Terrible e-commerce system is composed of:

- **Android Client:** customer-facing mobile application;
- **WPF Admin:** catalogue, customer, stock and order management;
- **C# REST API:** authentication, catalogue, cart, payment and order operations;
- **SQL Server:** persistent data storage;
- **Stripe:** secure test payments.

```text
Android Client
      │
      ▼
C# REST API
      │
      ▼
SQL Server
```
> The Android application never connects directly to SQL Server.
> All database operations are handled by the C# REST API.

The WPF administration application is responsible for managing the shop, while the Android application allows customers to browse the catalogue and place orders.

---

## 📱 Screenshots

> Screenshots will be added after first UI build.

| Home | Detail | Cart | Login |
| :---: | :---: | :---: | :---: |
| *Coming soon* | *Coming soon* | *Coming soon* | *Coming soon* |

---
## ✨ Features

### 🚧 Implementation in progress

- [ ] Display manga with cover, series, volume, author, price and stock
- [ ] Search by series or author
- [ ] Filter by category: Shonen, Seinen, Shojo and Manhwa
- [ ] Filter by publisher: Glénat, Kana, Pika and Ki-oon
- [ ] Display stock availability
- [ ] Add manga to the cart
- [ ] Display a live cart counter

### 🔐 Authentication in progress

- [ ] Login with email and password
- [ ] Register a new customer account
- [ ] Require authentication before adding an item to the cart
- [ ] Require authentication for checkout and order history
- [ ] Persist the authenticated session with DataStore
- [ ] Log out and clear the local session

### 📋 Planned

- [ ] Manga details screen
- [ ] Display all volumes from the same series
- [ ] Display related manga
- [ ] Cart quantity management
- [ ] Remove items from the cart
- [ ] Calculate the total cart price
- [ ] Select or edit the delivery address
- [ ] Stripe test payment
- [ ] Order history
- [ ] Order details
- [ ] Order tracking
- [ ] Favorites and wishlist
- [ ] Customer profile
- [ ] Password recovery
- [ ] Push notifications
- [ ] Dark mode
- [ ] Offline cache with Room
- [ ] Connection to the C# REST API with Retrofit

---

## 🔐 Authentication Rules

Visitors can browse and search the catalogue without creating an account.

Authentication is required before adding a product to the cart, completing a payment or viewing previous orders.

| Action | Visitor | Authenticated customer |
|---|:---:|:---:|
| Browse the catalogue | ✅ | ✅ |
| Search and filter manga | ✅ | ✅ |
| View manga details | ✅ | ✅ |
| Add to cart | ❌ Login required | ✅ |
| Place an order | ❌ Login required | ✅ |
| Pay with Stripe | ❌ Login required | ✅ |
| View previous orders | ❌ Login required | ✅ |

### Why is authentication required?

Authentication is required to:

1. associate an order with a customer;
2. retrieve the customer's delivery address;
3. prevent unauthorized stock modifications;
4. associate the Stripe transaction with the correct order;
5. allow customers to view and track their orders.

**Authentication Flow:**

```text
Home
  │
  ▼
Customer clicks "Add to cart"
  │
  ▼
Is the customer authenticated?
  │
  ├── No ──► Login
  │           │
  │           ├── Existing account ──► Authentication
  │           │
  │           └── No account ──► Registration
  │                                  │
  │                                  ▼
  │                            Account created
  │                                  │
  └──────────────────────────────────┘
                  │
                  ▼
            Return to Home
                  │
                  ▼
           Add item to cart
```

---

## 🧪 Demo Account

Use the following account for local development and testing:

```text
Email: test@test.com
Password: 1234
```

> ⚠️ This account is intended for local development only.
> Never use these credentials in a production environment.

---

## 🛠️ Tech Stack

| Area | Technology |
|---|---|
| Language | Kotlin 2.0 |
| User interface | Jetpack Compose and Material 3 |
| Architecture | MVVM |
| Navigation | Navigation Compose |
| Session persistence | DataStore |
| Local cache | Room, planned |
| Network communication | Retrofit, planned |
| Backend | C# REST API |
| Database | SQL Server |
| Payment | Stripe SDK, test mode |
| Image loading | Coil |
| Version control | Git and GitHub |

---

## 📦 Project Structure

```text
com.jlt/
├── MainActivity.kt
├── navigation/
│   └── AppNavigation.kt
├── ui/
│   ├── home/
│   │   └── HomeScreen.kt
│   ├── auth/
│   │   ├── LoginScreen.kt
│   │   └── RegisterScreen.kt
│   ├── detail/
│   │   └── DetailScreen.kt
│   ├── cart/
│   │   └── CartScreen.kt
│   ├── checkout/
│   │   └── CheckoutScreen.kt
│   ├── orders/
│   │   ├── OrdersScreen.kt
│   │   └── OrderDetailScreen.kt
│   ├── profile/
│   │   └── ProfileScreen.kt
│   └── theme/
│       ├── Color.kt
│       ├── Theme.kt
│       └── Type.kt
├── data/
│   ├── model/
│   │   ├── Manga.kt
│   │   ├── Client.kt
│   │   ├── CartItem.kt
│   │   └── Order.kt
│   ├── repository/
│   │   ├── MangaRepository.kt
│   │   ├── AuthRepository.kt
│   │   ├── CartRepository.kt
│   │   └── OrderRepository.kt
│   ├── local/
│   │   └── SessionManager.kt
│   └── AuthViewModel.kt
    ├── CartViewModel.kt
    └── OrderViewModel.kt
```

> The exact project structure may change as new features are implemented.

---

## 🗄️ Database Model

The application uses a SQL Server database managed through the C# REST API.

The main database entities are:

- `Clients`
- `Mangas`
- `Orders`
- `OrderItems`
- `Payments`

### Clients

| Field | SQL type | Description | Example |
|---|---|---|---|
| Id | INT | Customer identifier | `1` |
| FirstName | NVARCHAR | Customer's first name | `Junior` |
| LastName | NVARCHAR | Customer's last name | `Le Terrible` |
| Email | NVARCHAR UNIQUE | Unique login email | `juniorleterrible@test.be` |
| PasswordHash | NVARCHAR | Hashed password | `********` |
| Address | NVARCHAR | Delivery address | `Rue des Mangas 23, Liège` |

### Mangas

| Field | SQL type | Description | Example |
|---|---|---|---|
| Id | INT | Manga identifier | `1` |
| Series | NVARCHAR | Manga series | `One Piece` |
| Volume | INT | Volume number | `105` |
| Title | NVARCHAR | Volume title | `One Piece - Volume 105` |
| Author | NVARCHAR | Manga author | `Eiichiro Oda` |
| Price | DECIMAL(10,2) | Unit price | `9.99` |
| Stock | INT | Available quantity | `25` |
| Category | NVARCHAR | Manga category | `Shonen` |
| Publisher | NVARCHAR | Manga publisher | `Glénat` |
| Summary | NVARCHAR | Manga description | `The adventure continues...` |
| ImageUrl | NVARCHAR | Manga cover URL | `https://example.com/one-piece-105.jpg` |

### Orders

| Field | SQL type | Description | Example |
|---|---|---|---|
| Id | INT | Order identifier | `1001` |
| ClientId | INT | Customer identifier | `1` |
| OrderDate | DATETIME | Order creation date | `2026-11-25 14:30:00` |
| TotalAmount | DECIMAL(10,2) | Total order amount | `29.97` |
| DeliveryAddress | NVARCHAR | Delivery address | `Rue des Mangas 23, Liège` |
| Status | NVARCHAR | Current order status | `Preparing` |
| PaymentStatus | NVARCHAR | Current payment status | `Paid` |

### OrderItems

| Field | SQL type | Description | Example |
|---|---|---|---|
| Id | INT | Order item identifier | `1` |
| OrderId | INT | Associated order | `1001` |
| MangaId | INT | Associated manga | `1` |
| Quantity | INT | Ordered quantity | `3` |
| UnitPrice | DECIMAL(10,2) | Price at purchase time | `9.99` |

### Payments

| Field | SQL type | Description | Example |
|---|---|---|---|
| Id | INT | Payment identifier | `1` |
| OrderId | INT | Associated order | `1001` |
| TransactionReference | NVARCHAR | Stripe transaction reference | `pi_test_123456` |
| Amount | DECIMAL(10,2) | Paid amount | `29.97` |
| Status | NVARCHAR | Payment status | `Succeeded` |
| PaymentDate | DATETIME | Payment date | `2026-11-25 14:35:00` |

The complete SQL schema is available in:

```text
jlt-database/Database/jlt_db.sql
```

---

## 🚀 Installation

### Requirements

Before running the project, install:

- Android Studio Hedgehog or newer
- JDK 17
- Android SDK
- Android Emulator with API 34 or a physical Android device
- Git
- Access to the C# REST API

### Clone the repository

```bash
git clone https://github.com/Millet-Lab/jlt-client-android.git
cd jlt-client-android
```

### Open the project

1. Open Android Studio.
2. Select **Open**.
3. Select the `jlt-client-android` directory.
4. Wait for the Gradle synchronization to finish.
5. Configure the API base URL.
6. Start an Android emulator or connect a physical device.
7. Run the application.

---

## ⚙️ API Configuration

The Android application communicates with the C# REST API through Retrofit.

Configure the API base URL in the appropriate configuration file:

```kotlin
const val BASE_URL = "https://your-api-url/"
```

For an API running locally on the development computer, use the following address from the Android emulator:

```kotlin
const val BASE_URL = "http://10.0.2.2:5000/"
```

> `10.0.2.2` allows the Android emulator to access the host computer's `localhost`.

For a physical Android device, use the local IP address of the computer running the API:

```kotlin
const val BASE_URL = "http://192.168.1.100:5000/"
```

The computer and the Android device must be connected to the same local network.

> Do not hard-code production secrets, database credentials or private Stripe keys in the Android application.

---

## 💳 Stripe Payment

Stripe is used in **test mode** during development.

The payment flow is:

```text
Android Client
      │
      ▼
C# REST API
      │
      ▼
Stripe
      │
      ▼
Payment confirmation
      │
      ▼
Order validation
```

The Android application must never contain the Stripe secret key.

- The Android client uses the Stripe publishable key.
- The C# API uses the Stripe secret key.
- PaymentIntent creation is handled by the C# API.
- Card numbers are handled by Stripe and are never stored in SQL Server.

---

## 🔒 Security

The project follows these security rules:

- The Android application never connects directly to SQL Server.
- Database credentials are stored only on the backend.
- Passwords are hashed by the C# API.
- Passwords are never stored in plain text.
- Card numbers are never stored by the application.
- Payments are handled by Stripe.
- Private Stripe keys are stored only on the backend.
- Secrets and API keys are not committed to Git.
- Production communication must use HTTPS.
- The API validates the authenticated customer before creating an order.
- Product prices and stock are verified by the API before payment.
- The API uses database transactions when creating orders and updating stock.

Files containing secrets should be excluded through `.gitignore`, for example:

```gitignore
local.properties
.env
secrets.properties
appsettings.Development.json
```

---

## 🛣️ Roadmap

### Version 0.1 - Catalogue

- [ ] Home screen
- [ ] Manga catalogue
- [ ] Search
- [ ] Category filters
- [ ] Publisher filters
- [ ] Stock display

### Version 0.2 - Authentication

- [ ] Registration
- [ ] Login
- [ ] Session persistence
- [ ] Logout
- [ ] Authentication guards

### Version 0.3 - Shopping cart

- [ ] Add items to the cart
- [ ] Update quantities
- [ ] Remove items
- [ ] Calculate the total price
- [ ] Validate stock availability

### Version 0.4 - Orders and payment

- [ ] Checkout
- [ ] Delivery address
- [ ] Stripe test payment
- [ ] Order creation
- [ ] Stock update

### Version 0.5 - Customer account

- [ ] Order history
- [ ] Order tracking
- [ ] Customer profile
- [ ] Favorites
- [ ] Password recovery

### Version 1.0

- [ ] Stable C# API integration
- [ ] Error handling
- [ ] Offline cache
- [ ] Dark mode
- [ ] Push notifications
- [ ] Automated tests
- [ ] Production-ready release

---

## 🤝 Related Repositories

The complete Junior - Le Terrible system is divided into several repositories:

- `jlt-client-android`: Android customer application
- `jlt-admin-wpf`: WPF administration application
- `jlt-api`: C# REST API
- `jlt-database`: SQL Server database scripts

---

## 📄 License

This project was created for educational purposes.

It may be used, modified and shared for learning purposes.

# 🍽️ Flavor Fusion

<div align="center">

![Project](https://img.shields.io/badge/Flavor_Fusion-Ecommerce_Store-blue?style=for-the-badge)
![PHP](https://img.shields.io/badge/PHP-8.2-777BB4?style=for-the-badge&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-MariaDB-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Apache](https://img.shields.io/badge/Apache-HTTP_Server-2C2C2C?style=for-the-badge&logo=apache&logoColor=white)

### A Full-Featured **Snacks & Food Marketplace** Built with PHP and MySQL

[Getting Started](#-getting-started) · [Project Structure](#-project-structure) · [Database](#-database) · [Troubleshooting](#-troubleshooting) · [Contributing](#-development-workflow)

</div>

---

## 📖 About The Project

Flavor Fusion is a **complete web-based e-commerce marketplace** for premium snacks and food products. It is a full-stack PHP application that lets customers browse, search, and order products online while giving administrators a dashboard to manage products, categories, staff, orders, customers, and payments.

The application follows a classic **frontend + backend + database** architecture:

- **Frontend (`public/`)** — HTML, CSS, and JavaScript served by the browser. It displays products, categories, cart, checkout, user profile, and store pages.
- **Backend (`init.php` + `database/` + `public/admin/`)** — PHP application logic that handles routing, authentication, database operations, cart management, order processing, and the admin panel.
- **Database (MySQL / MariaDB)** — Stores users, products, categories, carts, orders, order items, payments, reviews, and stock.

### How users interact with it

1. A visitor lands on the **homepage**, browses **categories**, and views **featured products**.
2. Products can be searched, filtered by category, and paged through.
3. Each product has a detail page with reviews and a rating.
4. Logged-in users can add products to a **cart**, review their **cart**, and checkout.
5. Checkout creates an **order** and records **payment/billing** information.
6. The **admin panel** lets an administrator manage every part of the store: products, categories, staff, orders, customers, payments, and settings.

### Main purpose

- Sell food/snacks online with a searchable product catalog and product detail pages.
- Manage the catalog (products, categories) and transactions (orders, payments) through an admin dashboard.
- Provide registered users with accounts, profiles, addresses, and order history.

---

## ✨ Features

### Public / Customer Features

| Feature | What it does | How a user uses it | Handled by |
|---|---|---|---|
| **Browse categories** | Shows product categories with images on the homepage | Click any category card to filter the shop | `public/index.php`, `public/shop/index.php` |
| **Browse products** | Displays products with price, rating, and image (paginated, 8 per page) | Visit `/shop` | `public/shop/index.php` |
| **Search products** | Search by product name or description | Enter a term in the navbar search box | `public/shop/index.php` (uses `Connection::search`) |
| **Filter by category** | Filter products by category with an `INNER JOIN` on `categories` | Click a category card | `public/shop/index.php` (`Connection::inner_join`) |
| **Product detail page** | Shows one product's details, quantity, existing reviews, and rating | Click a product "View Detail" button | `public/shop/view/index.php` |
| **Reviews & ratings** | Reviews with a 5-star display and average rating | View each product's reviews on the detail page | `Connection::find` for `reviews` |
| **Shopping cart** | Add, update, delete items; shows subtotal, delivery charge, and total | Visit `/cart` | `public/cart/index.php` |
| **Cart validation** | Maximum 5 items per cart; delete when quantity is 0 | Update quantity in cart or delete | `public/cart/index.php` |
| **Checkout** | Enter/use address; choose delivery; computes delivery fee (₹40 if subtotal < ₹500) and displays total | Visit `/checkout` | `public/checkout/index.php` |
| **User account / profile** | View and update name, email, and phone | Visit `/profile` | `public/profile/index.php` |
| **User registration** | Create an account (name, email, password) with a default profile picture and `customer` role | Visit `/register` | `public/register/index.php` |
| **User login / logout** | Sign in with email + password (password hashed via `password_hash`); logout clears the session | Visit `/login` | `public/login/index.php`, `public/profile/logout/index.php` |
| **Password reset** | Request a reset link (token generated with `random_bytes`), email sent, reset password with token | Visit `/forgot-password` then `/reset-password?token=` | `public/forgot-password/index.php`, `public/reset-password/index.php`, `sendEmail` in `init.php` |
| **Protected routes** | Restricted pages (cart, checkout, profile) redirect to login when unauthenticated | Visits restricted page without login | `init.php` (`enable_protected_route`) |
| **Graceful page header/footer** | Consistent layout across all pages | Rendered automatically by `includes/header.php` and `includes/footer.php` | includes |

### Admin / Store Management Features

| Feature | What it does | How an admin uses it | Handled by |
|---|---|---|---|
| **Admin login** | Login with an admin/creator account (email + password verified with `password_verify`) | Visit `/admin/login` | `public/admin/login/index.php` |
| **Dashboard** | Shows key counts: total payments, product count, order count, customer count | Visit `/admin` | `public/admin/index.php` |
| **Manage products** | List, add, edit, view, delete products | Visit `/admin/products` | `public/admin/products/index.php` |
| **Manage categories** | List, add, edit, view, delete categories | Visit `/admin/categories` | `public/admin/categories/index.php` |
| **Manage staff** | Add staff members with name, email, phone, and password | Visit `/admin/staff/new` | `public/admin/staff/new/index.php` |
| **Manage orders** | List and view orders | Visit `/admin/orders` | `public/admin/orders/index.php` |
| **View customers** | List registered customers | Visit `/admin/customers` | `public/admin/customers/index.php` |
| **Payment & billing** | View completed payments | Visit `/admin/payments` | `public/admin/payments/index.php` |
| **Settings** | Manage site settings | Visit `/admin/settings` | `public/admin/settings/index.php` |
| **Admin route protection** | Only admins can access `/admin/*` pages | Any non-admin or unauthenticated user is redirected to `/admin/login` | `init.php` (`enable_admin_route`) |

---

## 🛠️ Technology Stack

### Frontend

- **HTML5** — page structure
- **CSS (Bootstrap 5 + custom `style.css`)** — layout, styling, and UI components
- **JavaScript (Bootstrap JS bundle + custom `index.js`)** — interactive UI elements (toast auto-hide, cart/quantity feedback)
- **Font Awesome 6** (via CDN) — icons
- **Google Fonts (Poppins, Roboto)** (via CDN) — typography

### Backend

- **PHP 8.2+** — server-side scripting and application logic
- **Superglobals (`$_GET`, `$_POST`, `$_SERVER`, `$_SESSION`)** — request and session handling
- **Built-in helpers in `init.php`** — `parse_input`, `redirect`, `show_alert`, `displayStars`, `getAverageRating`, `login`, `logout`, `get_auth_user`, `enable_unprotected_route`, `enable_protected_route`, `enable_admin_route`, `sendEmail`

### Database

- **MySQL / MariaDB** — relational database
- **PDO (PHP Data Objects)** — database access layer in `Connection.php`
- **SQL (DDL/DML)** — schema declared in `flavor_fusion_db.sql` and `query.sql`

### Server / Development Environment

- **XAMPP** — Apache + MySQL + PHP on Windows
- **Apache 2.4+** — web server (with `.htaccess` URL rewriting)
- **Git** — version control
- **phpMyAdmin** — database management (for importing SQL)

---

## 📂 Project Structure

```
flavor_fusion/
│
├── .gitignore
├── .htaccess
├── database/
│   ├── Connection.php      # PDO database access class
│   └── database.php        # DB credentials + PDO connection instance
├── includes/
│   ├── admin-footer.php    # Admin panel closing HTML/scripts
│   ├── admin-sidebar.php   # Admin dashboard sidebar (navigation)
│   ├── footer.php          # Page footer + social links
│   ├── header.php          # HTML head + opening body tag
│   ├── nav.php             # Top bar + navigation bar + main tag
│   └── user_sidebar.php    # User sidebar (reusable)
├── public/
│   ├── admin/              # Admin panel controllers + views
│   ├── assets/
│   │   ├── css/            # Bootstrap + custom styles
│   │   ├── images/         # Product, category, hero images
│   │   └── js/             # Bootstrap JS + app JS
│   ├── cart/               # Shopping cart page
│   ├── checkout/           # Order / checkout page
│   ├── forgot-password/    # Password reset request page
│   ├── index.php           # Public homepage entry point
│   ├── login/              # User login page
│   ├── profile/            # User profile + address + orders
│   ├── register/           # User registration page
│   ├── reset-password/     # Password reset page
│   ├── shop/               # Product catalog + shop listing + view
│   └── .htaccess           # Apache URL rewriting for public/
├── .htaccess               # Apache routing for the /flavor_fusion/ path
├── flavor_fusion_db.sql    # Complete database schema (phpMyAdmin dump)
├── init.php                # App bootstrap: constants, helpers, auth/se
└── README.md
```

> The repository currently keeps the application under the root `public/`, `includes/`, `database/`, `init.php`, and `flavor_fusion_db.sql` files. The earlier `flavor_fusion/` copy has been removed.

---

## 📁 Folder Explanation

### `public/`

The **only publicly accessible directory**. Apache's `.htaccess` rewrites all requests so that frontend pages are reached through here.

- `index.php` — **homepage entry point** (fetches categories + featured products, displays hero, categories, featured products).
- `shop/` — product catalog listing, category filtering, search, pagination.
- `shop/view/` — product detail page with reviews/rating.
- `cart/` — cart listing, quantity updates, item deletion.
- `checkout/` — order creation with delivery/payment logic.
- `login/` / `register/` / `forgot-password/` / `reset-password/` — auth pages.
- `profile/` — user profile, address, orders.
- `admin/` — admin panel controllers and views.
- `assets/` — front-end assets served to the browser:
  - `css/` — `bootstrap.min.css` + custom `style.css`
  - `images/` — product/category/hero images
  - `js/` — `bootstrap.bundle.min.js` + `index.js`

### `includes/`

Reusable HTML/PHP components included by the public and admin pages.

- `header.php` — HTML `<head>` + opening `<body>` tag.
- `nav.php` — top bar + navigation bar; shows login/register or account/cart links depending on session.
- `footer.php` — closing `</main>`, footer, social links, and scripts.
- `admin-sidebar.php` — admin dashboard sidebar navigation.
- `admin-footer.php` — admin panel closing HTML/scripts.
- `user_sidebar.php` — reusable user sidebar (kept in the project).

### `database/`

- `Connection.php` — a **PDO wrapper class** (`Connection`) with methods:
  `save`, `find`, `findOne`, `findById`, `findAll`, `update`, `delete`, `search`, `paginate`, `count`, `inner_join`.
- `database.php` — loads `Connection.php`, defines MySQL credentials (`$servername`, `$port`, `$db_name`, `$username`, `$password`), and creates a single `$connection` object used by every page.

### `init.php`

The **application bootstrap**. It is included at the top of every PHP page and provides:

- **Constants:** `PROJECT_NAME`, `ASSETS_PATH`, `ROOT`, `DB_ROOT`
- **Helper functions:** `displayStars`, `getAverageRating`, `parse_input`, `redirect`, `show_alert`, `login`, `logout`, `get_auth_user`, `enable_unprotected_route`, `enable_protected_route`, `enable_admin_route`, `sendEmail`
- **Session handling** (`session_start()`)
- **Authentication routes:** public, protected (logged-in), and admin-only routes

### `public/.htaccess`

Apache URL rewriting. It redirects `/public/some-page` requests to the actual page and rewrites all other requests into `public/`, so the app is accessed through `/flavor_fusion/`.

### Root `.htaccess`

Same rewrite rules as `public/.htaccess` — required so that requests to the site root (`/flavor_fusion/`) are routed into `public/`.

### Root `init.php` / `query.sql` / `flavor_fusion_db.sql`

- `init.php` — the application bootstrap (defines constants, helpers, and sessions).
- `query.sql` — SQL statements for creating the database, tables, and initial data.
- `flavor_fusion_db.sql` — a full phpMyAdmin dump of the `flavor_fusion_db` database.

---

## 🗄️ Database

### Database name

`flavor_fusion_db` (created and used by both `query.sql` and `flavor_fusion_db.sql`).

### Tables

| Table | Purpose |
|---|---|
| `users` | Customers, staff, and admins. Stores name, email, password (hashed), phone, profile picture, `role` (customer/admin/staff), and a reset token |
| `addresses` | One or more addresses per user (shipping address for checkout) |
| `categories` | Product categories (name, description, image) |
| `products` | Products (name, description, category, price, stock, image) |
| `cart` | Cart items per user (product, user, quantity) |
| `orders` | Customer orders (user, total, delivery option) |
| `order_items` | Items inside each order |
| `payments` | Payment/billing records (with completed/failed status) |
| `reviews` | Product reviews and ratings |
| `stock` | Stock quantity per product |

### Important columns & keys

- **`users`**: primary key `id`; unique `email`; `role` ENUM(`customer`, `admin`, `staff`), default `customer`; `password` VARCHAR(255) stores **hashed** password via `password_hash`; `token` stores a password-reset token.
- **`addresses`**: `id`; `user_id` (foreign key → `users.id`, `ON DELETE CASCADE`).
- **`categories`**: primary key `id`; unique `name`.
- **`products`**: primary key `id`; `category_id` foreign key → `categories.id`, `ON DELETE CASCADE`; `price` DECIMAL(10,2); `stock` INT.
- **`cart`**: cart items — `product_id`, `user_id`, `quantity`.
- **`orders`**: fields include `user_id`, `total_amount`, `delivery_option`.
- **`order_items`**: items for each order.
- **`payments`**: `status` (`completed` used by the dashboard), `amount`.
- **`reviews`**: reviews per product with a `rating`.
- **`stock`**: per-product quantity and last-updated timestamp.

### Relationships

- `addresses.user_id → users.id` (ON DELETE CASCADE)
- `products.category_id → categories.id` (ON DELETE CASCADE)
- Other joins happen in PHP, especially `products` ↔ `categories` via `Connection::inner_join()` in `public/shop/index.php`.

### How PHP communicates with MySQL

Every page includes `database/database.php`, which builds a **PDO** instance:

```php
$pdo = new PDO("mysql:host={$server_name};port={$port};dbname={$db_name}", $username, $password);
```

`database.php` then creates a `Connection` object and assigns it to a global `$connection` variable. Pages use `$connection->find()`, `findAll()`, `save()`, `update()`, `delete()`, `search()`, `paginate()`, `count()`, and `inner_join()` to run SQL. All queries use **prepared statements** (bound parameters), which protect against SQL injection.

### SQL files and when to use them

- **`query.sql`** — creates the database (`flavor_fusion_db`), selects it, and defines all tables.
- **`flavor_fusion_db.sql`** — a phpMyAdmin export of the same `flavor_fusion_db` database. Import this in phpMyAdmin for the fastest setup.

Both produce the same schema; importing either is sufficient.

---

## 💻 Requirements

| Requirement | Version / notes |
|---|---|
| **Windows** | Latest, with WSL2 optional |
| **XAMPP** | Apache, MySQL/MariaDB, PHP (recommended 8.2+) |
| **Apache** | Apache 2.4+ (XAMPP ships this) |
| **MySQL / MariaDB** | MySQL 5.7+ or MariaDB 10.4+ |
| **PHP** | 8.2+ is recommended (the project uses PHP 8.2 syntax) |
| **Browser** | Chrome / Edge / Firefox (modern, HTML5 + CSS3) |
| **Git** | For cloning and pushing updates |
| **phpMyAdmin** (via XAMPP) | For importing the SQL database |

---

## 🚀 Installation

### 📥 Clone The Repository

```bash
git clone https://github.com/Tirth-Shrimali/flavor_fusion.git
cd flavor_fusion
```

### 📁 Place the project inside `htdocs`

Open your XAMPP installation folder (e.g., `C:\xampp`), open the `htdocs` directory, and paste the `flavor_fusion` folder there:

```
C:\xampp\htdocs\flavor_fusion\
```

> After this, the project is reachable at `http://localhost/flavor_fusion/`.

### 🔌 Start Apache

1. Open the XAMPP Control Panel.
2. Start **Apache**.
3. Verify Apache is running (green checkmark + port 80 shown).

---

## 🗄️ Database Setup

1. Open a browser and go to **http://localhost/phpmyadmin**.
2. Click **New** in the left sidebar.
3. In the *Create database* field, type: **`flavor_fusion_db`**
4. Set *Collation* to `utf8mb4_general_ci` (or `utf8mb4` default) and click **Create**.
5. Click the newly created **`flavor_fusion_db`** database in the left sidebar.
6. Click the **Import** tab.
7. Click **Choose a file** and select **`flavor_fusion_db.sql`** (from the project folder).
8. Click **Go**.
9. Wait for the import to complete. The page will show **0 rows affected** / success.
10. Click the **Structure** tab to confirm all tables were created:
    - `users`, `addresses`, `categories`, `products`, `cart`, `orders`, `order_items`, `payments`, `reviews`, `stock`

---

## ⚙️ Configuration

All database configuration lives in **`database/database.php`**.

```php
$servername = 'localhost';
$port = 3306;
$db_name = 'flavor_fusion_db';
$username = 'root';
$password = '';
```

### Change these values if needed

| Config | Description |
|---|---|
| `servername` | MySQL host (default: `localhost`) |
| `port` | MySQL port (default: `3306`) |
| `db_name` | The database name (default: `flavor_fusion_db`) |
| `username` | MySQL user (default: `root`) |
| `password` | MySQL password (default: empty for XAMPP) |

> XAMPP ships with a `root` user and an empty password, so the default values work out of the box. If you created a custom MySQL user or password on your machine, change these values to match.

---

## ▶️ Running The Project

1. Keep **Apache** and **MySQL** running in the XAMPP Control Panel.
2. Open a browser and go to:

```
http://localhost/flavor_fusion/
```

3. The **homepage** opens first. It shows the hero, browse categories, and featured products.
4. Sign up / log in to browse the shop, cart, checkout, and profile.
5. Use `/admin/login` to access the admin panel.

---

## 🔄 How The Project Works

### Architecture

```text
User
 ↓
Browser
 ↓
Apache (web server, .htaccess routing)
 ↓
PHP (public/index.php entry point)
 ↓
  init.php (application bootstrap: constants, helpers, auth, sessions)
 ↓
MySQL (database)
```

### Request flow in detail

1. User opens `http://localhost/flavor_fusion/`.
2. Apache receives the request and applies `.htaccess` rewriting so the request reaches `public/index.php`.
3. `public/index.php` includes `../init.php` (application bootstrap) and `database/database.php` (PDO connection).
4. The bootstrap defines constants (`PROJECT_NAME`, `ASSETS_PATH`, `ROOT`, `DB_ROOT`) and starts the session.
5. Application logic runs: e.g., fetch categories and products with `$connection->findAll()` / `$connection->paginate()`.
6. MySQL returns the result sets.
7. PHP processes the data (e.g., calculate average ratings, paginate).
8. PHP renders HTML (via `includes/header.php`, `includes/nav.php`, and page templates).
9. The browser displays the page.

### Authentication flow

1. On registration/login, the password is hashed with `password_hash()` and stored in `users.password`.
2. On login, the entered password is compared with `password_verify()`.
3. A successful login stores the user record in `$_SESSION["user"]` via `login($user)`.
4. `get_auth_user()` reads the session.
5. `enable_protected_route()` redirects non-logged-in users to `/login`.
6. `enable_admin_route()` redirects non-admins to `/admin/login`.

---

## 🔐 Authentication / Authorization

Yes — the project includes full authentication and authorization.

- **Registration** (`public/register/index.php`): creates a user with a hashed password and `role = "customer"`.
- **Login** (`public/login/index.php`): validates the user and starts the session.
- **Logout** (`public/profile/logout/index.php`): clears the session.
- **Password reset** (`public/forgot-password/index.php` + `public/reset-password/index.php`): generates a 64-hex token, updates `users.token`, emails a reset link, then resets the password and clears the token.
- **Admin authorization** (`init.php`): `enable_admin_route()` redirects any non-admin or non-logged-in user to `/admin/login`.

Session-backed users are stored as `$_SESSION["user"]` and are available to the navigation through `get_auth_user()`.

---

## 🍽️ Main Application Flow

```text
Homepage (public/index.php)
  ↓
Browse categories (categories table)
  ↓
Browse products (products + categories tables)
  ↓
Search / filter / paginate
  ↓
Product detail (shop/view)
  ↓
User adds to cart (cart table)
  ↓
Checkout (checkout + orders + payments + addresses)
  ↓
Admin panel (products, categories, staff, orders, customers, payments)
  ↓
Result: a working online snack marketplace
```

---

## 🧩 Important Components

### `init.php`

- **What it does:** application bootstrap — defines path constants, session handling, auth helpers, and input/redirect/alert functions.
- **What calls it:** every PHP page in `public/`, `public/admin/`, etc.
- **Depends on:** nothing (it is the first included file).
- **Returns:** configures the app for the rest of the request.

### `database/Connection.php`

- **What it does:** a PDO wrapper class that runs all database queries (SELECT, INSERT, UPDATE, DELETE, search, paginate, count, inner join).
- **What calls it:** `database/database.php` creates the PDO instance and exposes it as `$connection`.
- **Depends on:** PHP PDO + a live MySQL/MariaDB database.
- **Returns:** database rows as associative arrays.

### `public/index.php`

- **What it does:** the public homepage. Fetches categories and featured products.
- **Depends on:** `init.php` (constants) + `database/database.php` (connection) + `includes/header.php` / `nav.php` / `footer.php`.

### `public/shop/index.php`

- **What it does:** product catalog with search, category filter, and pagination.
- **Depends on:** `init.php`, `database/database.php`, `includes/header.php`, `includes/nav.php`.

### `public/cart/index.php`

- **What it does:** cart listing, subtotal, delivery charge, total, add/update/delete cart items.
- **Depends on:** `init.php`, `database/database.php`, `enable_protected_route()` (requires login).

### `public/admin/index.php`

- **What it does:** admin dashboard, computes payment totals, product count, order count, and customer count.
- **Depends on:** `init.php`, `database/database.php`, `enable_admin_route()`.

---

## 🧪 Testing / Verification

After installation, verify each step:

| Check | How |
|---|---|
| Apache is running | Open `http://localhost` in a browser (XAMPP dashboard shows green) |
| MySQL is running | Open `http://localhost/phpmyadmin` in a browser |
| Database exists | In phpMyAdmin, confirm `flavor_fusion_db` database is present |
| Tables imported | In phpMyAdmin, confirm `users`, `categories`, `products`, `cart`, `orders`, `order_items`, `payments`, `reviews`, `stock`, `addresses` exist |
| Application opens | Visit `http://localhost/flavor_fusion/` and see the homepage |
| Database connection works | Visit the homepage — products and categories should render |
| Auth works | Register → login → access cart/profile → logout |
| Admin panel works | Register a user with email/role admin or login as admin → visit `/admin` |

---

## 🛑 Troubleshooting

### Apache not starting

- Check **XAMPP Control Panel → Apache** details; a common cause is another application using port 80 (Skype, IIS, another Apache instance).
- Try changing Apache's Port in **XAMPP → Actions → Config → Apache (httpd.conf)** (e.g., port 8080) and access `http://localhost:8080/flavor_fusion/`.

### MySQL not starting

- Open **XAMPP → Shell** and run `mysql -u root` to test the MySQL server.
- Check the **MySQL error log** in the XAMPP panel.

### Database connection error

- The most common cause is wrong credentials in **`database/database.php`**.
- Ensure the database exists, the user has permission, and the port is correct.
- For default XAMPP, `username = "root"` and `password = ""` (empty).

### 404 / Page Not Found

- Confirm the project is in **`C:\xampp\htdocs\flavor_fusion\`**.
- Confirm **Apache** is running and the URL is exactly:
  ```
  http://localhost/flavor_fusion/
  ```
- Check the **`.htaccess`** rewrite rules if URLs are not rewriting properly.

### PHP error

- With XAMPP, enable **Display errors** in **php.ini** (look for `display_errors = On`) and check the **Apache error log** under **xampp/apache/logs/error.log** or **xampp/php/logs/php_error_log**.

### SQL / database errors

- Re-import `flavor_fusion_db.sql` in phpMyAdmin after each deploy if tables are missing.
- Verify table names and column names exactly match the dump.
- Confirm the `users` table has enough rows for admin login (e.g., an admin account with a hashed password).

---

## 🔄 Updating The Project

To update a local clone with the latest changes from GitHub:

```bash
cd flavor_fusion
git pull origin main
```

> If changes conflict, review the conflict markers, resolve any file edits, then commit and push your changes as usual.

---

## 📤 Development Workflow

1. **Check status** — always run `git status` before committing to see what will be staged.
2. **Stage** — `git add .` (or specific files).
3. **Commit** — `git commit -m "your message"`.
4. **Push** — `git push origin main`.

> Never push passwords, API keys, tokens, `.env` secrets, or local configuration files. The `.gitignore` file is set up to block those.

---

## 🔒 Security

Never commit the following to GitHub:

- **Passwords** and credentials
- **API keys** and tokens
- **Secrets** in `.env` files
- **Private configuration** containing real values
- Local environment files or sensitive configuration

The project includes a `.gitignore` that blocks `node_modules/`, `.env` files, `*.log`, cache/cache/tmp, OS files, and AI-tool session metadata (`.freebuff/`). Always review `git status` before committing.

---

## 👨‍💻 Author

**Tirth Shrimali**  
GitHub: [https://github.com/Tirth-Shrimali](https://github.com/Tirth-Shrimali)  
Project: [https://github.com/Tirth-Shrimali/flavor_fusion](https://github.com/Tirth-Shrimali/flavor_fusion)

---

## ⭐ Project

If you found this project helpful, consider giving it a ⭐ on GitHub. Contributions, issues, and feature requests are welcome!

# ReadVice

> Find your next book.

ReadVice is a book discovery and recommendation website. It lets readers search an
extensive book database, filter titles by genre, save books to a personal list,
react to books they love, and leave comments — all wrapped in a social experience
that helps like-minded readers connect around what they read.

The dataset used in this project comes from
[goodreads_bbe_dataset](https://github.com/scostap/goodreads_bbe_dataset/tree/main).

![Home page](https://github.com/DYagmur/ReadVice/assets/30656517/a39c82e6-9570-4037-a728-2f455bf44dc0)
![Book page](https://github.com/DYagmur/ReadVice/assets/30656517/743cbc30-bd7a-4638-861f-5185223cd84b)

---

## Features

| Feature | Description |
| --- | --- |
| **Search** | Search books, authors or genres. |
| **Filter** | Filter the catalogue by clicking a genre. |
| **Like** | Rate a book positively with the *"I love this book"* button. |
| **Add to list** | Save a book to a personal list shown on your profile. |
| **Comments** | Leave comments on a book, displayed with username and date. |
| **Pagination** | Book results are paginated for a cleaner browsing experience. |
| **Contact** | Send an email through the contact form. |

---

## Tech stack

- **PHP 8** (plain PHP, no framework, no Composer dependencies)
- **MySQL / MariaDB** accessed through PDO (`pdo_mysql`)
- **HTML + SCSS/CSS** (compiled stylesheet in `css/`)

### Project structure

```
readvice/
├── index.php            # Home / catalogue
├── login.php            # Login
├── signup.php           # Registration
├── logout.php
├── bookInfo.php         # Single book page
├── userList.php         # Current user's saved list
├── about.php
├── contact.php          # Contact form (uses PHP mail())
├── css/                 # style.scss and compiled style.css
├── img/
└── inc/
    ├── config.inc.php   # Database credentials
    ├── Page.class.php / PageContent.class.php
    ├── BookPage.class.php
    ├── Entities/        # Book, User, UserComment, UserList
    └── Utilities/
        ├── PDOService.class.php
        ├── LoginManager.class.php
        ├── DAO/         # BookDAO, UserDAO, UserCommentDAO, UserListDAO
        └── Repositories/BookRepository.class.php
```

---

## Getting started

### Prerequisites

- PHP **8.0 or newer** with the `pdo_mysql` extension enabled
- MySQL or MariaDB (**XAMPP** bundles both, and its PHP already has `pdo_mysql`)

### 1. Get the code

```bash
git clone https://github.com/pssgarcia/readvice.git
cd readvice
```

### 2. Create the database and import the data

Start MySQL (e.g. from the XAMPP Control Panel), then:

```bash
# create the database
mysql -u root -e "CREATE DATABASE IF NOT EXISTS bookstest"

# import the schema and data
mysql -u root bookstest < inc/data/bookstest.sql
```

On Windows with XAMPP, use the bundled client:

```powershell
& C:\xampp\mysql\bin\mysql.exe -u root -e "CREATE DATABASE IF NOT EXISTS bookstest"
& C:\xampp\mysql\bin\mysql.exe -u root bookstest -e "source inc/data/bookstest.sql"
```

Alternatively, open **phpMyAdmin** (`http://localhost/phpmyadmin`), create a database
named `bookstest`, and import `inc/data/bookstest.sql` from the *Import* tab.

### 3. Configure the connection

Database credentials live in `inc/config.inc.php`. The defaults match a stock XAMPP
install:

```php
define("DB_USER", "root");
define("DB_PASS", "");
define("DB_HOST", "localhost");
define("DB_NAME", "bookstest");
```

Adjust `DB_USER` / `DB_PASS` if your MySQL uses different credentials.

### 4. Run the app

**Option A — PHP built-in server (recommended for local dev)**

Run from the project root. On Windows use the XAMPP PHP binary so `pdo_mysql` is
available:

```powershell
# Windows / XAMPP
& C:\xampp\php\php.exe -S localhost:8000
```

```bash
# macOS / Linux (or if a suitable php is on your PATH)
php -S localhost:8000
```

Then open **http://localhost:8000/index.php**.

> XAMPP's PHP may print harmless `pdo_firebird` / `pdo_oci` warnings on startup —
> ignore them. The server is up once you see
> `PHP ... Development Server (http://localhost:8000) started`.

**Option B — Apache (XAMPP htdocs)**

Copy or symlink the project into `C:\xampp\htdocs\readvice`, start Apache and MySQL
from the XAMPP Control Panel, and open **http://localhost/readvice/index.php**.

---

## Troubleshooting

| Symptom | Cause / fix |
| --- | --- |
| `Fatal error: Call to a member function prepare() on null` in `PDOService.class.php` | The database connection failed and the error was swallowed. Check that MySQL is running, the `bookstest` database exists and is imported, and the credentials in `inc/config.inc.php` are correct. |
| `could not find driver` | The PHP you are running doesn't have `pdo_mysql`. Use `C:\xampp\php\php.exe`, or enable `extension=pdo_mysql` (and `extension_dir = "ext"`) in your `php.ini`. |
| Blank page / `This site can't be reached` | The dev server isn't running or is on another port. Re-run the command from step 4 and confirm the `Development Server ... started` line. |
| Contact form doesn't send email | `contact.php` uses PHP's `mail()`, which needs a configured mail transport (SMTP / sendmail). It typically won't work on a bare local setup. |

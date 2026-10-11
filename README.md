<<<<<<< HEAD
# Course Review Portal — Web Security Demo (Part 2)

## Use case
A simple student course-review portal: users log in, search courses, post reviews,
and update their profile/email. Chosen because it naturally needs a login form,
a search field, user-generated content (reviews), and a state-changing action
(email update) — covering every required vulnerability class.

## Tech stack
- Node.js + Express
- EJS templates
- SQLite3 (file-based DB, no separate DB server needed)
- express-session for login sessions
- csurf (safe version only) for CSRF tokens

## Folder structure
```
vulnerable/   -> deliberately insecure version (port 3000)
safe/         -> hardened version, same features (port 3001)
seed-data.sql -> reference schema + seed rows
csrf-attack.html -> standalone page used to demonstrate the CSRF attack
```

## How to run

### Vulnerable version
```
cd vulnerable
npm install
node server.js
```
Open http://localhost:3000

### Safe version
```
cd safe
npm install
node server.js
```
Open http://localhost:3001

Both can run at the same time (different ports, different DB files).

## Vulnerabilities implemented (vulnerable/) and how they're fixed (safe/)

| Vulnerability | Where (vulnerable) | Payload example | Fix (safe) |
|---|---|---|---|
| Reflected XSS | `/search?q=` | `<script>alert('reflected')</script>` | EJS `<%= %>` auto-escaping |
| Stored XSS | Review comment on `/course/:name` | `<script>alert('stored')</script>` | EJS `<%= %>` auto-escaping on output |
| DOM-based XSS | `/profile#...` via `innerHTML` | `<img src=x onerror=alert('dom')>` | `textContent` instead of `innerHTML` |
| CSRF | `/update-email` (no token, no SameSite) | `csrf-attack.html` auto-submits a form | `csurf` token + `SameSite=strict` cookie |
| SQL Injection — Easy | `/login` (string concat) | `admin' OR '1'='1' --` | Parameterized query (`?` placeholders) |
| SQL Injection — Intermediate | `/check-user?username=` (blind boolean) | `admin' AND '1'='1` vs `admin' AND '1'='2` | Parameterized query |
| SQL Injection — Advanced | `/search?q=` (UNION-based) | `' UNION SELECT username, password FROM users--` | Parameterized query |

## Notes for the viva
- Passwords are stored in plaintext in both versions for demo clarity; in a real
  production app they should be hashed with bcrypt — mention this limitation if asked.
- The vulnerable app logs every raw SQL query to the console (`[SQL QUERY]`) so you
  can show the examiner exactly what gets executed.
=======
# Web-Security_Course-review-portal
>>>>>>> bada4d9c0b1c96fc38343a9112d534a1818ef835

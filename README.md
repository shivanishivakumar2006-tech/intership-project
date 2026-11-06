# Elevance Community Platform (Django + MySQL)

One Django project with six apps covering every internship task:

| App | Covers |
|---|---|
| `accounts` | Register, login (email/username), logout, **Forgot Password** (email or phone, once per day, letters-only generated password), profile |
| `security` | **Advanced Login Security**: login history (browser, OS, device, IP, location, time), session list + remote revoke, new-device email, OTP for unrecognised devices, trusted devices, inactivity expiry (configurable per user), admin login logs, hashed session keys + browser-binding against session reuse |
| `community` | **Community Feed**: posts (update/image/code/showcase/achievement), like, comment, reply, share, bookmark, edit, delete, follow, trending, hashtags, mentions, notifications, reports; admin moderation (remove content, suspend repeat offenders); server-side pagination + infinite scroll |
| `reputation` | **Reputation system** + Q&A: +5 answer, +10 accepted, +5 at 5 upvotes, +2 question with 10 upvotes, +10 profile bonus, -2 downvote, -5 own delete, -10 admin removal; privileges at 50/100/250/500; transfers (only if > 50 points) |
| `subscriptions` | **Subscription & Premium**: plans, test-mode checkout, auto-activation, unique invoice no., PDF invoice, confirmation email, billing dashboard, premium perks |
| `core` | **Multi-language** (EN, ES, HI, PT, ZH, FR). Switching to French = OTP to email; any other language = OTP to mobile (SMS) |

## Run locally
```bash
python -m venv venv && source venv/bin/activate     # Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py seed            # plans + admin/admin: Admin12345 + demo users alice/bob: Demo12345
python manage.py runserver
```
Open http://127.0.0.1:8000. OTPs/emails print in the terminal (and on-screen while `DEMO_SHOW_SECRETS=1`).

## Use MySQL
```sql
CREATE DATABASE elevance CHARACTER SET utf8mb4;
```
```bash
pip install mysqlclient
export MYSQL_NAME=elevance MYSQL_USER=root MYSQL_PASSWORD=yourpass   # Windows: set VAR=value
python manage.py migrate && python manage.py seed
```

## Tests
`python manage.py test`  (10 tests covering all modules)

## Deploy (Render / PythonAnywhere / Railway)
Push to GitHub, create a Web Service, build `pip install -r requirements.txt`, start with the `Procfile` command,
and set the variables from `.env.example`. Add a MySQL database (or leave SQLite for the demo).

## Notes / assumptions
* Payment gateway is a **mock in test mode** (`subscriptions/services.py::charge`) - swap in Stripe/Razorpay for real payments.
* SMS is a console mock unless Twilio env vars are set.
* Transfer rule text was cut off in the task screenshot - implemented as: sender needs > 50 points, max `TRANSFER_MAX_POINTS` (50) per transfer. Edit in `settings.py`.
* Set `DEMO_SHOW_SECRETS=0` for production so OTPs are not displayed on screen.

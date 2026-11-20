# Thaagam Foundation, Chennai – NGO Field Impact & Operations Management System

Python · Django 5 · MySQL · HTML5/CSS3 · Bootstrap 5 · Chart.js

## 1. Folder structure
```
thaagam_ngo/
├── manage.py
├── requirements.txt
├── config/                 settings.py (MySQL config), urls.py, wsgi/asgi
├── core/                   shared: role permissions, generic CRUD views, bootstrap forms, tests, seed_demo command
├── accounts/               login/logout, Profile (role), system-user management
├── dashboard/              dashboard + Chart.js data
├── beneficiaries/          Beneficiary CRUD + search/filter
├── volunteers/             Volunteer CRUD + availability
├── activities/             Activity CRUD (Planned / Ongoing / Completed) + detail page
├── assignments/            Volunteer ↔ Activity assignment + status
├── attendance/             Bulk Present/Absent marking + records
├── distribution/           Quantity, beneficiary count, proof photo upload
├── reports/                4 reports, Excel (openpyxl) + PDF (reportlab) export
├── templates/              base layout, generic list/form/detail, per-module pages
├── static/css/style.css
├── media/                  uploaded proof photos
└── database/               setup.sql (create DB/user), schema.sql (full MySQL schema)
```

## 2. Install (Windows / VS Code)
```bat
:: Python 3.10+ and MySQL 8 must be installed
cd thaagam_ngo
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```
Linux/macOS: `python3 -m venv venv && source venv/bin/activate && pip install -r requirements.txt`
(Linux needs `sudo apt install libmysqlclient-dev pkg-config build-essential` before mysqlclient.)

## 3. MySQL database
```bash
mysql -u root -p < database/setup.sql
```
Creates DB `thaagam_db` and user `thaagam_user` / `Thaagam@123`.
Different credentials? Set env vars: `DB_NAME DB_USER DB_PASSWORD DB_HOST DB_PORT` (or edit `config/settings.py`).

## 4. Tables, demo data, run
```bash
python manage.py migrate
python manage.py seed_demo          # users + 20 beneficiaries, 12 volunteers, 16 activities, attendance, distributions with photos
python manage.py runserver
```
Open http://127.0.0.1:8000/  (internet needed once for Bootstrap/Chart.js CDN)
`python manage.py seed_demo --reset` reloads demo data. `python manage.py test` runs the 12 automated tests.

## 5. Demo logins
| User | Password | Role | Can do |
|---|---|---|---|
| admin | admin123 | Admin | Everything incl. delete + System Users |
| coordinator | coord123 | Coordinator | Add/edit all modules, reports; no delete |
| staff | staff123 | Field Staff | View all; mark attendance; record distribution + photos |

## 6. Suggested demo flow (10 min)
Login → Dashboard → add Beneficiary → register Volunteer → create Activity → Assign volunteers →
Mark attendance → Record distribution with photo → Activity detail page → Reports → export Excel/PDF →
login as `staff` to show role restrictions.

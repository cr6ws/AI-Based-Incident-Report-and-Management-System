# AI-Based Incident Report and Management System

A Laravel-based incident management and reporting platform that combines real-time incident intake, role-based access control, AI-assisted severity prediction, and dispatch workflows for emergency response teams.

This project is designed to help administrators and responders monitor incidents from multiple sources (mobile reports and CCTV feeds), assign responsibilities, track status, and generate detailed incident reports.

## Overview

The system provides a central workflow for:

- receiving and organizing incident reports
- classifying severity with an AI model
- assigning incidents to responders
- tracking incident progress and resolution
- viewing historical incident logs
- generating printable PDF incident reports

## Key Features

### 1. Incident intake and monitoring
- Supports incident entries from mobile and CCTV sources
- Stores incidents in Firebase Realtime Database
- Displays incident data in live management tables and dashboards
- Tracks current incidents versus resolved cases

### 2. AI-based severity prediction
- Uses a Python Flask API to predict incident severity based on incident description
- Loads a trained machine learning model and vectorizer from joblib files
- Adds severity and priority metadata when a report is created

### 3. Role-based access control
- Admin users can manage users, review incidents, dispatch responders, and generate reports
- Responder users can access assigned incident details and historical activity
- Route protection ensures unauthorized role access is redirected or blocked

### 4. Dispatch and responder workflow
- Admins can view and dispatch incidents to assigned responders
- Responders are redirected to their dedicated incident pages
- Incident assignment logic is handled via dispatch records

### 5. Incident history and logs
- Keeps a resolved incidents archive
- Provides incident logs for review of prior cases
- Supports movement from active incidents to resolved records

### 6. Reporting and documentation
- Generates single-incident and bulk incident reports
- Produces downloadable PDF-style reports for operational documentation
- Helps document incident timeline, description, assignment, and status

## Technology Stack

- Backend: Laravel 12 (PHP 8.2)
- Frontend: Blade, Livewire, Tailwind CSS, Vite
- Authentication: Laravel Jetstream + Sanctum
- Database: MySQL (Laravel local database) and Firebase Realtime Database
- AI/ML: Python, Flask, scikit-learn, joblib, pandas
- Reporting: PDF generation via Laravel controller workflow and browser rendering
- Notifications: Laravel notifications and email integration

## Project Structure

```text
.
├── app/
│   ├── Http/Controllers/
│   ├── Livewire/
│   ├── Models/
│   ├── Notifications/
│   ├── Services/
│   └── View/
├── config/
├── database/
├── public/
├── resources/
│   ├── views/
│   └── css/
├── routes/
├── storage/
├── tests/
├── .env.example
├── composer.json
├── package.json
├── requirements.txt
├── predict_severity_api.py
├── train_severity_classifier.py
├── generate_incidents.py
├── severity_classifier.joblib
├── severity_vectorizer.joblib
├── severity_cat_columns.joblib
├── artisan
├── vite.config.js
├── tailwind.config.js
└── README.md
```

## Installation

### Prerequisites

- PHP 8.2+
- Composer
- Node.js and npm
- MySQL or another supported Laravel database
- Python 3.x
- Firebase service account credentials

### 1. Clone the repository

```bash
git clone https://github.com/cr6ws/AI-Based-Incident-Report-and-Management-System.git
cd AI-Based-Incident-Report-and-Management-System
```

### 2. Install PHP dependencies

```bash
composer install
```

### 3. Install frontend dependencies

```bash
npm install
```

### 4. Configure environment variables

Copy the example environment file:

```bash
cp .env.example .env
```

Update `.env` with your local configuration:

- database credentials (`DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`)
- application URL
- Firebase configuration values

Example database settings:

```dotenv
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=capstone_project
DB_USERNAME=root
DB_PASSWORD=
```

The project also expects Firebase configuration, which is referenced in `config/firebase.php`:

```php
'credentials' => storage_path('app/firebase_credentials.json'),
'database_url' => env('FIREBASE_DATABASE_URL', 'https://your-database.firebaseio.com'),
```

Create the Firebase service account file at:

```text
storage/app/firebase_credentials.json
```

and add the Firebase Realtime Database URL in `.env`:

```dotenv
FIREBASE_DATABASE_URL="https://your-project-default-rtdb.firebaseio.com"
```

### 5. Generate Laravel app key

```bash
php artisan key:generate
```

### 6. Run database migrations

```bash
php artisan migrate
```

### 7. Start the Python severity prediction API

The AI model is served by Flask and expects to be run from the project root so the model files can be found:

```bash
python predict_severity_api.py
```

This starts a Flask service on port `5000` and exposes the `/predict-severity` endpoint.

### 8. Run the Laravel app

In one terminal:

```bash
php artisan serve
```

In another terminal:

```bash
npm run dev
```

You can also run both together with:

```bash
composer run dev
```

## Running the Application

Once the app is running:

- open the web app in your browser
- create an admin account via the admin creation page
- log in and access the dashboard
- manage incidents and responders from the admin interface

### Admin creation route

```text
/admin-create
```

### Recovery route

```text
/admin-recover
```

## Core Workflows

### Admin workflow
- Access dashboard summary and overview metrics
- Review all active incidents
- View mobile and CCTV incident tables
- Dispatch incidents to responders
- Generate incident reports
- Archive resolved incidents and review logs

### Responder workflow
- View assigned incidents from the responder panel
- Open detailed incident information
- Review incident history and status
- Update operational awareness of assigned work

## AI Severity Model

The app includes a trained classification pipeline:

- `severity_classifier.joblib`
- `severity_vectorizer.joblib`
- `severity_cat_columns.joblib`

The Python API loads these files and predicts a severity label from an incident description. The prediction is sent from Laravel through an HTTP call to:

```text
http://127.0.0.1:5000/predict-severity
```

## Extra Scripts

The repository includes utility scripts for model generation and sample data:

- `train_severity_classifier.py` - trains the severity model
- `generate_incidents.py` - generates sample incidents
- `admin-control-panel.bat` - Windows helper for launch/admin control tasks
- `start-vite-dev.bat` - helper to launch Vite in development

## Notes

- The project uses Firebase for operational incident storage while Laravel handles app authentication, dashboard logic, routing, and report generation.
- The Python service must remain running for AI severity prediction to work properly.
- `requirements.txt` contains the Python dependencies needed by the prediction service.

## License

This project is distributed under the MIT license, consistent with the Laravel project template.

## Contributing

Contributions are welcome. Please open a pull request or discuss improvements before making large changes.

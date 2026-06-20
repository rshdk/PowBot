# PowBot

A web-based PowerShell administration framework with real-time client management.

## Features

### Dashboard

* Real-time client monitoring
* Interactive command terminal
* Client geolocation detection
* Multi-client command execution
* Clean dark-themed interface

### Script Builder

* PowerShell one-liner generator
* Configurable execution policies
* Adjustable check-in intervals
* Optional persistence support

### Security

* HTTPS/TLS support
* User authentication
* GitHub OAuth integration
* Secure session management

### Performance

* Gevent-based asynchronous server
* Long-polling command delivery
* Automatic inactive client cleanup
* SQLite-backed storage

## Installation

### Prerequisites

* Python 3.10+
* PowerShell 5.1+ (Windows) or PowerShell 7+
* OpenSSL (optional, for HTTPS deployment)

### Quick Start

Clone the repository:

```bash
git clone https://github.com/rshdk/PowBot.git
cd PowBot
```

Create and activate a virtual environment:

```bash
python -m venv venv

# Windows
.\venv\Scripts\activate

# Linux/macOS
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the application:

```bash
python runLocal.py
```

Open:

```text
http://127.0.0.1:5000
```

Create an account through the registration page.

## Production Deployment

Generate SSL certificates:

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout private.key -out certificate.crt
```

Start the production server:

```bash
python runWeb.py
```

Default ports:

* Web Interface: 443
* API Server: 4000

## Usage

### Generate a Client Script

1. Open the Settings page.
2. Configure:

   * Target server URL
   * Execution policy
   * Check-in interval
   * Persistence options
3. Generate the PowerShell command.
4. Deploy the generated command to authorized systems.

### Execute Commands

1. Open the Dashboard.
2. Select one or more connected clients.
3. Enter a PowerShell command.
4. Submit the command.
5. View the returned output.

### Example Commands

```powershell
Get-ComputerInfo | Select-Object CsName, OsName, OsVersion

Get-Process | Select-Object -First 10 Name, CPU, WorkingSet

Get-NetTCPConnection | Where-Object State -eq 'Established'

Get-ChildItem -Path C:\ -Force
```

## Project Structure

```text
PowBot/
├── apps/
│   ├── authentication/
│   ├── home/
│   │   └── master/
│   │       └── stub/
│   ├── static/
│   └── templates/
├── Powroute.py
├── runLocal.py
├── runWeb.py
└── requirements.txt
```

## Architecture

```text
Client Agent
     │
     │ HTTPS
     ▼
API Server (Powroute)
     │
     │ Internal Communication
     ▼
Flask Web Panel
     │
     ▼
Administrator Browser
```

## Configuration

### Environment Variables

| Variable                | Description                |
| ----------------------- | -------------------------- |
| SECRET_KEY              | Flask session secret       |
| SQLALCHEMY_DATABASE_URI | Database connection string |
| ASSETS_ROOT             | Static asset path          |

### Client Parameters

| Parameter      | Description        |
| -------------- | ------------------ |
| URL            | Server endpoint    |
| CHECK_INTERVAL | Polling interval   |
| PERSISTENCE    | Enable persistence |

## Dependencies

### Core

* Flask
* Flask-SQLAlchemy
* Flask-Login
* Flask-WTF
* Flask-CORS
* Gevent

### Authentication

* Flask-Dance
* email-validator

### Additional

* Flask-Minify
* Flask-Migrate

## Disclaimer

This project is intended for authorized security testing, red team operations, and educational purposes only.

Users are responsible for ensuring compliance with applicable laws, regulations, and organizational policies. The authors assume no responsibility for misuse of this software.

## License

Released under the MIT License. See the LICENSE file for details.

## Contributing

Contributions are welcome.

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to your fork
5. Open a pull request

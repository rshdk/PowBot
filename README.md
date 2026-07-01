# PowBot

A web-based PowerShell administration console built with Python and Flask. PowBot provides centralized management, command execution, and real-time status monitoring for Windows endpoints, through a browser-based dashboard.

> **Authorized use only.** PowBot is built for lab environments, coursework, and IT administration on systems you own or are explicitly authorized to manage. Deploying the client agent on any system without the owner's informed consent is prohibited. See [Security Considerations](#security-considerations) below.

## Intended Use

PowBot was built as a learning project to explore:

* Client-server architecture over HTTPS
* Flask web application development
* Authentication, sessions, and access control
* PowerShell automation and remote execution patterns
* Real-time status monitoring with long-polling

It's intended for personal labs, coursework, and authorized internal IT use — not for deployment on any system without the explicit, informed consent of its owner or administrator.

## Features

### Client Management

* Real-time online/offline status tracking
* Client identification and inventory
* Multi-client selection for batch actions
* IP-based location metadata (for inventory/asset tracking in known environments)

### Administration

* Interactive PowerShell command execution
* Command output collection and review
* Multi-client task distribution
* Centralized dashboard for connected endpoints

### Configuration

* Client script generation for known, authorized endpoints
* Configurable execution policy
* Adjustable status update interval
* Optional startup behavior (should default to off — see note below)

### Security

* HTTPS/TLS support
* User authentication with password hashing
* GitHub OAuth integration
* Session management and CSRF protection

### Performance

* Gevent-based asynchronous server
* Long-polling for client status updates
* Automatic cleanup of inactive clients
* Lightweight SQLite storage

## Installation

### Prerequisites

* Python 3.10 or later
* PowerShell 5.1 or later
* OpenSSL (optional, for HTTPS in local development)

### Clone the Repository

```bash
git clone https://github.com/rshdk/PowBot.git
cd PowBot
```

### Create a Virtual Environment

```bash
python -m venv venv
```

Windows:

```bash
.\venv\Scripts\activate
```

Linux/macOS:

```bash
source venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Start the Development Server

```bash
python runLocal.py
```

Open your browser and navigate to:

```text
http://127.0.0.1:5000
```

Create an account through the registration page.

## Production Deployment

### Generate SSL Certificates

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout private.key -out certificate.crt
```

### Start the Production Server

```bash
python runWeb.py
```

Default ports:

| Service       | Port |
| ------------- | ---- |
| Web Interface | 443  |
| API Server    | 4000 |

## Usage

### Client Setup

1. Open the Settings page.
2. Configure:

   * Server URL
   * Execution policy
   * Status update interval
   * Startup behavior
3. Generate the client script.
4. Install it only on systems where you have explicit authorization, with the system owner's knowledge.

### Managing Clients

1. Open the Dashboard.
2. View connected clients.
3. Select one or more systems.
4. Run administrative PowerShell commands.
5. Review command output.

### Example Commands

#### System Information

```powershell
Get-ComputerInfo | Select-Object CsName, OsName, OsVersion
```

#### Running Processes

```powershell
Get-Process | Select-Object -First 10 Name, CPU, WorkingSet
```

#### Network Connections

```powershell
Get-NetTCPConnection | Where-Object State -eq 'Established'
```

#### File System

```powershell
Get-ChildItem -Path C:\ -Force
```

## Project Structure

```text
PowBot/
├── apps/
│   ├── __init__.py
│   ├── config.py
│   ├── authentication/
│   ├── home/
│   │   ├── routes.py
│   │   └── master/
│   │       └── stub/
│   │           ├── PShell.ps1
│   │           └── Pcrypt.ps1
│   ├── static/
│   └── templates/
├── Powroute.py
├── runLocal.py
├── runWeb.py
├── requirements.txt
└── README.md
```

## Architecture

```text
┌─────────────────┐
│ Client Systems  │
└────────┬────────┘
         │ HTTPS
         ▼
┌─────────────────┐
│   API Server    │
│   (Powroute)    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Flask Web Panel │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Administrator   │
│    Browser      │
└─────────────────┘
```

## Configuration

### Environment Variables

| Variable                | Description                |
| ----------------------- | -------------------------- |
| SECRET_KEY              | Flask session secret       |
| SQLALCHEMY_DATABASE_URI | Database connection string |
| ASSETS_ROOT             | Static asset location      |

### Client Parameters

| Parameter      | Description                           |
| -------------- | -------------------------------------- |
| URL            | Server endpoint                        |
| CHECK_INTERVAL | Status update interval                 |
| STARTUP        | Whether the client runs at system startup (default: off; enable only with the device owner's knowledge) |

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

## Security Considerations

* Use HTTPS in production; never run this over plain HTTP outside local dev.
* Only install the client agent on systems you own, or where you have documented, explicit authorization from the owner/administrator.
* The client should be installed visibly (e.g., as a named, discoverable service) so the system owner can identify and remove it — this project should not be used to hide the agent's presence.
* Restrict dashboard access to authorized administrators and rotate credentials regularly.
* Review your organization's IT and security policies before deploying this in any environment beyond a personal lab.
* Misuse of this software to access, monitor, or control systems without authorization may violate computer-crime laws in your jurisdiction.

## Disclaimer

This project was built for educational and authorized administrative use only.

The author assumes no responsibility for misuse, unauthorized deployment, or damages resulting from use of this software. Anyone using PowBot is responsible for complying with all applicable laws, regulations, and organizational policies, and for obtaining explicit authorization before installing the client agent on any system.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Contributing

Contributions are welcome.

1. Fork the repository
2. Create a feature branch

```bash
git checkout -b feature/my-feature
```

3. Commit your changes

```bash
git commit -m "Add my feature"
```

4. Push your branch

```bash
git push origin feature/my-feature
```

5. Open a Pull Request

## Acknowledgments

PowBot was built as a learning and research project exploring remote administration concepts, web application development, and PowerShell automation.

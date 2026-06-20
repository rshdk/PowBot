# PowBot

A web-based PowerShell remote administration framework built with Python and Flask. PowBot provides centralized client management, remote command execution, and real-time monitoring through a modern web interface.

## Intended Use

PowBot is intended for educational purposes, system administration research, lab environments, and authorized security testing.

The project demonstrates concepts including:

* Client-server communication
* Remote administration workflows
* PowerShell automation
* Flask web application development
* Authentication and access control
* HTTPS-based communications
* Real-time client management

Use this software only on systems you own or are explicitly authorized to administer or assess.

## Features

### Client Management

* Real-time client monitoring
* Online/offline status tracking
* Client identification and inventory
* Multi-client selection and management
* IP-based geolocation information

### Remote Administration

* Interactive PowerShell command execution
* Command output collection
* Multi-client task distribution
* Centralized administration dashboard

### Configuration

* Client script generation
* Configurable execution policies
* Adjustable status update intervals
* Optional auto-start configuration
* Customizable deployment settings

### Security

* HTTPS/TLS support
* User authentication
* GitHub OAuth integration
* Password hashing
* Session management
* CSRF protection

### Performance

* Gevent-based asynchronous server
* Long-polling communication
* Automatic inactive client cleanup
* Lightweight SQLite storage

## Installation

### Prerequisites

* Python 3.10 or later
* PowerShell 5.1 or later
* OpenSSL (optional, for HTTPS deployment)

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

### Start Development Server

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

### Start Production Server

```bash
python runWeb.py
```

Default ports:

| Service       | Port |
| ------------- | ---- |
| Web Interface | 443  |
| API Server    | 4000 |

## Usage

### Client Configuration

1. Open the Settings page.
2. Configure:

   * Server URL
   * Execution policy
   * Status update interval
   * Auto-start preferences
3. Generate the client script.
4. Deploy to authorized systems.

### Managing Clients

1. Open the Dashboard.
2. View connected clients.
3. Select one or more systems.
4. Execute administrative PowerShell commands.
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

| Parameter      | Description              |
| -------------- | ------------------------ |
| URL            | Server endpoint          |
| CHECK_INTERVAL | Status update interval   |
| PERSISTENCE    | Auto-start configuration |

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

* Use HTTPS in production environments.
* Restrict access to authorized administrators.
* Rotate credentials regularly.
* Deploy only on systems you own or are authorized to manage.
* Review organizational policies before use.

## Disclaimer

This project is provided for educational, research, and authorized administrative purposes only.

The authors assume no responsibility for misuse, unauthorized deployment, or any damages resulting from the use of this software. Users are responsible for complying with all applicable laws, regulations, and organizational policies.

## License

This project is licensed under the MIT License.

See the LICENSE file for details.

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

PowBot was created as a learning and research project focused on remote administration, web application development, and PowerShell automation.

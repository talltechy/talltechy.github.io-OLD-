# Wiki.JS 2 - config.yml Example

```yml
#######################################################################
# Wiki.js - CONFIGURATION                                             #
#######################################################################
# Full documentation + examples:
# https://docs-beta.requarks.io/install

# ---------------------------------------------------------------------
# Port the server should listen to
# ---------------------------------------------------------------------

port: 4434

# ---------------------------------------------------------------------
# Database
# ---------------------------------------------------------------------
# Supported Database Engines:
# - postgres = PostgreSQL 9.5 or later
# - mysql = MySQL 8.0 or later (5.7.8 partially supported, refer to docs)
# - mariadb = MariaDB 10.2.7 or later
# - mssql = MS SQL Server 2012 or later
# - sqlite = SQLite 3.9 or later

db:
  type: postgres
  # PostgreSQL / MySQL / MariaDB / MS SQL Server only:
  host: PGSQL-EXAMPLE-SERVER
  port: 5432
  user: wikijs_u
  pass: ***REDACTED***
  db: wikijs2
  ssl: true
  # SQLite only:
  #storage: path/to/database.sqlite

#######################################################################
# ADVANCED OPTIONS                                                    #
#######################################################################
# Do not change unless you know what you are doing!

# ---------------------------------------------------------------------
# Use X-Forwarded-For header
# ---------------------------------------------------------------------
# Enable only if Wiki.js is behind a reverse-proxy (nginx, apache, etc)
# or a cloud proxying services like Cloudflare.

trustProxy: true

# ---------------------------------------------------------------------
# SSL/TLS Settings
# ---------------------------------------------------------------------
# Consider using a reverse proxy (e.g. nginx) if you require more
# advanced options than those provided below.

ssl:
  enabled: false

  # Certificate format, either 'pem' or 'pfx':
  format: pem
  # Using PEM format:
  key: /etc/ssl/certs/server.key
  cert: /etc/ssl/certs/certificate.crt
  # Using PFX format:
  pfx: path/to/cert.pfx
  # Passphrase when using encrypted PEM / PFX keys (default: null):
  passphrase: null
  # Diffie Hellman parameters, with key length being greater or equal
  # to 1024 bits (default: null):
  dhparam: null

  # Listen on this HTTP port and redirect all requests to HTTPS.
  # Set to false to disable (default: 80):
  redirectNonSSLPort: false

# ---------------------------------------------------------------------
# Database Pool Options
# ---------------------------------------------------------------------
# Refer to https://github.com/vincit/tarn.js for all possible options

pool:
  # min: 2
  # max: 10

# ---------------------------------------------------------------------
# IP address the server should listen to
# ---------------------------------------------------------------------
# Leave 0.0.0.0 for all interfaces

bindIP: 0.0.0.0

# ---------------------------------------------------------------------
# Log Level
# ---------------------------------------------------------------------
# Possible values: error, warn, info (default), verbose, debug, silly

logLevel: info

# ---------------------------------------------------------------------
# Upload Limits
# ---------------------------------------------------------------------
# If you're using a reverse-proxy in front of Wiki.js, you must also
# change your proxy upload limits!

uploads:
  # Maximum upload size in bytes per file (default: 5242880 (5 MB))
  maxFileSize: 5242880
  # Maximum file uploads per request (default: 20)
  maxFiles: 10

# ---------------------------------------------------------------------
# Offline Mode
# ---------------------------------------------------------------------
# If your server cannot access the internet. Set to true and manually
# download the offline files for sideloading.

offline: false
```

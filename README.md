# Nginx Shallow Dive

This configuration demonstrates a production-ready Nginx setup with advanced security and access control features.

## Overview

The configuration establishes a secure HTTPS server for example.com with HTTP-to-HTTPS redirection, serving files from the `/srv` directory with multiple protection mechanisms.

## Key Features

### Core Configuration
- **User**: Runs as `www-data` for security
- **Worker Processes**: Set to auto for optimal CPU utilization
- **Worker Connections**: 2048 connections per worker
- **PAM Module**: Loads authentication module for system-level authentication

### SSL/HTTPS
- **Port 80**: Redirects all HTTP traffic to HTTPS (301 permanent redirect)
- **Port 443**: Serves content over HTTPS with SSL certificates
- **SSL Certificates**: Uses `/etc/nginx/ssl/server.crt` and `/etc/nginx/ssl/server.key`

### Request Limiting
- **Rate Limiting Zone**: `auth_limit` restricts to 5 requests per minute per IP address
- **Burst Limit**: Allows burst of 3 requests to prevent legitimate traffic blocking

### Protected Locations

#### `/` - Root Location
- Serves static files from `/srv` directory
- Returns 404 for non-existent files

#### `/secure` - IP-Based & Basic Auth
- Allows access from `192.168.1.0/24` and `172.20.10.0/24` networks
- Requires basic authentication via `/etc/nginx/.htpasswd` file
- Uses `satisfy any` (allows either IP match OR auth)
- Rate limited to 5 requests/minute with burst of 3

#### `/user` - User-Level Authentication
- Requires basic authentication via `/etc/nginx/.htpasswd.user`
- Rate limited to 5 requests/minute with burst of 3

#### `/admin` - Admin-Level Authentication
- Requires basic authentication via `/etc/nginx/.htpasswd.admin` (separate password file)
- Rate limited to 5 requests/minute with burst of 3
- Intended for administrative access

#### `/auth-pam` - PAM System Authentication
- Integrates with system PAM (Pluggable Authentication Modules)
- Uses nginx PAM service for authentication
- Provides system-level user authentication

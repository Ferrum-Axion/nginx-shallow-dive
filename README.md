# Nginx Authentication Practice
This configuration demonstrates nginx authentication and access control mechanisms for securing web applications.

# Authentication Methods
- **Basic HTTP Authentication**: Password protection for `/secure`, `/admin`, and `/user` directories using separate password files
- **PAM Authentication**: System-level authentication for `/pam` using Linux user credentials

# Security Features
- **SSL/TLS Encryption**: All traffic redirected to HTTPS (port 443) 
- **IP-Based Access Control**: `/secure` allows local network (192.168.1.0/24) and localhost without authentication, requires password for external IPs

# How It Works
When users access protected directories, nginx checks authentication based on the location:
- `/secure` - Checks IP first, then requires password if from external network
- `/admin` and `/user` - Always require password (from referenced files)
- `/pam` - Uses Linux system user accounts instead of password files

All passwords use bcrypt hashing for security. SSL ensures encrypted communication.
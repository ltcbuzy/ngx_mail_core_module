# NGX_MAIL_CORE_MODULE
ngx_mail_core_module
The **ngx_mail_core_module** in Nginx is a fundamental module designed to handle mail protocols such as SMTP and IMAP. While Nginx is widely recognized for its prowess as an HTTP server, its versatility extends to managing mail-related functionalities through this dedicated module.

Here's a more in-depth exploration of the **ngx_mail_core_module** with code examples:
### Mail Configuration Block:
```
mail {
    # Global mail configuration directives go here
    server {
        # Server-specific directives go here
    }
}
```
The mail block serves as the overarching configuration container for mail-related settings. Within this block, you can define one or more mail servers using the server directive.
### Server Configuration:
```
mail {
    server {
        listen     25;
        protocol   smtp;
        # Additional SMTP configuration directives go here
    }
}
```
## Example Configuration
```
worker_processes auto;

error_log /var/log/nginx/error.log info;

events {
    worker_connections  1024;
}

mail {
    server_name       mail.example.com;
    auth_http         localhost:9000/cgi-bin/nginxauth.cgi;

    imap_capabilities IMAP4rev1 UIDPLUS IDLE LITERAL+ QUOTA;

    pop3_auth         plain apop cram-md5;
    pop3_capabilities LAST TOP USER PIPELINING UIDL;

    smtp_auth         login plain cram-md5;
    smtp_capabilities "SIZE 10485760" ENHANCEDSTATUSCODES 8BITMIME DSN;
    xclient           off;

    server {
        listen   25;
        protocol smtp;
    }
    server {
        listen   110;
        protocol pop3;
        proxy_pass_error_message on;
    }
    server {
        listen   143;
        protocol imap;
    }
    server {
        listen   587;
        protocol smtp;
    }
}
```
## Directives
```
Syntax:	listen address:port [ssl] [proxy_protocol] [backlog=number] [rcvbuf=size] [sndbuf=size] [bind] [ipv6only=on|off] [so_keepalive=on|off|[keepidle]:[keepintvl]:[keepcnt]];
Default:	—
Context:	server
```
In this example, a basic mail server is configured to listen on port 25 for SMTP connections. The protocol directive specifies that this server block is intended for the SMTP protocol.
### SSL/TLS Configuration:
```
mail {
    server {
        listen     465 ssl;
        protocol   smtp;
        ssl_certificate     /path/to/certificate.pem;
        ssl_certificate_key /path/to/private_key.pem;
        # Additional SSL configuration directives go here
    }
}

```
For secure mail communication, you can enable SSL/TLS support using the ssl directive. This example configures the server to listen on port 465 for SMTP with SSL.
### Access Control:
```

mail {
    server {
        listen     25;
        protocol   smtp;
        allow      192.168.1.0/24;
        deny       all;
        # Additional access control directives go here
    }
}
```
Access control can be enforced using directives like allow and deny within a server block. In this case, the server allows connections from the IP range 192.168.1.0/24 while denying all others.
### Authentication:

[Nginx's mail modules support](https://nginx.org/en/docs/mail/ngx_mail_core_module.html) various authentication mechanisms, such as auth_http, auth_mysql, or auth_pam. These can be configured to secure mail servers by validating user credentials.

The ngx_mail_core_module empowers Nginx to serve as a versatile mail server, offering a range of configuration options for different mail protocols and secure communication. This module, combined with additional mail-specific modules, allows Nginx to efficiently handle diverse mail-related tasks within a single server setup.

Defines the address and port configuration for the socket on which the server will accept incoming requests. You can specify only the port if needed. Additionally, the address can be represented as a hostname. For instance:
```
listen 127.0.0.1:110;
listen *:110;
listen 110;     # same as *:110
listen localhost:110;
````
#### IPv6 addresses (0.7.58) are specified in square brackets:
```
listen [::1]:110;
listen [::]:110;
```
#### UNIX-domain sockets (1.3.5) are specified with the “unix:” prefix:
```
listen unix:/var/run/nginx.sock;
```
The proxy_protocol parameter (introduced in version 1.19.8) enables the specification that all connections accepted on this port should utilize the PROXY protocol. This feature allows the acquired information to be transmitted to the authentication server and can be leveraged to modify the client address on the [website](https://targeted-visitors.com).

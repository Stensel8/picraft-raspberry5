# About

This repository contains a lightweight Minecraft server optimized for running on a Raspberry Pi 5. The server uses PurpurMC, a high-performance Minecraft server implementation that is based on the popular PaperMC software.

The server is pre-configured with optimized settings files and scripts for dynamically changing server and plugin settings to ensure the best possible performance on a Raspberry Pi 5.

## Prerequisites

Before you can use this server, you'll need to make sure you have the following prerequisites:

- A Raspberry Pi 5 (8GB or 16GB model)
- A compatible power supply
- A NVMe drive with a Linux-based OS installed (NVMe HAT recommended)
- Java 21 or later installed on your Raspberry Pi

# Installation

## Web server Beta)

**Note**: The web server is only required if you want to use map plugins (such as DynMap, Pl3xMap, etc)

Install the following packages: `lighttpd`, `fcgi`, `php-cgi`

Update `/etc/lighttpd/lighttpd.conf` to:

```
server.port             = 25550
server.username         = "http"
server.groupname        = "http"
server.document-root    = "/srv/http"
server.errorlog         = "/var/log/lighttpd/error.log"
dir-listing.activate    = "enable"
index-file.names        = ( "index.html" )
mimetype.assign         = (
				".html" => "text/html",
				".txt" => "text/plain",
				".css" => "text/css",
				".js" => "application/x-javascript",
				".jpg" => "image/jpeg",
				".jpeg" => "image/jpeg",
				".gif" => "image/gif",
				".png" => "image/png",
				"" => "application/octet-stream"
			)
include "conf.d/fastcgi.conf"
```

Create the `/etc/lighttpd/conf.d/fastcgi.conf` file with:

```
server.modules += ("mod_fastcgi")
index-file.names += ("index.php")
fastcgi.server = (
    ".php" => (
        "localhost" => (
            "bin-path" => "/usr/bin/php-cgi",
            "socket" => "/tmp/php-fastcgi.sock",
            "broken-scriptfilename" => "enable",
            "max-procs" => "4",
            "bin-environment" => (
                "PHP_FCGI_CHILDREN" => "1"
            )
        )
    )
)
```

## Configuration

The server is pre-configured with optimized settings files and scripts for dynamically changing server and plugin settings. However, if you need to customize the server settings, you can do so by editing the following files:

Updating or customising those configurations should be done through the `scripts/configure-settings.sh` script.

## Usage

This server is designed to be easy to use and maintain. To start the server, simply run the following command:
```bash
./start.sh
```

This will start the server with the pre-configured settings and Java flags. To stop the server, simply use `Ctrl+C` in the terminal.

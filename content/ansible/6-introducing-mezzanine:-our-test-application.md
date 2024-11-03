---
title: "6 Introducing Mezzanine: Our Test Application"
date: 2024-10-23T14:41:55+04:00
draft: true
---

Deploying to Production is always more complicated than
running software in development mode on your laptop.

In production, you want to run a server-based database,
because those have better support for multiple, concurrent requests,
and server-based databases allow us to run multiple HTTP servers for load balancing.

Use NGINX as our web server for serving static assets and for handling the TLS encryption.

For a server application, we need it to run as a background process.

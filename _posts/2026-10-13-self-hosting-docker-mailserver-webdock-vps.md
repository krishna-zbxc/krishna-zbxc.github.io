---
title: "Self-Hosting Docker-Mailserver on a VPS"
date: 2026-10-14
categories: [Tech, Personal Projects]
tags: [self-hosting, email, docker, dns]
slug: self-hosting-docker-mailserver-vps
---

*Sorry for not posting for 11 months! Life got busy, but I am back with a brand new self-hosting project.*

Running a self-hosted mail server from scratch is one of the ultimate tests of patience and systems architecture. Let me tell you—nothing wanted to cooperate at first. I spent hours looping through the wrong commands over and over again, wrestling with stubborn configs, and dealing with a setup that felt completely fragile. But after pushing through the chaos and finally getting a solid 12-hour sleep to reset, I managed to get a fully working, containerized mail server up and running on my VPS.

Here is how I set it up, the roadblocks I hit, and how I got everything passing authentication.

---

## 1. Deploying docker-mailserver with Docker Compose

To keep everything clean and isolated, I used `docker-mailserver` via Docker Compose. Setting up Postfix, Dovecot, SpamAssassin, and ClamAV inside containers made management a lot easier.

Here is the configuration I used for my stack:

```yaml
services:
  mailserver:
    image: mailserver/docker-mailserver:latest
    container_name: mailserver
    hostname: mail.[your-domain]
    domainname: [your-domain]
    ports:
      - "25:25"
      - "143:143"
      - "465:465"
      - "587:587"
      - "993:993"
    environment:
      - ENABLE_SPAMASSASSIN=1
      - ENABLE_CLAMAV=1
      - ENABLE_FAIL2BAN=1
      - SSL_TYPE=letsencrypt
    volumes:
      - ./docker-data/mailserver/data:/var/mail
      - ./docker-data/mailserver/state:/var/mail-state
      - ./docker-data/mailserver/config:/tmp/docker-mailserver
      - /etc/letsencrypt:/etc/letsencrypt:ro
    restart: unless-stopped
```
Deploying it was as simple as running:
``` bash
docker compose up -d
```
2. Handling SSL/TLS & Client Integration
To avoid TLS handshake errors, I made sure my container mounts the system's Let's Encrypt live directories as read-only.
Connecting it to my mail client took a bit of fine-tuning. Pointing Apple Mail explicitly to my server hostname (mail.[your-domain]) rather than my root domain cleared up the certificate trust warnings immediately, allowing secure IMAP and SMTP connections.
3. The DNS & Authentication Gauntlet (SPF, DKIM, DMARC, rDNS)
Getting my mail to actually land in inboxes instead of spam folders was the hardest part. I had to configure four core DNS pillars:
 * SPF: I published a root TXT record authorizing my mail server IP:
   v=spf1 mx ip4:[your-server-ip] -all

 * DKIM: I generated RSA keys via the mailserver script. Because long keys exceed standard DNS line lengths, formatting them correctly across quote blocks was crucial to prevent validation errors:
   v=DKIM1; h=sha256; k=rsa; p=[your-dkim-public-key]

 * DMARC: I added a quarantine policy to handle unauthenticated mail:
   v=DMARC1; p=quarantine; rua=mailto:dmarc@[your-domain]; fo=1

 * Reverse DNS (PTR): I updated my VPS provider's control panel to ensure my server IP's PTR record matched my outbound HELO identity (mail.[your-domain]), preventing receiving servers from rejecting my messages.

Conclusion: 

After running external tests on Mail-Tester, my server achieved a solid 8.8/10 score with full SPF and DMARC verification.
Building a mail server from scratch while battling a fragile setup is definitely a trial-by-fire experience, but having full control over my own email infrastructure and seeing it pass all authentication checks makes the hours of debugging totally worth it.


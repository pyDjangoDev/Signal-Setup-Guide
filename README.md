# Signal Server Setup Guide

This guide is a community collaboration effort to setup on-premise (v3.21) Signal Server.

*Signal™ is a trademark of Quiet Riddle Ventures LLC. This projects is not affliated with Signal.org, Signal Messenger LLC, nor Signal Foundation.*

## Requirements
* **Ubuntu Server** (20.04 LTS)
* **Twilio** (For SMS OTP)
* **Amazon S3** (For Avatar and Attachments., can be subtituted with [MinIO](../master/signal-minio) )
* **Amazon SQS** (For CDS Queue. can be subtituted with LocalStack)
* **Firebase** (For push notifications)

## Content
* Backend:
    * [Server dependencies: PostgreSQL & Redis](https://github.com/aqnouch/signal-docker-dependencies)
    * [Signal Service](../master/signal-server)
    * Storage
    * CDN
    * CDN2
    * CDS
    * Uptime
    * API Backup
    * SFU Voip
    * Content Proxy
* Clients:
    * [Signal Android](../master/signal-android)
    * [Signal iOS](../master/signal-ios)
    * [Signal Desktop](../master/signal-desktop)

## Cotributing
You are welcome to contribute on this guide. If you have any questions please write an issues.

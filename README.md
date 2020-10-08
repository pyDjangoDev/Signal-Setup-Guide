# On-Premise Signal Setup Guide

This guide is a community collaboration effort to setup on-premise (self-hosted) Signal Server.

*Signal™ is a trademark of Quiet Riddle Ventures LLC. This projects is not affliated with Signal.org, Signal Messenger LLC, nor Signal Foundation.*

## Requirements
* **Twilio** (For SMS OTP)
* **Amazon S3** (For Avatar and Attachments., can be subtituted with [MinIO](../master/signal-minio) )
* **Amazon SQS** (For CDS Queue. can be subtituted with LocalStack)
* **Firebase** (For push notifications)

## Content
What's proven works
* [Server dependencies: PostgreSQL & Redis](https://github.com/aqnouch/signal-docker-dependencies)
* [Signal Server](../master/signal-server)
* [Signal Android](../master/signal-android)
* [Signal iOS](../master/signal-ios)
* [Signal Desktop](../master/signal-desktop)

## ToDos

* Contact Discovery Service (CDS)

## Cotributing
You are welcome to contribute on this guide. If you have any questions please write an issues.

# Signal Server Setup Guide: Service

*This guide is written by using Signal **v3.21**.*

## Requirement

* Serveur Ubuntu 18.04 or 20.04
* Java SE 14 
* Domaine name
* Firebase Cloud Messaging
* Twilio
* AWS(EC2, S3, CloudFront, SQS)

## Building Steps
1. First Clone service-dependencies.
```
git clone https://github.com/aqnouch/signal-docker-dependencies.git
cd signal-docker-dependencies
docker-compose build
docker-comose up -d 
```
2. Clone the project source code:
```
git clone https://github.com/signalapp/Signal-Server.git && cd Signal-Server
git checkout 3432529f9c018d75774ce89f3207b18051c26fe7
```
3. Update zkgroup dependency in service/pom.xml from 0.6.0 to 0.7.0.
```
<dependency>
    <groupId>org.signal</groupId>
    <artifactId>zkgroup-java</artifactId>
    <version>0.7.0</version>
</dependency>
```
4. Create your own `config.yml`, put it inside `signal-server/service/config/`. You can take a look at the [example here](../signal-server/example-signal.yml).
5. Build the server
```
mvn clean install -DskipTests
```
6. Generate zkparams value for the config and replace 'zkConfig' in config.yml with the results from the below command
```
java -jar service/target/TextSecureServer-3.21.jar zkparams
```
7. Generate value for **UnidentifiedDelivery**

You will get key pair using this command (keep the keypair, you will need it for Android and for the next step)
```
java -jar service/target/TextSecureServer-3.21.jar certificate -ca
```
Use the Private key to generate certificate (id can be random, i use 1234)
```
java -jar service/target/TextSecureServer-3.21.jar certificate --key <priv_key_from_step_above> --id 1234
```
Replace unidentifiedDelivery in config.yml with the certificate and privatekey from the above command. 

7.	Run the coTurn server for Audio/Video call.
8.	Migrate databases:
```
java -jar service/target/TextSecureServer-3.21.jar abusedb migrate service/config/config.yml
java -jar service/target/TextSecureServer-3.21.jar accountdb migrate service/config/config.yml
java -jar service/target/TextSecureServer-3.21.jar messagedb migrate service/config/config.yml
```

9.	Run the server
```
java -jar service/target/TextSecureServer-3.21.jar server service/config/config.yml
```

10. To run the server as daemon, use nohup
```
nohup java -jar service/target/TextSecureServer-3.21.jar server service/config/config.yml &>/dev/null &
```

## Nginx Reverse Proxy

1. Install nginx on your system
```
sudo apt install nginx     
```

2. generate a self signed certificate(change **domain.com** to your own domaine)

```
sudo openssl genrsa -out /etc/nginx/cert.key 2048
```

Create the config file configuration.conf:

```
[req]
distinguished_name=req
[SAN]
subjectAltName=DNS:your-domain or ec2-domain
```

And the certificate:

```
sudo openssl req -new -x509 -key /etc/nginx/cert.key -out /etc/nginx/cert.crt -days 3650 -subj /CN=domain.com -extensions SAN -config 'configuration.conf'
```

3. Allow Nginx to be accessed from outside using Firewall.
```
sudo ufw allow 'Nginx HTTPS'
```

4. Next you will need to edit the default Nginx configuration file in `/etc/nginx/sites-enabled/default`, using the [example nginx config](../signal-server/example-nginx.conf)
5. Update your nginx config to suits your need, you can take a look at the [example here](../signal-server/example-nginx.conf).

6. Check if your configuration is correct
```
sudo nginx -t
```

7. If there's no error, you can reload your nginx to apply the new configuration
```
sudo nginx -s reload
```

## FAQ

Q: How do I get GCM?

A: Setup [Firebase Cloud Messaging](https://firebase.google.com/), you will get the key from there.

Q: What AWS service do i need?

A: CDN Cloudfront, S3 Bucket, SQS FIFO type, and IAM for the key.

Q: How do I disable AccountCrawler Error?

A: Disable accountDatabaseCrawler logging by commenting `environment.lifecycle().manage(accountDatabaseCrawler);` it is located in `service/src/main/java/org/whispersystems/textsecuregcm/WhisperServerService.java`. Rebuild the server then rerun it after you did the modification.

```
...

apnSender.setApnFallbackManager(apnFallbackManager);
environment.lifecycle().manage(apnFallbackManager);
environment.lifecycle().manage(pubSubManager);
environment.lifecycle().manage(pushSender);
environment.lifecycle().manage(messagesCache);
// environment.lifecycle().manage(accountDatabaseCrawler);

...
```

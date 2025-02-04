# Infrastructure and application setup for new clients

*Welcome to the process of setting up your infrastructure and your application!*

![Page HTML load time](/Page-html-load-time.png)

Details about [running your web application](https://git.io/vNryB)

### Specialized infrastructure providers

Choose one per category.

1. Domain registrar:
   **Gandi :eu:, AWS, No-IP, Name.com by Donuts, Hexonet by CentralNic, Rackhost/.hu :eu:**
1. DNS provider with [DNSSEC](https://www.icann.org/news/announcement-2019-02-22-en):
   **AWS, HE, [Google](https://cloud.google.com/dns/pricing), Cloudflare, Exoscale :eu:, Gandi :eu:**
1. Server provider:
   **UpCloud :eu:**
1. SSL certificate provider for HTTPS:
   **[Cheapsslsecurity.com](https://cheapsslsecurity.com/rapidssl/rapidsslcertificate.html),
   [SSLMate](https://sslmate.com/),
   DigiCert,
   Certum :eu:,
   [Buypass](https://www.buypass.com/ssl) :eu:**
1. CDN provider:
   **AWS, KeyCDN :eu:, Akamai from Selectel**
1. Transactional email provider:
   **AWS, SparkPost, SparkPost EU :eu:**
1. Storage provider:
   **AWS, Backblaze B2, Selectel, Oktawave :eu:**

[.hu domain regisztrátorok](http://www.domain.hu/domain/)

[Google Cloud Platform Premium Support for $100/mo](https://cloud.google.com/support/?options=premium-support#support-options)

[AWS Europe invoicing](https://aws.amazon.com/legal/aws-emea/)

[AWS certificates for internal usage only](https://aws.amazon.com/certificate-manager/faqs/#general)

### Policy for user accounts at service providers

- Who is the legal owner of the account?
- Who has access to this account?
- Do we share account passwords?
- Do main accounts have 2FA?
- What other non-relevant services are under this account?
- Accounts for domain registration and DNS services must use an email address with a different domain name.
- Is the email account/phone number/bank card of this account in daily use?
- Use a virtual bank card with a sub account instead of a physical bank card tied to the main bank account!

### Secure browser in an ephemeral cloud instance

This section contains preparations for secure registration.

- Deploy [Windows Server 2016 Standard instance](https://hub.upcloud.com/server/create)
- Finish installation on the console: set language
- Log in as `Administrator` with
  [RDP on Windows](https://ci.freerdp.com/job/freerdp-nightly-windows/arch=win64,label=vs2013/)
  or [RDP on Mac](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12)
- Download [Basilisk browser](http://eu.basilisk-browser.org/release/basilisk-latest.win64.zip)
- Create UpCloud shortcut on the Desktop: `basilisk.exe "https://www.upcloud.com/register/?promo=U29Q8S"`
- Create AWS shortcut: `"https://portal.aws.amazon.com/gp/aws/developer/registration/index.html"`
- Download [`user.js`](https://github.com/szepeviktor/windows-workstation/blob/master/upcloud/user.js) to `%APPDATA%\Moonchild Productions\Basilisk\Profiles\`
- Open On-Screen Keyboard for entering passwords
- Use the browser
- Delete the instance

### UpCloud registration

- Referral URL
- [KeePass](https://keepass.info/) is an open source password manager
- **Enable 2FA** ([Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2))
- My Account / Billing / MANUAL
- My Account / Billing / AUTOMATED / Credit Card drop-down
- Servers / Deploy a server
- Check IP reputation (Security Trails, Project Honey Pot, HE BGP Toolkit, AbuseIPDB)
- Servers / Server listing / (server name) / IP ADDRESSES / REVERSE DNS NAME Public IPv4 + IPv6
- Log out (prevent session hijacking)
- Document server IP + password

### Amazon Web Services registration

- https://aws.amazon.com/
- [KeePass](https://keepass.info/) is an open source password manager
- Account type: Professional
- Verification phone call: dial numbers
- Support Plan: Basic
- **Enable 2FA** ([Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2))
- Billing preferences / Disable Free Tier Usage Alerts + Enable Billing Alerts
- CloudWatch / Create Alarm for EstimatedCharges
- Route53 / Domain + DNS
- CloudFront / CDN
- SES / Domain + SMTP credentials +
  [Move Out of the Sandbox](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/request-production-access.html) +
  Bounce notification
- S3 / Server backup bucket
- IAM / Route53 API user + CloudFront API user + S3 API user
- Log out (prevent session hijacking)
- Document credentials

### Cheapsslsecurity.com registration

[RapidSSL DV](https://cheapsslsecurity.com/rapidssl/rapidsslcertificate.html)

- Buy Multiple Years: 2 Year
- Billing Address, Payment Method

[Dashboard](https://cheapsslsecurity.com/client/ordersummary.html)

- Generate Cert Now
- (1) New **or** Renewal
- (2) Switching from Another SSL Brand: No
- (3) DNS Based Authentication
- (4) Generate CSR: `cert-update-req-install.sh DOMAIN`
- (5) Webserver: Other
- (6) SHA-2

Verify your URL

- Check domain name
- Set TXT record in DNS
- Wait for issuance

:bulb: Only ASCII characters in name and address.

[Dashboard / Manage Renewal Email Preferences](https://cheapsslsecurity.com/client/renewalemail-preferences.html)

- Select Admin/Technical contact: `[ ]` `[ ]`

##### Renew a certificate

[Dashboard](https://cheapsslsecurity.com/client/ordersummary.html)

- (_select the certificate_)
- RENEW

### Email delivery

- There is no guaranteed email delivery on the Internet
- [ESP](https://twofactorauth.org/#email) for *One-to-One* emails including inbound messages:
  **G Suite, [Protonmail :eu:](https://protonmail.com/signup), [DomainFactory :eu:](https://www.df.eu/int/e-mail-hosting/), [Почта Mail.Ru](https://biz.mail.ru/mail/)**
  <!-- not Tiliq -->
- File sharing, large file sending: [WeTransfer](https://wetransfer.com/),
  [Firefox Send](https://send.firefox.com/), [pCloud :eu:](https://transfer.pcloud.com/),
  [Smash](https://fromsmash.com/)
- *Transactional* emails and notification emails for alerts, log excerpts:
  [see providers above](#specialized-infrastructure-providers)
- *Bulk* email for newsletter: [see providers above](#specialized-infrastructure-providers)
- Bounce messages for **all three** email types
- Sender fraud protection and content integrity for **all three**: SPF, DKIM, DMARC
- My email address: `webmaster@`
- Teamwork in one Gmail inbox: [Drag](https://www.dragapp.com/)

<!--
- Email 2.0 for work: [Consider](https://consider.co/) (Gmail only)
-->

### Infrastructure setup

- Document in hosting.yml and server.yml (Skype, Google Contacts, KeePass, link-torzs)
- Gain access to providers (web based sub-account or API)
- Manage migrations (WeTransfer.com)
- PTR/IPv4, PTR/IPv6 records
- Domain locking and autorenew
- DNS records (check, clean up, monitor)
- Incoming ESP and bounce notification
- Whitelisted IP-s (office)

### Application setup

- Environments: development, staging, production
- 3rd parties (document, gain access, set up)
- User names and SSH keys
- Git repository, branch usage (git flow)
- Issue tracker
- Paid plugins, libraries (updates, gain access, support)
- Application environment definition
- Set up CI
- Write deploy script
- Notifications (email, chat, SMS)
- Revenue tracking
- Error tracking
- **Development: development in production?, who has access, where to develop, how to deploy**
- Editorial duties: **who has time and competence**

### Backup

- Data on servers is automatically backed up daily with 7 days rotation
- External resources (S3 bucket)
- Email accounts (local, IMAP)
- Issues (Clubhouse, Trello, GitHub, GitLab)
- Code repositories (GitLab, GitHub)

### Cyber security

- Notify on account breach: search email address https://haveibeenpwned.com/
- Notify on account breach: search password https://haveibeenpwned.com/Passwords
- All participants should stop using their browsers to store form data and passwords
- Data breach prevention in the application: automated attacks, paid hacker
- **Incident response plan** (outage, security incident)
- Spam filtering
- Protection against malware and phishing attacks (**credential stealing**)
- Against key loggers
- Against mobile malware
- Ransomware mitigation
- Yearly security check

### Collaboration

- _No emails if it is possible_
- Issues/ticketing: Clubhouse or Trello
- Chat: Slack

### Onboarding for developers

- We run Debian GNU/Linux on an UpCloud cloud instance
- All services run in [UTC timezone](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html)
- MariaDB or Percona Server + Apache with HTTP/2 and event MPM + PHP-FPM 7 + Redis
  ([full feature list](/debian-setup/debian-setup.sh#L23))
- Every web application (and website) runs as a separate Linux user
- There are no passwords for Linux users, only SSH keys
- All **non-production** servers are accessible through SSH: **terminal, MySQL tunnel, file upload, code deploy** etc.
- Production servers are not accessible for humans (except through HTTPS)
- TCP ports for web and SSH are heavily protected (maxretry=3) [with Fail2ban](/security/fail2ban-conf)
- Source code is kept in git (version-control system)
- PHP OPcache's [file timestamp validation](/webserver/phpfpm-pools/Skeleton-pool.conf#L30) is off,
  thus PHP files are read once at first access, we use [cachetool](https://github.com/gordalina/cachetool)
  to reset OPcache after code change
- There are *standard* directories for [sessions, upload and tmp](/webserver/phpfpm-pools/Skeleton-pool.conf#L36-L38)
- `.htaccess` files are disabled, Apache rules should be in vhost configuration (it is faster)
- File versioning is not in query string but turned into file names like `filename.002.ext` in URL-s,
  [an Apache rule](/webserver/apache-sites-available/Skeleton-site-ssl.conf#L155-L156) reverts them
- Your web application is protected by a [WAF](https://github.com/szepeviktor/waf4wordpress)
- Blacklisted things: FTP/S protocol, web-based administration tools (cPanel, phpMyAdmin), POP3/S protocol
- How to design and implement [CI and CD](/webserver/Continuous-integration-Continuous-delivery.md)
- [Running a Laravel application](/webserver/laravel)
- [Installing WordPress](/webserver/WordPress.md)
- Interesting read on [web applications](/webserver/PHP-development.md)

# DNS Records Explained: Your Email Security Foundation

**Version:** 1.0  
**Last Updated:** April 11, 2026  
**For:** All users (non-technical overview) & system administrators  

---

## Introduction

Your email relies on several DNS records working together. MailGuard monitors these records to ensure they're correctly configured. This guide explains what each one does, why it matters, and how to understand MailGuard's feedback.

---

## SPF (Sender Policy Framework)

### What It Does

SPF tells email providers: "These are the mail servers allowed to send email from my domain." It prevents spoofing by restricting who can claim to be sending from your domain.

### Example SPF Record

```
v=spf1 include:_spf.google.com include:sendgrid.net ~all
```

**Breaking it down:**
- `v=spf1` — SPF version
- `include:_spf.google.com` — Google's mail servers are authorized
- `include:sendgrid.net` — SendGrid is authorized
- `~all` — Other servers *fail soft* (still delivers, but marks as suspicious)
- `include:_spf.mailguard.net` — Any email service listed here is allowed

### What MailGuard Checks

✓ **Passes:** SPF record exists, is formatted correctly, and includes your email providers  
⚠ **Warning:** SPF record exists but may be incomplete (you send from a service not listed)  
✗ **Fails:** No SPF record, or syntax errors prevent it from working

### How to Fix It

1. List all email services you use (Gmail, Office 365, Mailchimp, SendGrid, etc.)
2. Add their SPF include directives to your record
3. Use `~all` (soft fail) unless you want to reject unauthenticated email
4. Test your record using an SPF checker (search "SPF lookup tool")

### Common Providers' SPF Includes

| Provider | SPF Include |
|----------|-------------|
| Google Workspace | `include:_spf.google.com` |
| Office 365 | `include:spf.protection.outlook.com` |
| SendGrid | `include:sendgrid.net` |
| Mailchimp | `include:servers.mcsv.net` |
| HubSpot | `include:hsrp.net` |

---

## DKIM (DomainKeys Identified Mail)

### What It Does

DKIM adds a cryptographic signature to your emails proving they came from you. It's like a tamper-proof seal on a letter.

### Example DKIM Record

```
v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC8...
```

This is a **public key** that tells email providers: "If you see an email from us signed with this key, it's genuine."

### What MailGuard Checks

✓ **Passes:** DKIM records exist for your domain, are valid, and use strong cryptography  
⚠ **Warning:** DKIM configured but may have weak settings or missing selectors  
✗ **Fails:** No DKIM record, invalid syntax, or weak key size

### How to Fix It

DKIM setup varies by email provider:

**Gmail (Google Workspace):**
1. Log in to Google Admin
2. Go to **Security** → **Authentication** → **Manage DKIM**
3. Click **Start authentication**
4. Google provides CNAME records to add to your DNS
5. Return after 24 hours to finalize

**Office 365:**
1. Go to Microsoft 365 admin center
2. Navigate to **Settings** → **Domains**
3. Select your domain
4. Choose **Add custom DKIM records**
5. Microsoft provides values to add to DNS

**SendGrid, Mailchimp, etc.:**
Your email provider will give you a DKIM selector and public key. Add it as a TXT record in DNS.

### Multiple DKIM Selectors

You may have multiple DKIM records (e.g., `default._domainkey.example.com`, `k1._domainkey.example.com`). This is normal if you use multiple email services—add them all.

---

## DMARC (Domain-based Message Authentication, Reporting and Conformance)

### What It Does

DMARC is the supervisor for SPF and DKIM. It says: "If an email claims to be from my domain but fails SPF or DKIM, here's what you should do with it."

### Example DMARC Record

```
v=DMARC1; p=quarantine; rua=mailto:dmarc-reports@example.com
```

**Breaking it down:**
- `v=DMARC1` — DMARC version
- `p=quarantine` — Policy: put failing emails in spam (or reject with `p=reject`)
- `rua=mailto:dmarc-reports@example.com` — Send daily summary reports to this email
- `ruf=mailto:forensics@example.com` — (Optional) Send detailed failure reports

### DMARC Policies Explained

| Policy | Behavior | When to Use |
|--------|----------|------------|
| `p=none` | Monitor only, no action taken | Testing phase (don't use long-term) |
| `p=quarantine` | Send failing emails to spam | Good balance between security and safety |
| `p=reject` | Reject failing emails entirely | Maximum security (but risky if SPF/DKIM misconfigured) |

**Important:** Start with `p=none` or `p=quarantine` while you get SPF/DKIM right. Only move to `p=reject` when you're confident everything is configured correctly.

### What MailGuard Checks

✓ **Passes:** DMARC record exists, policy is set to quarantine or reject, reporting is enabled  
⚠ **Warning:** DMARC exists but policy is `none`, or reporting email is misconfigured  
✗ **Fails:** No DMARC record, or syntax errors

### How to Fix It

1. Create a DMARC TXT record (see example above)
2. Set the policy to `quarantine` or `reject`
3. Add a reporting email address (any email you monitor)
4. Test using a DMARC lookup tool
5. After 1 week, check your reports to confirm SPF/DKIM are working
6. If reports show 100% pass rate, you can safely move to `p=reject`

---

## MX Records (Mail Exchange)

### What It Does

MX records tell the world which servers handle incoming email for your domain.

### Example MX Records

```
10  mail1.example.com
20  mail2.example.com  (backup)
30  mail3.example.com  (backup)
```

The numbers (10, 20, 30) are priority. Lower numbers are tried first. Have at least 1–2 backup MX records in case your primary mail server goes down.

### What MailGuard Checks

✓ **Passes:** MX records exist, are valid, and resolve to working mail servers  
⚠ **Warning:** Only one MX record (no backups), or MX records point to suspicious hosts  
✗ **Fails:** No MX records, or they don't resolve to valid servers

### How to Fix It

1. Check your email provider's documentation for their MX server hostnames
2. Add primary and secondary MX records to your DNS
3. Use appropriate priority numbers (10 for primary, 20 and higher for backups)
4. Save and verify using an MX lookup tool

---

## BIMI (Brand Indicators for Message Identification)

### What It Does

BIMI displays your company logo in recipient email inboxes—but only if SPF, DKIM, and DMARC are perfectly configured. It's a reward for strong email security.

### Example BIMI Record

```
v=BIMI1; a=self; l=https://example.com/logo.svg; vmc=https://example.com/vmc.pem
```

### What MailGuard Checks

✓ **Passes:** BIMI record exists and is properly configured  
⚠ **Warning:** BIMI record has issues, or prerequisite DMARC/SPF/DKIM not fully configured  
✗ **Fails:** No BIMI record, or SPF/DKIM/DMARC are not properly set

### How to Set Up BIMI

BIMI is optional and complex. Only pursue if you've already fixed SPF, DKIM, and DMARC.

1. Design a square SVG logo (usually 200x200px minimum)
2. Host the logo on your domain
3. Create a Verified Mark Certificate (VMC) from a BIMI certificate authority
4. Add the BIMI TXT record pointing to your logo and certificate
5. Test on bimi.mailmodo.com

---

## Blacklist Check

### What It Does

Spammers register domains, send millions of junk emails, then abandon the domain. Blacklist services track these bad domains and tell email providers to block them.

**Bad news:** Your domain's IP address might be on a blacklist (even if you're legitimate) due to a past owner or a hacked email account.

### What MailGuard Checks

✓ **Passes:** Your domain's sending IP address is not on major spam blacklists  
⚠ **Warning:** Listed on a minor or regional blacklist  
✗ **Fails:** Listed on major blacklist services (Gmail, Outlook, etc. may reject your mail)

### How to Fix It

If you're blacklisted:

1. **Stop sending spam** (obvious, but legitimate for us to say)
2. **Contact the blacklist service** and request delisting
3. **Implement proper authentication** (SPF, DKIM, DMARC)
4. **Fix the source** (hacked account, email injection flaw, etc.)
5. **Request re-scan** after 24–48 hours

Most blacklists automatically delist after 30 days of clean behavior.

---

## Certificate Expiry & TLS

### What It Does

When your email servers accept encrypted connections (TLS), they use SSL certificates to prove their identity. If the certificate expires, email from you might be rejected or flagged as insecure.

### What MailGuard Checks

✓ **Passes:** Certificates on your mail servers are valid and won't expire soon  
⚠ **Warning:** Certificate expires within 30 days  
✗ **Fails:** Certificate is already expired or invalid

### How to Fix It

Contact your email provider or sysadmin to renew certificates. Most modern providers handle this automatically.

---

## MTA-STS (Mail Transfer Agent Strict Transport Security)

### What It Does

MTA-STS tells email servers: "Always connect to my mail servers using encryption. Never fall back to unencrypted connections."

It's like requiring HTTPS for email.

### What MailGuard Checks

✓ **Passes:** MTA-STS is configured and enforces TLS  
⚠ **Warning:** MTA-STS exists but may have issues  
✗ **Fails:** No MTA-STS policy

### How to Fix It

1. Create an MTA-STS policy file (usually `mta-sts.txt`)
2. Host it at `https://.well-known/mta-sts.txt` on your domain
3. Add an MTA-STS TXT record pointing to your policy
4. Test using an MTA-STS checker

---

## DANE (DNS-based Authentication of Named Entities)

### What It Does

DANE allows you to publish your mail server's TLS certificate fingerprint in DNS. It prevents man-in-the-middle attacks on encrypted email connections.

### What MailGuard Checks

✓ **Passes:** DANE is configured and valid  
⚠ **Warning:** Partial DANE configuration  
✗ **Fails:** No DANE records

### How to Fix It

DANE is advanced. Only deploy if your mail servers and DNS support TLSA records. Consult your email provider.

---

## TLS-RPT (SMTP TLS Reporting)

### What It Does

TLS-RPT sends reports to you when email servers fail to connect to your mail servers using encryption.

### Example TLS-RPT Record

```
v=TLSRPTv1; rua=mailto:tlsrpt@example.com
```

### What MailGuard Checks

✓ **Passes:** TLS-RPT is configured and reporting is enabled  
⚠ **Warning:** TLS-RPT exists but may have issues  
✗ **Fails:** No TLS-RPT record

---

## PTR Records (Pointer Records)

### What It Does

PTR records tie IP addresses back to hostnames. Email servers use them to verify that the IP address sending mail actually matches the sending server's hostname.

### What MailGuard Checks

✓ **Passes:** Your mail server's IP address has a valid PTR record  
✗ **Fails:** No PTR record, or it doesn't match your mail server

### How to Fix It

PTR records are usually set by your hosting provider or ISP (not your domain registrar). Contact them to add/update the reverse DNS entry.

---

## Subdomain Checks

### What It Does

Sometimes you send email from subdomains (e.g., `noreply@alerts.example.com`). Subdomains can have their own SPF/DKIM/DMARC records—or inherit from the parent domain.

### What MailGuard Checks

✓ **Passes:** Subdomains that send mail have proper SPF/DKIM/DMARC configuration  
⚠ **Warning:** Subdomains may inherit parent settings but should have explicit records  
✗ **Fails:** Subdomains send mail but lack authentication records

---

## Troubleshooting Tips

### "My check result isn't updating—why?"

DNS changes can take 5–60 minutes to propagate. Wait a bit, then click "Run checks now" on your domain details page to force an immediate re-check.

### "I added the record but MailGuard still says it's failing"

1. Verify the record was added to the *correct* DNS zone (not a subdomain's zone)
2. Use an online DNS lookup tool (e.g., mxtoolbox.com) to confirm the record exists
3. Check for typos in the record value
4. Wait a few more minutes for full propagation

### "I'm worried about SPF/DKIM/DMARC—is this secure?"

Yes! These records are *published* DNS records. Anyone can see them. They don't expose secrets—they only prove ownership and set policy. Your actual private keys (DKIM signing keys) are never in DNS.

---

## Next Steps

1. **Review your MailGuard dashboard** to see which checks are passing
2. **Work through each failing check** using the guidance above
3. **Use online DNS tools** to verify your records are correct
4. **Set up alerts** so you know immediately if a record breaks
5. **Consider Premium** for AI-powered guidance on complex issues

---

## Additional Resources

- **DMARC.org** — Excellent DMARC primer
- **SPF Project** — Official SPF documentation
- **MXToolbox** — Free DNS lookup tools
- **MailGuard Support** — support@mailguard.alsecurity.com

Your email security is a journey, not a destination. MailGuard is here to guide you. 🔒

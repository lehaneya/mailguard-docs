# Getting Started with MailGuard

**Version:** 1.0  
**Last Updated:** April 11, 2026  
**For:** All users  

---

## Welcome to MailGuard

MailGuard by ALSecurity is your dedicated email security guardian. We monitor your domain's email infrastructure—checking critical DNS records like SPF, DKIM, and DMARC—to protect against email spoofing, phishing, and deliverability problems.

### What MailGuard Does

Every day, criminals attempt to impersonate businesses by spoofing their email addresses. MailGuard stops this by ensuring your email DNS records are correctly configured to:

- **Authenticate your email** (SPF, DKIM, DMARC)
- **Direct email to the right servers** (MX records)
- **Enhance trust and legitimacy** (BIMI, DANE, MTA-STS)
- **Prevent spoofing and phishing** against your domain

MailGuard runs automatic checks on your domains and alerts you when something breaks or needs attention.

---

## Before You Start

### System Requirements

- A web browser (Chrome, Firefox, Safari, Edge)
- Access to your domain's DNS records (typically your domain registrar or DNS host)
- An email address to receive alerts

### Account Tiers

MailGuard offers **two plans**:

| Feature | Standard ($20/mo) | Premium ($30/mo) |
|---------|-------------------|------------------|
| Domains included | 1 | 1 |
| DNS monitoring (SPF, DKIM, DMARC, MX, BIMI, etc.) | ✓ | ✓ |
| Real-time email alerts | ✓ | ✓ |
| Security score & letter grade | ✓ | ✓ |
| **AI-powered analysis** | ✗ | **✓** |
| **DMARC report ingestion** | ✗ | **✓** |
| Additional domains | $9.99 each | $9.99 each |

**Free Trial:** Start with a 14-day free trial. No credit card required.

---

## Step 1: Sign Up and Start Your Trial

1. Visit **mailguard.alsecurity.com** and click **"Start 14-day free trial"**
2. Enter your email address and create a strong password
3. Verify your email by clicking the link we send you
4. You're ready to add your first domain!

**No credit card is needed during the trial.** We'll only charge you if you decide to upgrade to a paid plan.

---

## Step 2: Add Your First Domain

Once logged in, you'll see the **Dashboard** with an "Add Domain" button.

### To add a domain:

1. Click **"Add Domain"** (top right of the Dashboard)
2. Enter your domain name (e.g., `example.com`)
3. Click **"Add"**
4. You'll see a **verification requirement**: a TXT DNS record to add to your domain

### Why verify?

Verification proves you own the domain. We'll check for a specific TXT record you place in your DNS settings.

---

## Step 3: Verify Domain Ownership

Verification requires you to add a **TXT record** to your domain's DNS. The process varies slightly depending on your DNS provider (GoDaddy, Cloudflare, Route 53, etc.), but the steps are similar:

### General steps:

1. **Sign in to your DNS provider** (domain registrar or DNS host)
2. **Find your DNS records** (often called "DNS Management" or "DNS Settings")
3. **Add a new TXT record** with:
   - **Name/Host:** `_mailguard` (or your root domain, depending on your provider)
   - **Value:** Copy the value MailGuard shows you on the Domain Details page
4. **Save the record**
5. **Return to MailGuard and click "Verify now"**

**Note:** DNS changes can take 5–60 minutes to propagate. If verification fails on first try, wait a few minutes and try again.

### Example (Cloudflare):

1. Log in to Cloudflare dashboard
2. Select your domain
3. Go to **DNS** → **Records**
4. Click **"Add record"**
5. Choose **TXT** as record type
6. Enter the name and value MailGuard provides
7. Click **"Save"**
8. Return to MailGuard and click "Verify now"

---

## Step 4: Check Your Security Score

Once verified, MailGuard automatically runs DNS checks on your domain. Within seconds, you'll see:

- **Security Score** (0-100): A numeric health score
- **Letter Grade** (A–F): A quick visual summary (A is best)
- **Check Results** (SPF, DKIM, DMARC, MX, etc.): Pass, Fail, or Warning status

### What the grades mean:

- **A (90-100):** Excellent DNS security. Minor issues if any.
- **B (80-89):** Good security. Review warnings and fix recommended items.
- **C (70-79):** Fair security. One or more checks are failing or misconfigured.
- **D (60-69):** Poor security. Multiple issues that could affect email delivery or increase spoofing risk.
- **F (Below 60):** Critical. Email authentication is likely broken. Fix immediately.

---

## Step 5: Review Check Results

Click on your domain from the Dashboard to see detailed results.

### Each check tab shows:

- **Status icon** (✓ Pass, ✗ Fail, ⚠ Warning)
- **What this check does** (plain English explanation)
- **Why it matters** (business impact)
- **Current configuration** (what we found)
- **Recommended fixes** (actionable next steps)

### Common checks you'll see:

**SPF (Sender Policy Framework)**  
Tells email providers which servers are allowed to send email for your domain.

**DKIM (DomainKeys Identified Mail)**  
Uses cryptographic signatures to verify emails are really from you.

**DMARC (Domain-based Message Authentication, Reporting and Conformance)**  
Ties SPF and DKIM together; tells providers what to do with unauthenticated email.

**MX Records**  
Direct incoming email to the correct mail servers.

**BIMI (Brand Indicators for Message Identification)**  
Display your company logo in recipient inboxes.

**Blacklist Check**  
Ensures your domain isn't on spam blacklists.

---

## Step 6: Set Up Email Alerts

You don't need to manually check MailGuard daily. We'll email you when something breaks.

### To configure alerts:

1. Go to **Alerts** (in the main menu)
2. Click **"Manage preferences"**
3. Choose which checks trigger alerts (e.g., "Alert me when DMARC fails")
4. Save your preferences

You'll receive an email immediately when:
- A DNS check fails
- A previously passing check starts failing
- A certificate is about to expire

---

## Step 7: Upgrade to Premium (Optional)

When your trial ends (14 days), you can:

- **Upgrade to Standard** ($20/month) for continued monitoring
- **Upgrade to Premium** ($30/month) to unlock AI analysis and DMARC report ingestion
- **Let your trial expire** (no charges)

### Premium benefits:

**AI-Powered Analysis**  
Get a plain-English summary of all your DNS security issues, along with step-by-step instructions to fix them. Powered by Claude AI.

**DMARC Aggregate Report Analysis**  
Instead of manually parsing DMARC reports, upload them to MailGuard. We parse, summarize, and analyze them for you.

---

## Common Questions

**Q: How often does MailGuard check my domains?**  
A: MailGuard checks your domains multiple times per hour, plus a full rescan every Sunday morning. Critical checks (like certificate expiry) run more frequently.

**Q: Can I monitor multiple domains?**  
A: Yes! Each plan includes 1 domain. Add more for $9.99/month per additional domain.

**Q: How is my DNS data kept secure?**  
A: MailGuard only reads your DNS records (no write access). All data is encrypted in transit and at rest. We never store your private keys.

**Q: What if I'm away and DNS breaks—will I know?**  
A: Yes! Configure email alerts and you'll get notified immediately. You can also invite team members to your MailGuard account (Premium feature).

**Q: Can I export my security score?**  
A: Yes. Each domain has a public security page and embeddable badge you can share with stakeholders or include in reports.

---

## Next Steps

1. ✓ **Add your first domain** and verify it
2. ✓ **Review your security score** and check results
3. ✓ **Set up email alerts** so you don't miss problems
4. ✓ **Fix failing checks** using the guidance MailGuard provides
5. ✓ **Upgrade to Premium** (when trial ends) if you want AI insights and DMARC analysis

---

## Need Help?

- **Check the detailed guides** in the Support section for each DNS record type
- **Contact us** at support@mailguard.alsecurity.com
- **Browse FAQs** on our website

We're here to help you keep your email secure! 🔐

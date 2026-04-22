# DMARC Aggregate Report Ingestion (Premium)

**Version:** 1.0  
**Last Updated:** April 11, 2026  
**For:** Premium subscribers (small business to enterprise IT teams)

---

## Overview

DMARC aggregate reports are daily summaries that email providers send to you showing authentication pass/fail statistics. Instead of manually collecting, parsing, and analyzing these reports, MailGuard does it for you.

MailGuard provides a **unique email address for each domain** where you can direct DMARC reports. Upload them to MailGuard, and we'll parse, summarize, and analyze them—giving you clear insights into your email authentication health.

---

## Why DMARC Reports Matter

Every day, Gmail, Outlook, Yahoo, and thousands of other email providers send DMARC reports to your domain. These reports show:

- **Total messages** claiming to be from your domain
- **Pass rate** (% authenticated via SPF/DKIM/DMARC)
- **Failures** (authentication breaches, spoofing attempts, misconfigured services)
- **Which senders** are using your domain (legitimate services or attackers)
- **Delivery outcomes** (how many reached inboxes vs. spam/rejected)

**Without reports**: You have no data. Are people spoofing your domain? Is your service misconfigured? You don't know.

**With reports**: You have proof. You see exactly what's happening and who's authenticating correctly.

---

## Tier Requirements

| Feature | Standard | Premium |
|---------|----------|---------|
| DMARC monitoring (DNS record check) | ✓ | ✓ |
| **DMARC report ingestion** | ✗ | **✓** |
| Manual report upload | ✗ | **✓** |
| Automatic report ingestion | ✗ | **✓** |
| **AI analysis of reports** | ✗ | **✓** |
| Report history & analytics | ✗ | **✓** |

DMARC report features are **Premium-only**.

---

## Part 1: Setting Up Automatic Report Ingestion

MailGuard provides a unique email address where email providers can send their DMARC reports automatically.

### Step 1: Get Your RUA Address

1. Log in to MailGuard
2. Navigate to **Premium** → **DMARC Reports** (or click on a domain → DMARC Reports tab)
3. Look for the **"RUA Address"** box at the top
4. Copy the email address (e.g., `rua-abc123def456@ingest.mailguard.alsecurity.com`)

### Step 2: Update Your DMARC Record

Now update your DMARC DNS record to tell email providers where to send reports.

**Current DMARC record might look like:**
```
v=DMARC1; p=quarantine; rua=mailto:dmarc-reports@example.com
```

**Update the `rua=` field:**
```
v=DMARC1; p=quarantine; rua=mailto:rua-abc123def456@ingest.mailguard.alsecurity.com
```

### Step 3: Verify the Change

1. Update your DNS record (go to your domain registrar or DNS provider)
2. Replace the `rua=mailto:...` value with MailGuard's address
3. Save the DNS record
4. Wait 5–60 minutes for DNS propagation
5. Go back to MailGuard and click **"Verify"** to confirm reports are flowing

**Within 24 hours**, email providers will start sending reports to MailGuard's address. You'll see them appear in the DMARC Reports page.

---

## Part 2: Manual Report Upload

If you already have DMARC reports (XML files) from your current system, you can upload them to MailGuard.

### Supported Formats

- `.xml` — Raw XML DMARC reports
- `.gz` — Gzipped XML (common from email providers)
- `.zip` — Zipped DMARC reports (multiple files)

### How to Upload

1. Go to **Premium** → **DMARC Reports**
2. Click **"Upload Report"** button
3. Select your report file(s)
4. MailGuard parses and ingests the file within seconds
5. You'll see the report in your history

### Where to Get Reports

**Gmail:**
1. Go to your Google Admin console
2. Navigate to **Security** → **DMARC & DKIM**
3. Under "Domain security", click on your domain
4. Download aggregate reports (XML files)
5. Upload to MailGuard

**Office 365:**
1. Log in to Microsoft 365 admin center
2. Go to **Security & Compliance** → **Reports** → **DMARC Reports**
3. Download reports as zip files
4. Upload to MailGuard

**Other Providers (Yahoo, ProtonMail, etc.):**
Reports typically arrive as email attachments. Download the `.xml.gz` or `.xml` file and upload it.

---

## Part 3: Understanding Your DMARC Reports

Once reports are flowing, you'll see a dashboard with:

### Summary Statistics

| Metric | Meaning |
|--------|---------|
| **Total Reports** | Number of DMARC reports received |
| **Total Messages** | Total number of messages claiming to be from your domain |
| **Passing Messages** | Count of messages that passed SPF/DKIM/DMARC checks |
| **Pass Rate** | Percentage of messages that authenticated successfully |

### What's a Good Pass Rate?

- **95%+** — Excellent. Your authentication is solid.
- **80-95%** — Good. Minor legitimate services need configuration.
- **60-80%** — Fair. Review failing sources; may have misconfigurations.
- **Below 60%** — Poor. Significant authentication issues or spoofing attempts.

### Report Table

Each report shows:

| Column | Meaning |
|--------|---------|
| **Organization** | Company that sent the report (Gmail, Outlook, Yahoo, etc.) |
| **Report ID** | Unique identifier for this report |
| **Records** | Number of authentication records in the report |
| **Received** | When MailGuard received the report |

Click on any report to see detailed records (rare for most users; mostly useful for troubleshooting).

---

## Part 4: AI-Powered Analysis (Premium)

MailGuard can use AI to analyze your DMARC reports and generate:

1. **Plain-English summary** of your email authentication status
2. **Risk assessment** explaining deliverability threats
3. **Action plan** with step-by-step fixes

### How to Run Analysis

1. Go to **Premium** → **DMARC Reports**
2. Click **"Analyze Reports"** button
3. Wait 10-30 seconds (AI analyzes your reports)
4. Read the results:
   - **Overview** — What's working, what isn't
   - **Deliverability Risk** — Impact on inbox placement
   - **Action Plan** — Specific steps to fix issues

### Example Analysis Output

**Overview:**  
"Your domain receives ~50,000 messages daily with an 87% DMARC pass rate. This is good, but indicates 13% of traffic isn't properly authenticated. Review your sending services."

**Deliverability Risk:**  
"The 13% failure rate puts you at moderate risk. Email from misconfigured services may be delivered to spam or rejected. This could impact newsletters, order confirmations, or password resets."

**Action Plan:**  
"1. Identify which services are failing (check DMARC reports for 'fail' entries). 2. For each failing service, verify SPF/DKIM is properly configured. 3. Add missing services to your SPF record. 4. Retest after 24 hours."

### Rate Limits

AI analysis is limited to **once per domain per day** to manage costs and ensure fair usage. If you need updated analysis, you can:

- **Refresh** (re-analyze with latest reports) — limited to once per day
- **Troubleshoot manually** using the report data
- **Contact support** for additional analyses

### Caching

Analyses are cached for 30 days. If you run analysis twice on the same day, you'll get a note saying "cached" with the timestamp of when it was generated.

---

## Troubleshooting DMARC Ingestion

### "I updated my DMARC record, but no reports are coming in. Why?"

1. **Wait 24-48 hours** — Email providers take time to notice the change
2. **Verify the DNS record** — Use an online DMARC lookup tool to confirm the RUA address is correct
3. **Check for typos** — Ensure the MailGuard RUA address is copied exactly
4. **Confirm report settings** — Some providers have separate toggles for enabling DMARC reporting

### "My pass rate is 0%. What does that mean?"

This typically means:
- No reports have been received yet (wait 24-48 hours)
- Or, all your messages are failing authentication (critical issue)

**Quick fix:** Verify SPF, DKIM, and DMARC records are all correctly configured. Use MailGuard's DNS check results as a starting point.

### "I see old reports that shouldn't be there. Can I delete them?"

DMARC reports are kept for 12 months for historical analysis. You cannot delete individual reports, but you can:

- **Start fresh** by rotating your RUA address (see advanced section below)
- **Contact support** if you have compliance/privacy reasons to purge

### "Reports are coming in, but the AI analysis says the same thing every day. Why not update?"

1. **Wait 7 days** of data collection before expecting big changes
2. **Check if sources changed** — New services failing, old ones fixed?
3. **Refresh the analysis** by clicking "Refresh Analysis" to force re-analysis

---

## Advanced: Rotating Your RUA Address

Your RUA address is unique and permanent, but you can rotate it if:

- You're moving to a different email provider
- You want to start fresh with a clean inbox
- You suspect the address was compromised

### How to Rotate

1. Go to **Premium** → **DMARC Reports**
2. Click **"Rotate RUA Address"**
3. MailGuard generates a new address
4. Update your DMARC DNS record with the new address
5. **Warning**: You'll no longer receive reports at the old address

Once you rotate, start collecting reports at the new address. Old reports remain in MailGuard for historical reference.

---

## Best Practices

### Do

✓ Update your DMARC record to point to MailGuard within the first week  
✓ Check reports weekly to spot trends  
✓ Use AI analysis to understand issues  
✓ Fix failing sources immediately (don't let them linger)  
✓ Archive old reports for compliance/audit purposes  
✓ Run analysis after major email infrastructure changes (new service, new provider, etc.)

### Don't

✗ Ignore reports with low pass rates (investigate immediately)  
✗ Upload duplicate reports (they'll skew statistics)  
✗ Rotate the RUA address casually (it disrupts data collection)  
✗ Share your RUA address publicly (keep it private)  
✗ Assume reports will magically fix your authentication (action is required)

---

## Examples & Use Cases

### Use Case 1: Discovering Spoofing

You notice your pass rate is 85%, but you only send email from one service. Investigation reveals:

- **70% from Gmail** (your configured sender) — 100% pass
- **15% from unknown IP** — 0% pass (spoofing attempts!)

**Action:** Contact your email provider, enable MTA-STS to prevent external senders, potentially update your DMARC policy to `p=reject`.

---

### Use Case 2: Finding a Misconfigured Service

Your company uses Mailchimp for newsletters, but recently added HubSpot for marketing automation. Pass rate drops from 98% to 92%.

DMARC reports show HubSpot traffic failing authentication.

**Action:** Add HubSpot's SPF include to your SPF record. Pass rate recovers to 97% within 24 hours.

---

### Use Case 3: Third-Party Integration

You integrate with a customer feedback tool that sends emails on your behalf. DMARC reports show 10% of traffic is from this tool and failing authentication.

**Options:**
1. Ask the vendor to use your DKIM/SPF infrastructure
2. Update your DMARC policy to allow them via SPF
3. Have them use `p=none` (reporting only) instead of `p=quarantine`

---

## Next Steps

1. **Copy your RUA address** from MailGuard
2. **Update your DMARC DNS record** to point to it
3. **Wait 24-48 hours** for reports to start flowing
4. **Review your pass rate** and identify any failing sources
5. **Use AI analysis** to understand issues and get fix recommendations
6. **Monitor weekly** and act on issues promptly

---

## Questions & Support

- **MailGuard Support:** support@mailguard.alsecurity.com
- **DMARC Questions:** See "DNS Records Explained" guide
- **Report Issues:** Check the Troubleshooting section above

DMARC reports are powerful. Use them to stay on top of email authentication and catch spoofing before it becomes a problem. 📊🔐

# Configuring Email Alerts

**Version:** 1.0  
**Last Updated:** April 11, 2026  
**For:** All users (non-technical overview) & administrators  

---

## Overview

MailGuard can email you when DNS checks fail, when configurations degrade, or when certificates are about to expire. This guide explains how to set up and manage alerts.

---

## Why Email Alerts?

Email problems often go unnoticed until your messages start bouncing or being marked as spam. MailGuard alerts you immediately when:

- A DNS check fails unexpectedly (someone misconfigured a record)
- A previously passing check degrades to a warning
- A certificate on your mail server is about to expire
- Your domain appears on a blacklist

**Without alerts**, you might not know for days or weeks. **With alerts**, you fix it in hours.

---

## Accessing Alert Settings

1. Log in to MailGuard
2. Click **Alerts** in the left menu
3. You'll see two sections:
   - **Alerts** (your notification history)
   - **Manage Preferences** (configure when to alert)

---

## Setting Up Your First Alert

### Step 1: Click "Manage Preferences"

You'll see a form to create alert rules.

### Step 2: Choose What to Monitor

Select which DNS checks should trigger alerts:

- **SPF** — Alert if SPF record fails or is misconfigured
- **DKIM** — Alert if DKIM check fails
- **DMARC** — Alert if DMARC record is missing or invalid
- **MX Records** — Alert if mail servers are unreachable
- **BIMI** — Alert if BIMI configuration breaks
- **Blacklist** — Alert if domain appears on spam blacklists
- **Certificate Expiry** — Alert if SSL certificates expire soon
- **MTA-STS** — Alert if MTA-STS policy fails
- **DANE** — Alert if DANE configuration breaks
- **TLS-RPT** — Alert if TLS reporting is broken
- **PTR Records** — Alert if reverse DNS fails
- **Subdomains** — Alert if subdomain authentication fails

You can select all checks, specific ones, or create multiple preferences for different domains.

### Step 3: Choose Domains

If you monitor multiple domains, select which ones this preference applies to. You can:

- Apply to all domains
- Select specific domains
- Create separate preferences for different domains with different alert rules

### Step 4: Choose Frequency

When should you get alerts?

- **Immediately** — Alert as soon as a check fails
- **Daily Summary** — One email per day listing all issues
- **Weekly Summary** — One email per week with changes

**Recommendation:** Start with "Immediately" for critical checks (SPF, DMARC) and "Daily Summary" for less critical ones.

### Step 5: Specify Alert Email

Enter the email address where alerts should be sent. This can be:

- Your personal email
- A team distribution list (e.g., `it-team@company.com`)
- A Slack email bridge (if your Slack workspace supports email forwarding)

### Step 6: Save

Click **"Save Preference"** and you're done!

---

## Understanding Alert History

The **Alerts** page shows your recent notifications:

| Column | Meaning |
|--------|---------|
| **Check Type** | Which DNS record had an issue (SPF, DKIM, etc.) |
| **Domain** | Which domain was affected |
| **Issue** | What went wrong (e.g., "SPF record missing") |
| **Severity** | Info, Warning, or Critical |
| **Date** | When the alert was sent |
| **Status** | Acknowledged or pending |

### Acknowledging Alerts

When you fix a DNS issue, MailGuard will send a follow-up alert. You can manually mark alerts as "Acknowledged" to clean up your inbox:

1. Click on an alert
2. Click **"Acknowledge"**
3. It moves to the "Reviewed" section

You can also **"Acknowledge All"** to mark everything as reviewed.

---

## Example Alert Configurations

### Setup 1: Startup or Small Business (All-In-One Alert)

**Goal:** Get notified of anything urgent immediately.

- **Checks:** All checks enabled
- **Domains:** All domains
- **Frequency:** Immediately
- **Email:** Your main business email

**Pros:** Maximum visibility, won't miss anything  
**Cons:** May get frequent emails if DNS frequently changes

---

### Setup 2: Enterprise with Multiple Domains

**Goal:** Different people manage different domains.

**Preference 1:**
- **Domain:** marketing.company.com
- **Checks:** DMARC, SPF, DKIM
- **Frequency:** Immediately
- **Email:** `marketing-ops@company.com`

**Preference 2:**
- **Domain:** support.company.com
- **Checks:** All checks
- **Frequency:** Daily summary
- **Email:** `infrastructure@company.com`

---

### Setup 3: Development/Testing (Noisy)

**Goal:** Monitor but don't spam the team with alerts.

- **Checks:** Certificate Expiry, Blacklist (only)
- **Domains:** All
- **Frequency:** Weekly summary
- **Email:** `devops-alerts@company.com`

Then investigate details on-demand in the MailGuard dashboard.

---

## What Each Alert Type Means

### SPF Alert

**Issue:** SPF record is missing, invalid, or incomplete.

**Action:** Review your SPF record. Have you added all your email providers? Common services: Google Workspace, Office 365, SendGrid, Mailchimp, HubSpot, etc.

---

### DKIM Alert

**Issue:** DKIM record missing, invalid, or uses weak cryptography.

**Action:** Contact your email provider and request DKIM setup, or consult the DNS Records guide for help.

---

### DMARC Alert

**Issue:** DMARC record missing, invalid, or policy too permissive.

**Action:** Create or fix your DMARC record. Start with `p=quarantine` if you haven't fully tested SPF/DKIM yet.

---

### MX Record Alert

**Issue:** Your domain's mail server is unreachable or unresponsive.

**Action:** Contact your email provider or hosting support. Check that mail servers are online and responding to DNS queries.

---

### Blacklist Alert

**Issue:** Your domain or IP address is listed on a spam blacklist.

**Action:** This is serious. Contact the blacklist service and request delisting. Check for compromised accounts or email injection vulnerabilities.

---

### Certificate Expiry Alert

**Issue:** Your mail server's SSL certificate expires in 30 days (or sooner).

**Action:** Contact your email provider or hosting company to renew the certificate. Modern providers usually handle this automatically.

---

### BIMI Alert

**Issue:** BIMI record is missing or misconfigured.

**Action:** BIMI is optional. Only fix if you've already perfected SPF, DKIM, and DMARC. Otherwise, ignore this alert.

---

## Troubleshooting Alert Issues

### "I'm not receiving alert emails. Why?"

1. **Check junk/spam folder** — Alerts might be filtered
2. **Add MailGuard to safe senders** — List `alerts@mailguard.alsecurity.com` as trusted
3. **Verify the email is correct** — Go to Alerts → Manage Preferences and confirm the email address
4. **Check alert preferences** — Make sure checks are enabled and frequency isn't set to "never"

### "I'm getting too many alerts. How do I quiet them down?"

1. Switch from "Immediately" to "Daily Summary" or "Weekly Summary"
2. Disable less critical checks (e.g., BIMI, TLS-RPT) if you don't need them
3. Create separate preferences for different domains with different rules
4. Mark alerts as "Acknowledged" after you've reviewed them

### "An alert says my SPF record is failing, but it looks correct to me. What's wrong?"

1. Use an online SPF lookup tool (mxtoolbox.com) to verify the record actually exists in DNS
2. Check for typos or extra spaces
3. Make sure the record was added to your root domain, not a subdomain
4. Wait a few minutes for DNS propagation and re-check

### "Can I forward alerts to Slack?"

Not directly (yet). But you can:
- Forward MailGuard emails to a Slack email integration if your Slack workspace supports it
- Use a service like Zapier or IFTTT to forward MailGuard notifications to Slack
- Contact us about Slack integration—it's on our roadmap!

---

## Advanced: Managing Multiple Alert Preferences

If you have many domains or team members, you can create multiple alert preferences:

| Preference | Applies To | Checks | Frequency | Email |
|-----------|-----------|--------|-----------|-------|
| 1 | Production domains | SPF, DKIM, DMARC, MX | Immediately | `ops@company.com` |
| 2 | Staging domains | All checks | Daily summary | `dev@company.com` |
| 3 | Partner integrations | Certificate Expiry | Weekly | `partnerships@company.com` |

**To manage:**

1. Go to **Alerts** → **Manage Preferences**
2. You'll see all existing preferences listed
3. Click **Edit** to change one
4. Click **Delete** to remove one
5. Click **Create New** to add another

---

## Best Practices

### Do

✓ Create at least one alert for critical checks (SPF, DKIM, DMARC, MX)  
✓ Use team distribution lists so alerts reach the right people  
✓ Set certificate expiry alerts to catch problems early  
✓ Review alert history regularly to spot patterns  
✓ Acknowledge alerts once fixed so they don't stay cluttering your inbox

### Don't

✗ Ignore "Immediately" alerts—they're usually serious  
✗ Leave alerts enabled for tests/staging domains (too noisy)  
✗ Forget to update email addresses when team members leave  
✗ Use only personal emails for alerts (what if you're on vacation?)  
✗ Disable all alerts just to quiet notifications (you'll miss problems)

---

## Next Steps

1. **Create your first alert preference** using the steps above
2. **Test it** by temporarily breaking a DNS record and waiting for the email
3. **Adjust frequency** based on how many emails you receive
4. **Share with your team** so everyone stays informed
5. **Review alert history monthly** to identify recurring issues

---

## Questions?

- **MailGuard Support:** support@mailguard.alsecurity.com
- **Documentation:** Check the DNS Records Explained guide for specifics on each check
- **Dashboard:** Click "Run checks now" on any domain to get immediate feedback

Your email security is only as strong as your awareness of problems. Alerts help you respond fast. 📧🔔

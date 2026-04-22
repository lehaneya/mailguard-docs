# AI-Powered Security Analysis (Premium)

**Version:** 1.0  
**Last Updated:** April 11, 2026  
**For:** Premium subscribers (business owners & IT administrators)

---

## What is AI Analysis?

MailGuard uses Claude, an advanced AI assistant by Anthropic, to analyze all of your DNS security results and provide:

1. **Plain-English summary** of your email security status
2. **Risk assessment** explaining business impact
3. **Step-by-step action plan** to fix problems

Instead of decoding technical DNS jargon, you get clear guidance in simple language.

---

## Tier Requirements

| Feature | Standard | Premium |
|---------|----------|---------|
| DNS monitoring | ✓ | ✓ |
| Email alerts | ✓ | ✓ |
| **AI-powered analysis** | ✗ | **✓** |
| DMARC report analysis | ✗ | **✓** |

AI features require a **Premium subscription**.

---

## Accessing AI Analysis

### From the Domain Details Page

1. Log in to MailGuard
2. Go to **Dashboard** and click on any domain
3. You'll see a tab labeled **"AI Analysis"** (marked with a ✦ star)
4. Click it to view or generate analysis

### First Time Running Analysis

If this is your first time:

1. Click the **"AI Analysis"** tab
2. Click **"Analyze Domain"** button
3. Wait 10–30 seconds while the AI reviews all your DNS checks
4. View the results

---

## Understanding Your Analysis Results

Your AI analysis consists of three parts:

### 1. Overview

A plain-English summary of your email security posture.

**Example:**

> "Your domain has good email security fundamentals. SPF, DKIM, and DMARC are all properly configured, which protects against spoofing and helps with inbox placement. Your security score of 78/100 is solid, with room for improvement in BIMI and MTA-STS configuration."

### 2. Deliverability Risk Assessment

Explains how DNS issues affect your ability to reach customer inboxes.

**Example:**

> "Your current configuration poses low-to-moderate risk to email delivery. While your core authentication is strong, the missing MTA-STS policy leaves a gap. If an attacker intercepts your mail server connection, emails could be delivered unencrypted, potentially flagged as suspicious by recipients. Estimate: 2-5% of emails could be misrouted or delayed."

### 3. Action Plan

Step-by-step fixes, prioritized by impact and complexity.

**Example:**

> "1. Enable DMARC Enforcement: Update your DMARC policy from `p=quarantine` to `p=reject` (requires 100% DKIM/SPF pass rate first). 2. Implement MTA-STS: Create a simple policy file and host it on your domain. 3. Monitor DMARC Reports: Upload weekly reports to track authentication health. 4. Consider BIMI: Set up brand logo display in inboxes (optional, improves brand trust)."

Each step is actionable and includes explanations of why it matters.

---

## How It Works (Non-Technical Overview)

1. **You run the analysis** by clicking a button
2. **MailGuard gathers all your DNS check results** (SPF, DKIM, DMARC, etc.)
3. **We send an encrypted summary to Claude AI** (along with industry best practices)
4. **Claude analyzes your security posture** and generates recommendations
5. **Results are returned to MailGuard** and displayed to you
6. **Your data is cached** for 30 days to avoid re-analysis

**Privacy:** Your DNS record values are never sent to Claude. Only the status (pass/fail/warning) and configuration summaries are analyzed. This is similar to hiring a security consultant—they review your setup but don't store or share your data.

---

## Using the Action Plan

The action plan is designed to be actionable, not overwhelming.

### For Each Recommended Action

Ask yourself:

1. **Is this critical?** (Risk to email delivery or security)
2. **Do I have the expertise?** (Can I implement it, or do I need IT help?)
3. **What's the effort?** (Quick DNS update vs. complex certificate setup?)
4. **What's the priority?** (Fix critical items first)

### Prioritization Guide

- **Critical (do first):** SPF, DKIM, DMARC failures, blacklist issues
- **Important (do within 2 weeks):** MTA-STS, TLS-RPT, missing certificate validation
- **Nice-to-have (do within a month):** BIMI, DANE, advanced configurations

### Getting Help

If an action is too technical:

- **Contact your email provider** (they can help with DKIM, MX records)
- **Hire a consultant** (for advanced configurations)
- **Email MailGuard support** (we can point you in the right direction)

---

## Example Scenarios

### Scenario 1: Startup with Basic Setup

**Your status:** SPF is configured, but DKIM and DMARC are missing.

**AI says:**  
"You have partial email authentication. SPF alone isn't enough to prevent spoofing. Add DKIM (most email providers offer free setup) and enable DMARC to complete your protection. This will take 1-2 hours of work."

**Your action:** Contact your email provider, follow their DKIM setup guide, add DMARC record within 24 hours.

**Result:** Your score improves from D (60/100) to B (85/100).

---

### Scenario 2: Growing Company Adding Services

**Your status:** SPF includes Gmail and SendGrid. You just added HubSpot marketing automation, and your pass rate dropped.

**AI says:**  
"Your core authentication is good, but a recent service addition (HubSpot) is failing authentication. Add HubSpot's SPF include to your record. Once updated, pass rates will recover to 95%+."

**Your action:** Update SPF record to include HubSpot. Takes 5 minutes.

**Result:** Pass rate recovers. No customer impact.

---

### Scenario 3: Compliance-Heavy Industry (Finance, Healthcare)

**Your status:** You have A-grade email security, but want stronger TLS enforcement.

**AI says:**  
"Your email security is excellent. For enhanced compliance, consider implementing MTA-STS to enforce encrypted connections and DANE for certificate pinning. These are advanced but valuable for regulated industries."

**Your action:** Work with IT to implement MTA-STS and DANE. Takes 1-2 days.

**Result:** Certificate authority-level protection. Compliance checkboxes ✓.

---

## Rate Limits

AI analysis is rate-limited to protect costs and ensure fair usage:

- **Standard Premium plan:** 1 analysis per domain per day
- **Multiple domains:** Each domain has its own quota (so 10 domains = 10 analyses per day total)
- **Refreshing:** You can re-analyze the same domain once per day (shows as "cached" if analyzed within 24 hours)

If you need more frequent analysis (e.g., real-time dev/testing), contact support about enterprise plans.

---

## Caching & Expiration

Analysis results are cached (saved) to reduce redundant work:

- **Cache duration:** 30 days
- **Cache indicator:** Results show "Generated X days ago" and "Expires in Y days"
- **Refresh button:** Click "Refresh Analysis" to re-run and update cache
- **Auto-expiry:** After 30 days, analysis is considered stale and should be re-run

---

## When to Re-Run Analysis

Run a fresh analysis after:

- **DNS record changes** (added SPF include, enabled DMARC, etc.)
- **Service migrations** (switched email providers)
- **Security incidents** (spoofing attempt, compromise, etc.)
- **Regular reviews** (monthly or quarterly)

You don't need to refresh every day unless your infrastructure changes frequently.

---

## Troubleshooting AI Analysis

### "I clicked 'Analyze Domain' but nothing happened. Why?"

**Possible reasons:**

1. **Not Premium:** Standard plan subscribers can't use AI features. Upgrade to Premium.
2. **Rate limit hit:** You already analyzed this domain today. Try again tomorrow.
3. **No DNS checks completed:** Domain may not be verified yet. Verify domain ownership first.
4. **Browser issue:** Reload the page and try again.

**Verify you're Premium:**
1. Go to **Billing** page
2. Confirm your subscription shows "PREMIUM"

---

### "The analysis mentions a check that I already fixed. Why?"

Analysis caches results for 30 days. If you recently fixed something:

1. Click **"Refresh Analysis"** button
2. Wait for updated results
3. Results will now reflect your fix

---

### "I don't understand one of the recommendations. What does it mean?"

Each recommendation includes an explanation. If it's still unclear:

1. Check the **DNS Records Explained** guide (in Help section)
2. Search your email provider's knowledge base (e.g., "Gmail MTA-STS setup")
3. Email MailGuard support: support@mailguard.alsecurity.com

---

### "Can I get AI analysis for DMARC reports too?"

**Yes!** If you have Premium and are using DMARC report ingestion:

1. Go to **Premium** → **DMARC Reports**
2. Ensure you have uploaded reports
3. Click **"Analyze Reports"** button
4. Get AI analysis of your authentication patterns

This is separate from domain DNS analysis. You can have both!

---

## Privacy & Security

### What data does AI see?

✓ Status of DNS checks (pass/fail/warning)  
✓ Types of checks monitored (SPF, DKIM, etc.)  
✓ Configuration summaries (e.g., "3 SPF includes detected")  
✗ Actual DNS values (secrets are never shared)  
✗ Private keys or sensitive data  

### How is data protected?

- **Encryption:** Sent via HTTPS (encrypted in transit)
- **No storage:** Claude AI doesn't store your data after analysis
- **Caching:** MailGuard caches results on our secure servers
- **Compliance:** MailGuard complies with GDPR, CCPA, and SOC 2

---

## Best Practices

### Do

✓ Run analysis when you first set up MailGuard (baseline)  
✓ Re-run after making DNS changes to see improvement  
✓ Share the action plan with your IT team  
✓ Prioritize critical items first  
✓ Monitor your score over time  
✓ Use analysis as a checklist for improvements

### Don't

✗ Ignore the overview (it explains your overall posture)  
✗ Blindly follow all recommendations (prioritize based on risk)  
✗ Share your analysis externally (it's for internal use)  
✗ Expect AI to solve everything (it advises; you implement)  
✗ Spam "refresh" on the same domain (rate-limited for a reason)

---

## Integration with Other Features

### With Alerts

- **Alerts** notify you when something breaks
- **AI Analysis** helps you understand and fix the break

Example: Alert says "DMARC policy too permissive," then AI recommends "Update to p=quarantine after verifying 100% SPF/DKIM pass rate."

### With DMARC Reports

- **Reports** show authentication failures in real data
- **AI Analysis** explains what those failures mean and how to fix them

Example: Report shows 10% of emails failing DKIM. AI says "Check for misconfigured sending services."

### With Dashboard

- **Dashboard** shows your current security score
- **AI Analysis** explains what to do to improve it

Example: Score is 72/100 (C). AI says "Fix the 3 warnings to reach 85/100 (B)."

---

## Example: Full Workflow

1. **Day 1:** Sign up, add domain, get score of 65/100
2. **Day 1:** Click "AI Analysis" to understand issues
3. **Day 1-2:** Follow the action plan to fix top 3 items
4. **Day 3:** Score improves to 82/100. Set up alerts.
5. **Week 2:** AI analysis reminds you to consider BIMI (optional)
6. **Month 1:** Regular AI review shows you're now at 90/100 (A grade)
7. **Ongoing:** Alerts notify you if something breaks; rerun AI after fixes

---

## Next Steps

1. **Go to your domain** → **AI Analysis** tab
2. **Click "Analyze Domain"** to generate your first analysis
3. **Review the overview** to understand your posture
4. **Prioritize the action plan** based on your risk tolerance
5. **Share with your team** (if applicable)
6. **Implement the top 3 recommendations** this week
7. **Rerun analysis** after 1 week to see improvement

---

## Questions?

- **General:** See the Getting Started guide
- **DNS specifics:** Check DNS Records Explained
- **DMARC:** See DMARC Report Ingestion guide
- **Support:** support@mailguard.alsecurity.com

AI analysis is a tool to make you smarter about your email security—not to replace your expertise. Use it, but trust your judgment. 🧠🔐

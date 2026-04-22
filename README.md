# MailGuard Customer Documentation

**Version:** 1.0  
**Last Updated:** April 11, 2026  
**Product:** MailGuard by ALSecurity — Email DNS Security Monitoring

---

## Welcome

MailGuard helps you protect your email infrastructure by monitoring critical DNS records and alerting you to security issues before they impact your business.

This documentation covers everything from getting started to advanced technical integrations.

---

## Documentation Index

### For Everyone

**[Getting Started](01-getting-started.md)**  
Sign up, add your first domain, understand your security score, and configure alerts. Start here if you're new to MailGuard.

**[DNS Records Explained](02-dns-records-explained.md)**  
Plain-English explanations of SPF, DKIM, DMARC, MX, BIMI, and other DNS records that protect your email. Understand what each check does and how to fix failures.

**[Alerts Configuration](03-alerts-configuration.md)**  
Set up email notifications so you know immediately when DNS security issues occur. Create custom alert preferences for different domains and teams.

**[Billing & Plans](06-billing-and-plans.md)**  
Understand MailGuard's pricing, plans, trial, subscriptions, and account management.

---

### For Premium Subscribers

**[DMARC Report Ingestion](04-premium-dmarc-reports.md)**  
Use MailGuard's unique email address to automatically collect DMARC aggregate reports. Upload historical reports. View pass rates and authentication trends.

**[AI-Powered Analysis](05-premium-ai-analysis.md)**  
Get AI-generated plain-English summaries of your email security posture, risk assessments, and step-by-step action plans. Powered by Claude AI.

---

### For Developers & System Administrators

**[Technical Reference](07-technical-reference.md)**  
REST API endpoints, authentication, webhook examples, SDK documentation, and self-hosted deployment configuration.

---

## Quick Start (3 Minutes)

1. **Sign up** at mailguard.alsecurity.com — 14-day free trial, no credit card needed
2. **Add your domain** (e.g., `example.com`)
3. **Verify ownership** by adding a TXT record to your DNS
4. **Review your security score** — See which DNS checks pass or fail
5. **Fix any failing checks** using the guidance MailGuard provides

See [Getting Started](01-getting-started.md) for detailed instructions.

---

## Understanding Your Security Score

MailGuard grades your email security on a scale of 0-100:

- **A (90-100):** Excellent DNS security
- **B (80-89):** Good security; minor issues
- **C (70-79):** Fair security; review warnings
- **D (60-69):** Poor security; multiple issues
- **F (<60):** Critical; email authentication likely broken

Your score is based on 12+ DNS checks:

| Check | Why It Matters |
|-------|----------------|
| **SPF** | Prevents spoofing by authorizing mail servers |
| **DKIM** | Cryptographic signatures prove emails are authentic |
| **DMARC** | Enforces policy when SPF/DKIM fail |
| **MX** | Routes incoming email to correct servers |
| **BIMI** | Display your logo in recipient inboxes |
| **Blacklist** | Ensures domain isn't flagged as spam |
| **Certificate** | Validates mail server encryption certificate |
| **MTA-STS** | Enforces encrypted mail server connections |
| **DANE** | Prevents man-in-the-middle attacks |
| **TLS-RPT** | Reports TLS connection failures |
| **PTR** | Links IP addresses to hostnames |
| **Subdomains** | Monitors subdomains that send email |

Learn more in [DNS Records Explained](02-dns-records-explained.md).

---

## Plans & Pricing

### Standard — $20/month

- 1 domain included
- 12+ DNS security checks
- Real-time email alerts
- Security score and letter grade
- Public security page
- Embeddable badge

### Premium — $30/month

Everything in Standard, plus:

- **AI-powered analysis** — Plain-English summaries and fix recommendations
- **DMARC report ingestion** — Automatic report collection and analysis
- **Report analytics** — Track authentication pass rates over time

**Free Trial:** 14 days of Standard features, no credit card required.

**Additional Domains:** $9.99/month each (add anytime).

Learn more in [Billing & Plans](06-billing-and-plans.md).

---

## Common Workflows

### Workflow 1: New User (Standard Plan)

1. Sign up and verify your domain
2. Review your security score
3. Fix failing checks one by one (DNS Records Explained helps)
4. Set up email alerts so you know if something breaks
5. (Optional) Upgrade to Premium for AI guidance

### Workflow 2: IT Administrator (Multiple Domains)

1. Add all your company domains
2. Create alert preferences for each domain
3. Route alerts to appropriate teams (ops, marketing, etc.)
4. Review security scores monthly
5. Use AI analysis (Premium) to improve weakest domains

### Workflow 3: Compliance Officer (Premium)

1. Monitor all production domains
2. Review DMARC reports to verify authentication health
3. Use AI analysis to document security posture
4. Generate reports for audits
5. Track improvements over time

### Workflow 4: API Integration

1. Generate API credentials in Settings
2. Use REST API to query domain status
3. Integrate alerts into your ticketing system
4. Build custom dashboards
5. Automate remediation workflows

---

## Troubleshooting

### "I verified my domain but checks aren't running"

DNS changes take 5–60 minutes to propagate globally. Wait a bit, then click "Run checks now" on the domain details page.

### "My SPF/DKIM/DMARC looks correct but MailGuard says it's failing"

Use an online DNS lookup tool (e.g., mxtoolbox.com) to verify the record exists. Check for:
- Typos or extra spaces
- Record added to wrong DNS zone
- DNS propagation delay (wait 5-60 minutes)

### "I'm not receiving alert emails"

1. Check your junk/spam folder
2. Add `alerts@mailguard.alsecurity.com` to safe senders
3. Verify your email address in Alerts → Manage Preferences
4. Ensure alerts are enabled and frequency isn't "never"

### "Can I export my data?"

Yes. You can:
- Download PDF invoices from Billing page
- Access API to pull domain and check data programmatically
- Export DMARC reports as CSV (Premium)
- Contact support for data export

### "What if something breaks?"

1. **Check the dashboard** — See which check is failing
2. **Review the guidance** — MailGuard explains what's wrong
3. **Check DNS Records Explained** — Get fix instructions
4. **Use AI Analysis** (Premium) — Get personalized action plan
5. **Contact support** — support@mailguard.alsecurity.com

---

## Frequently Asked Questions

### Q: How often does MailGuard check my domains?

**A:** Multiple times per hour during the day, plus a full rescan every Sunday. Critical checks (like certificate expiry) run more frequently.

---

### Q: Can I monitor subdomains?

**A:** Yes. MailGuard automatically detects subdomains that send email and monitors them. You can also manually add subdomains.

---

### Q: Is my data secure?

**A:** Yes. MailGuard uses industry-standard encryption (TLS in transit, encrypted at rest). We comply with GDPR, CCPA, and SOC 2. DNS records are *public* information—MailGuard doesn't expose secrets.

---

### Q: Can I cancel anytime?

**A:** Yes. Cancel your subscription anytime, no penalty. You'll receive a pro-rata refund for unused days.

---

### Q: Do you offer API access?

**A:** Yes. REST API with SDKs for JavaScript, Python, and Go. See [Technical Reference](07-technical-reference.md).

---

### Q: How do I add team members?

**A:** Go to Settings → Team and invite colleagues by email. They'll get access to your domains (Premium feature).

---

### Q: What happens to my data if I cancel?

**A:** Your account data is deleted after 30 days. DMARC reports and AI analyses are deleted when you cancel. Download/export what you need before canceling.

---

### Q: Is MailGuard suitable for my industry?

**A:** MailGuard works for any business that sends email. It's especially valuable for:
- **SaaS/Tech** — Transactional emails, alerts, sign-ups
- **E-commerce** — Order confirmations, shipping notifications
- **Finance** — Account statements, security alerts
- **Healthcare** — Patient communications, compliance
- **Education** — Student communications, notifications
- **Nonprofits** — Fundraising, communications

---

## Getting Help

### Resources

- **Documentation:** You're reading it! Browse the guides above.
- **Dashboard help:** Hover over any question mark icon for tooltips.
- **Email support:** support@mailguard.alsecurity.com
- **Status page:** status.mailguard.alsecurity.com
- **Twitter/X:** @MailGuardSecure

### Before Contacting Support

- Check the relevant guide (e.g., DNS Records Explained for DNS issues)
- Use online tools (e.g., MXToolbox) to verify your DNS
- Check the Troubleshooting section of relevant guides
- Review your alert history and error messages

### Enterprise Support

For organizations with 10+ domains or requiring SLAs:

- **Email:** enterprise@mailguard.alsecurity.com
- **Phone:** Available for enterprise customers
- **Dedicated contact:** Assigned support person
- **Custom integrations:** API support and guidance

---

## Roadmap & Feature Requests

MailGuard is constantly improving. Planned features include:

- **Lookalike domain monitoring** — Detect domains impersonating yours
- **Email header analyzer** — Debug email delivery issues
- **Compliance checker** — Verify security standards
- **Slack integration** — Get alerts in Slack
- **GraphQL API** — Alternative to REST API
- **Annual billing discount** — Save 20% with yearly plans
- **Multi-user teams** — Shared account management
- **Advanced reporting** — Custom dashboards and exports

### Request a Feature

Email feature requests to: product@mailguard.alsecurity.com

---

## Documentation Feedback

Found an error? Want to suggest improvements? Let us know:

- **Email:** docs@mailguard.alsecurity.com
- **GitHub Issues:** github.com/alsecurity/mailguard-docs/issues (if available)

---

## Legal & Compliance

- **Privacy Policy:** mailguard.alsecurity.com/privacy
- **Terms of Service:** mailguard.alsecurity.com/terms
- **Security:** mailguard.alsecurity.com/security
- **Compliance:** SOC 2 Type II, GDPR, CCPA

---

## Your Security Journey

Email security is a journey. MailGuard is here to guide every step:

1. **Baseline** — See where you stand with your security score
2. **Improve** — Fix failing checks one by one
3. **Monitor** — Stay alerted to problems
4. **Optimize** — Use AI analysis to identify next steps
5. **Maintain** — Prevent regression with ongoing checks

Every improvement reduces risk and improves deliverability. Start today. 🔐

---

## Document Map

```
docs/customer/
├── README.md                          (You are here)
├── 01-getting-started.md             (Beginner's guide)
├── 02-dns-records-explained.md        (DNS concepts)
├── 03-alerts-configuration.md         (Alerts setup)
├── 04-premium-dmarc-reports.md        (DMARC ingestion)
├── 05-premium-ai-analysis.md          (AI features)
├── 06-billing-and-plans.md            (Pricing & subscription)
└── 07-technical-reference.md          (API & developer docs)
```

---

**Last Updated:** April 11, 2026  
**Version:** 1.0  
**© 2026 ALSecurity, Inc. All rights reserved.**

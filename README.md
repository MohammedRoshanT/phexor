# Phexor 🎣
> AI-powered Phishing Email Analyzer

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=flat-square&logo=python)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-lightgrey?style=flat-square)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)

Phexor is a command-line phishing email analysis tool that parses raw `.eml` files, extracts and scans embedded URLs, detects authentication failures, and runs everything through an AI analyst to produce a structured phishing verdict in seconds.

---

## What It Does

Given a raw `.eml` email file, Phexor:

1. **Parses** headers, sender info, URLs, and attachments from the email
2. **Analyzes** SPF, DKIM, DMARC authentication results for spoofing indicators
3. **Detects** header anomalies — Reply-To mismatches, suspicious subject keywords
4. **Scans** extracted URLs against **VirusTotal** and **URLscan.io**
5. **Feeds** all findings to **Groq AI (Llama 3.3 70B)** for phishing assessment
6. **Outputs** a structured verdict with MITRE ATT&CK mapping
7. **Saves** full report as JSON or TXT

---

## Demo

```
╭──────────────────────────────────── Phishing Email Analyzer ────────────────────────────────────╮
│ Phexor | File: test_phish.eml                                                                   │
╰─────────────────────────────────────────────────────────────────────────────────────────────────╯
╭───────────────────────────────────── Email Summary ─────────────────────────────────────────────╮
│  From       PayPal Security <security@paypa1-support.com>                                       │
│  Reply-To   collect@harvester-domain.ru                                                         │
│  Subject    URGENT: Your account has been suspended - Verify immediately                        │
│  Sender IP  185.220.101.45                                                                      │
│  SPF        FAIL                                                                                │
│  DKIM       Missing                                                                             │
│  URLs Found 2                                                                                   │
╰─────────────────────────────────────────────────────────────────────────────────────────────────╯
⚠ Header Anomalies Found: 5
  → SPF check failed
  → DKIM signature missing
  → DMARC check failed
  → Reply-To mismatch: sender != reply_to
  → Suspicious subject keywords: ['urgent', 'verify', 'suspended', 'account', 'immediately']

╭───────────────────────────────────── AI Phishing Analysis ──────────────────────────────────────╮
│ PHISHING VERDICT: Phishing                                                                      │
│ CONFIDENCE SCORE: 95%                                                                           │
│                                                                                                 │
│ KEY INDICATORS:                                                                                 │
│ • SPF/DKIM/DMARC failures indicate domain spoofing                                             │
│ • Reply-To redirects to harvester-domain.ru                                                    │
│ • URLs point to credential harvesting pages                                                    │
│ • Urgency-driven subject line matches phishing TTPs                                            │
│                                                                                                 │
│ ATTACK TYPE: Credential Harvesting                                                              │
│ RECOMMENDED ACTION: Block                                                                       │
│                                                                                                 │
│ MITRE ATT&CK:                                                                                   │
│ • T1566 – Phishing (Spearphishing Link)                                                        │
│ • T1598 – Phishing for Information                                                             │
╰─────────────────────────────────────────────────────────────────────────────────────────────────╯

Report saved → reports/test_phish_20260516_142301.json
```

---

## Installation

```bash
# Clone the repository
git clone https://github.com/MohammedRoshanT/phexor.git
cd phexor

# Create virtual environment
python -m venv venv

# Linux / Parrot OS / Kali
source venv/bin/activate

# Windows
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

---

## Configuration

Create a `.env` file in the project root:

```env
VT_API_KEY=your_virustotal_key
ABUSEIPDB_API_KEY=your_abuseipdb_key
GROQ_API_KEY=your_groq_key
URLSCAN_API_KEY=your_urlscan_key
```

| API | Free Tier | Get Key |
|-----|-----------|---------|
| VirusTotal | ✅ | [virustotal.com](https://www.virustotal.com/gui/join-us) |
| AbuseIPDB | ✅ | [abuseipdb.com](https://www.abuseipdb.com/register) |
| URLscan.io | ✅ | [urlscan.io](https://urlscan.io) |
| Groq | ✅ | [console.groq.com](https://console.groq.com) |

---

## Usage

```bash
# Analyze a .eml file
python main.py email.eml

# Save report as plain text
python main.py email.eml --output txt
```

---

## Project Structure

```
phexor/
├── main.py                  # CLI entry point
├── requirements.txt
├── .env                     # API keys (not committed)
├── modules/
│   ├── email_parser.py      # Parse .eml, extract headers/URLs/attachments
│   ├── header_analyzer.py   # SPF/DKIM/DMARC anomaly detection
│   ├── url_scanner.py       # VirusTotal + URLscan.io URL scanning
│   └── ai_analyst.py        # Groq AI phishing verdict
└── reports/                 # Saved analysis reports (auto-generated)
```

---

## Detection Capabilities

| Check | Details |
|-------|---------|
| SPF | Pass / Fail / Missing |
| DKIM | Present / Missing |
| DMARC | Pass / Fail / Missing |
| Reply-To Mismatch | Sender vs Reply-To domain comparison |
| Subject Keywords | Urgency/phishing keyword detection |
| URL Scanning | VirusTotal + URLscan.io per extracted URL |
| Sender IP | Extracted from Received headers |
| Attachments | Filename and MIME type extraction |

---

## AI Analyst Output

Every scan produces a structured AI phishing report:

- **Phishing Verdict** — Phishing / Suspicious / Legitimate / Unknown
- **Confidence Score** — 0 to 100%
- **Key Indicators** — Strongest phishing signals detected
- **Attack Type** — Credential harvesting / Malware delivery / BEC / Spam
- **Recommended Action** — Block / Quarantine / Release / Investigate
- **MITRE ATT&CK Mapping** — Relevant technique IDs

---

## Disclaimer

Phexor is built for educational purposes and authorized security research only.
Only analyze emails you have permission to investigate.

---

## Author

**Mohammed Roshan T**
 [LinkedIn](https://linkedin.com/in/mohammed-roshan-t) · [GitHub](https://github.com/MohammedRoshanT) · TryHackMe: r0x404 (Top 5% Global)
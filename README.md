# EWB Phishing URL Awareness Checker

## Overview
Phishing is a trick where criminals send fake links or messages to make people share passwords, card numbers or OTPs. The fake link often looks very similar to a real one. This project is a small Python command-line tool that reads the text of a URL and points out visible warning signs. It never opens the URL, so nothing is visited or sent anywhere.

## Features
- Detects an @ symbol in the URL
- Flags unusually long URLs (over 100 characters)
- Flags IP-address hostnames
- Flags many hostname segments (5 or more)
- Flags punycode-style hostnames (xn--)
- Flags the HTTP scheme
- Loop with quit command: check many URLs in one run
- "Review carefully" summary that counts the warning signs found
- Safe handling of malformed input, so the program does not crash

## Requirements
Python 3.10 or later. No extra packages.

## How to Run
Windows: `python phishing_checker.py`

Mac/Linux: `python3 phishing_checker.py`

Type a sample URL and press Enter. Type `q`, `quit` or `exit` to stop.

## Example Output
```
EWB Phishing URL Awareness Checker
Text checks only - this program does not open URLs.

Enter a sample URL (or type q to quit): http://192.0.2.10/account

Result:
- Uses an IP address instead of a familiar domain.
- Uses HTTP. HTTPS alone does not prove trust.

REVIEW CAREFULLY: 2 warning sign(s) found.
Never enter passwords from unexpected links.

Enter a sample URL (or type q to quit): q
Goodbye! Stay alert online.
```

## Testing
Five sample URLs were tested. The full table is in `tests/test_samples.md`.

| # | Input | Expected | Actual | Result |
|---|-------|----------|--------|--------|
| 1 | https://www.example.com/login | No warning signs | No listed warning signs found. This does NOT prove safety. | PASS |
| 2 | http://192.0.2.10/account | IP address + HTTP | Uses an IP address instead of a familiar domain. Uses HTTP. | PASS |
| 3 | http://example.com@sample.invalid/login | @ symbol + HTTP | Contains an @ symbol; inspect carefully. Uses HTTP. | PASS |
| 4 | https://a.b.c.d.e.example.invalid/path | Many hostname segments | Contains many hostname segments/subdomains. | PASS |
| 5 | https://xn--example.invalid/ | Punycode-style hostname | Contains a punycode-style hostname. | PASS |

Extra checks: pressing Enter with no input shows "No URL entered.", and typing `http://[bad` shows "This text is not a valid URL format." instead of crashing.

## Screenshots
![Sample run](screenshots/sample_run.png)
![Test results](screenshots/test_results.png)

## Improvements
1. **Loop with quit** - The original program checked one URL and then closed. I replaced the single `input()` with a `while True` loop that keeps asking until the user types `q`, `quit` or `exit`. This makes testing faster because all five samples can be run in one session. I tested it by checking several URLs in a row and then quitting.
2. **Review carefully summary** - I added a helper function, `has_warnings()`, that tells real warnings apart from information messages such as "No URL entered". When real warnings are found, the program prints `REVIEW CAREFULLY` and the number of warning signs. I tested it with the five samples: the first one (no warnings) shows no summary, and the others show the correct count.
3. **Handling malformed input** - Typing `http://[bad` used to crash the program with a `ValueError`. I wrapped the parsing step in `try / except` so the program now prints a clear message and continues. I tested it with `http://[bad` and then re-ran all five sample tests to confirm nothing broke.

## Limitations
This is an educational tool. It only reads URL text. It does not visit URLs, scan for malware or prove that a URL is safe. No warning signs found does NOT mean a URL is safe. A link with zero clues can still be dangerous (for example, a name with a slightly wrong spelling), and a link with clues can be harmless. A URL typed without a scheme (like `www.example.com`) is treated as `http://`, so it triggers the HTTP warning.

## Ethical Use
Use sample strings only. Do not collect credentials. Do not use this project to deceive anyone or to test websites without permission.

## Author
Aaditya Tyagi, Ai powered Cybersecurity, EWB Courses

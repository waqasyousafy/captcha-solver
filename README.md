# Captcha Bypass

A small Python automation project for detecting and handling CAPTCHA-protected pages in browser-driven workflows. The repository includes a browser-based URL review script, CAPTCHA detection patterns, and an audio-based reCAPTCHA solver built on top of DrissionPage.

## Overview

This project is designed for browser automation experiments and internal QA workflows. It can:

- open a page with a Chromium browser
- scan the page HTML for CAPTCHA markers
- detect common providers such as Google reCAPTCHA, hCaptcha, and Cloudflare Turnstile
- optionally attempt to solve reCAPTCHA challenges using the audio challenge flow
- review a list of URLs from CSV files and summarize which pages are loaded, skipped, or failed

## Repository structure

- `url_review.py` — loads a URL, looks for CAPTCHA markers, and attempts to solve a detected challenge
- `RecaptchaSolver.py` — audio-based solver for Google reCAPTCHA
- `test.py` — sequential CSV workflow for reviewing a batch of URLs
- `csv_report_panel.py` — interactive CSV review panel
- `captcha_handlers.py` — captcha-related helper logic
- `Turnstile.py` — Turnstile-related handling

## Requirements

- Python 3.9+
- Chromium/Chrome installed and available on the system
- The Python dependencies from `requirements.txt`

Install dependencies:

```bash
pip install -r requirements.txt
```

If you are using a Linux system and need audio conversion support for the reCAPTCHA audio workflow, install FFmpeg as well:

```bash
sudo apt-get install ffmpeg
```

## Quick start

Basic usage with a browser session:

```python
from DrissionPage import ChromiumPage
from RecaptchaSolver import RecaptchaSolver

driver = ChromiumPage()
solver = RecaptchaSolver(driver)

driver.get("https://www.google.com/recaptcha/api2/demo")
solver.solveCaptcha()
```

Review a single URL with the helper in `url_review.py`:

```python
from url_review import review_url, create_browser

browser = create_browser()
status, message, duration = review_url(browser, "https://example.com")
print(status, message, duration)
```

## CSV workflow

To review a set of URLs from a CSV file:

```bash
python test.py pages.csv
```

The CSV should include a column named one of the following:

- `url`
- `page`
- `link`
- `website`
- `target`

You can also provide a custom column name with the relevant CLI option if needed. The script will visit matching pages, skip ones that show CAPTCHA markers, and write a reviewed output CSV report.

## Panel workflow

Start the CSV review panel:

```bash
python csv_report_panel.py
```

This tool walks through URLs from a CSV file, shows timings for each page, counts successful versus skipped or failed pages, and saves a summary report.

## Notes

- This project depends on browser automation and may be rate-limited or blocked by target sites.
- Use it only for authorized testing, internal QA, or ethically permitted automation.
- CAPTCHAs are often site- and environment-specific; results can vary across pages, browser versions, and network conditions.

## License

This project is shared for educational and automation research use. Please review the repository license and respect the terms of any target sites or services you interact with.
# captcha-solver

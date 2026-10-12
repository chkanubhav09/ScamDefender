# ScamDefender

A free browser-local scam-prevention suite, combining ScamLens and VERIFACE with a broader set of practical checks. Static HTML/CSS/JavaScript. No accounts, API keys, uploads, backend or paid services.

## Tools

1. Message analyzer: copied ScamLens English TF-IDF logistic model and separate explainable rules.
2. URL inspector: actual host, expected-domain mismatch, userinfo, IP addresses, punycode, shorteners, ports and embedded redirects. Does not visit the supplied destination.
3. QR decoder: bundled jsQR 1.4.0. Raster image and camera decoding; no automatic destination opening.
4. UPI URI inspector: parses payee, claimed name, amount, currency and note; warns on repeated parameters. Does not verify ownership or authorize payments.
5. Offer checker: job/task, loan, investment/crypto, shopping, romance/family, delivery/KYC/utility, charity and tech-support/refund paths.
6. Caller triage: threats, authority claims, digital arrest, secrets, remote access and coerced payment patterns.
7. Video evidence: local VERIFACE file, camera, screen capture and quality-gated live pulse estimate.
8. Email-header inspector: claimed From, Reply-To, Return-Path and Authentication-Results. Pasted headers remain unverified.
9. Filename checker: executable/macro formats, double extensions, archives and direction-control text. Not antivirus.
10. Evidence notebook: temporary by default; explicit save/load/delete in localStorage; local JSON/text export.
11. Six-scenario prevention quiz with explanations.
12. Before-you-act checklist, plus India incident-response guidance and official sources.

## Run

Serve this directory over HTTP for text and file tests:

```sh
python3 -m http.server 8000
```

Camera needs localhost or HTTPS. Screen capture is mainly a desktop-browser feature. Use a single well-lit face and supported video codec. The video tool analyzes at most the first 30 seconds and rejects files above 250 MB. QR images are capped at 15 MB.

GitHub Pages serves a losslessly gzip-packed self-contained `index.html` from `main`. It unpacks locally with DecompressionStream (supported in current Chrome, Edge, Firefox and Safari). All message-model, QR and video application code and data are included; video analysis still loads external MediaPipe assets. Expanded source is available in the delivered source archive. To use expanded source, serve `index-standalone.html` or the modular `index.html` from that archive.

## Privacy

Text, QR images, video frames and notebook inputs stay on the device. No telemetry, ads, upload endpoints or link expansion. Hosting receives ordinary requests. The message model is served with this app. The video tool loads pinned MediaPipe 0.10.21 JavaScript/WASM from jsDelivr and the face landmark model from Google storage when analysis starts; these providers receive asset requests, not footage. No claim of fully offline video support.

Notes are not saved automatically. Browser-saved notes are unencrypted and available to other users of that browser. Explicit deletion removes the saved localStorage notebook, not already-downloaded exports. Do not store OTPs, passwords, PINs, recovery phrases, card details or identity-document numbers. Closing the integrated video tool removes its frame and stops media tracks. QR camera stops on decode, after 60 seconds, when hidden, or on close/navigation.

## Important limits

**No tool proves safety or fraud.** There is no live malware/reputation/DNS/certificate/WHOIS lookup, redirect following, identity verification, payee ownership check, investment/lender registration check, sender authentication or attachment scanning. URL, UPI, offer, caller, header and filename rules are educational and unbenchmarked. They can miss new scams and flag legitimate material. Example domains are reserved and never opened.

### ScamLens benchmark, not a fraud probability

19,090 unique deduplicated English messages from UCI SMS Spam Collection and IMC2025 public smishing reports. 11,434 train / 3,754 validation / 3,902 test; grouping by normalized opening-eight-word templates, seeds 42/43. TF-IDF logistic model, validation-selected threshold 0.80.

Held-out accuracy 92.72%, recall 90.64%, precision 99.85%. 4 false positives / 912 benign, 280 missed positives / 2,990 spam-smishing. Dataset/source/time imbalance, historical benign messages and incomplete campaign grouping limit generalization. This is not measured modern-India fraud accuracy. Non-ASCII, short or low-vocabulary inputs withhold model interpretation. Rules are separate and not benchmarked. Full metrics are in `scamlens-metrics.json`.

### VERIFACE is experimental

Five hand-weighted signals: pulse/spatial agreement, eyes/blinks, frequency spread, high-pass noise spread and temporal landmarks. Not a trained or validated real/fake classifier. Browser-port accuracy, fake recall and precision are unavailable. Pulse can survive face swaps and compression can alter it. Live BPM is an experimental signal estimate, not a medical measurement. Weak estimates must remain withheld. The Python reference's three-original-video pulse error does not validate JavaScript or deepfake detection.

Phone layouts are tested at 320/390 px in Chromium; actual iPhone/Android camera hardware and all browser engines have not been tested. Permissions, codecs, screen capture and WebAssembly support vary.

## Tests

`node tests/unit.cjs` runs the rule/model unit tests. `tests/browser.cjs` uses Playwright and qrcode dev dependencies with locally installed Chrome. It exercises text tools, empty states, real QR decode, XSS rendering, notebook persistence and exports, quiz, checklist, filtering and responsive overflow. `tests/browser-results.json` records the test run. No test inputs contain personal data. Tests are smoke/functional tests, not detector-accuracy benchmarks.

## Credits and terms

Original user-owned projects: ScamLens (`ForgeHacks-ScamLens`) and VERIFACE. No new blanket code license is granted by this merge. Third-party data/library terms remain their own:

- UCI SMS Spam Collection: https://archive.ics.uci.edu/dataset/228/sms%2Bspam%2Bcollection . Preserve `scamlens-uci-license.txt`.
- IMC2025 Smishing Dataset: https://github.com/reportsmishing/Smishing-Dataset-IMC25 and https://doi.org/10.1145/3730567.3764431 . Preserve `scamlens-imc-license.txt`.
- jsQR 1.4.0: https://github.com/cozmo/jsQR , Apache 2.0 (`jsQR-LICENSE.txt`). Copyright retained in source.
- MediaPipe Tasks Vision and face model: served externally under the upstream project's terms.

Safety sources checked 12 October 2026:

- I4C reporting portal and 1930: https://i4c.mha.gov.in/ncrp.aspx
- MHA digital-arrest response and cybercrime portal: https://www.pib.gov.in/PressReleasePage.aspx?PRID=2110809
- NPCI UPI PIN confidentiality: https://www.npci.org.in/complaint-status
- SBI UPI transfer/receive flows: https://sbi.bank.in/web/customer-care/upi1
- FTC tech-support scams: https://consumer.ftc.gov/articles/how-spot-avoid-and-report-tech-support-scams
- FTC task scams: https://www.ftc.gov/news-events/data-visualizations/data-spotlight/2024/12/paying-get-paid-gamified-job-scams-drive-record-losses

ScamDefender is not affiliated with these agencies. Reporting does not guarantee fund recovery. In India, financial cyber fraud can be reported promptly on 1930 and https://cybercrime.gov.in . Contact your bank using a known official route too.

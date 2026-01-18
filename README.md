# Analysis of Nginx/Apache Silent Sanitization (Status 4X009)

## 🔍 Overview
This repository documents a peculiar behavior encountered during a security audit. The target environment (Nginx frontend -> Windows/Apache backend) sanitizes SQLi payloads without dropping the connection, returning a custom error code.

## 🛠 Tech Stack
- **Frontend Layer:** Nginx (Proxy/WAF)
- **Backend Layer:** Windows Server / Apache 2.4.54 / PHP 7.3.33
- **Communication:** mod_fcgid 2.3.9a

## 🧠 The "Hard Nut" (Behavioral Analysis)
The system exhibits "Silent Sanitization":
- **Status:** All payloads (`/**/`, `/*!50000*/`, `%df'`) return `HTTP 200 OK`.
- **Response:** The body contains a consistent JSON error: `{"status_code":"4X009"}`.
- **Analysis:** It appears a "Search & Replace" filter at the proxy layer is wiping malicious characters before the PHP backend processes them.
- **SSL Obstacle:** Direct backend IP access results in `SSL routines::unexpected eof`, indicating strict SNI checks.

## 🚀 Seeking Collaboration
I am looking for expert insights on:
1. **HTTP Request Smuggling**: CL.TE or TE.CL vectors for Nginx -> Apache 2.4 desync.
2. **Advanced Encoding**: Unicode or double-encoding tricks to bypass the `4X009` filter.
3. **SNI Spoofing**: Methods to re-establish a TLS handshake directly with the source IP.

*Disclaimer: For educational and authorized research only. No real target IDs included.*

# Footprinting – UniKL VLE (vle.unikl.edu.my)

## Target: https://vle.unikl.edu.my

### Target Info

| Domain  | unikl.edu.my    |
|---------|-----------------|
| IP      | `113.23.135.20` |
| Server  | nginx           |
| CMS     | Moodle          |
| Country | Malaysia        |

## Footprinting Table

| Step | Tool | Finding | Interpretation | Mitigation |
|------|------|---------|----------------|------------|
| 1 | WHOIS (who.is) | Domain registered under MYNIC Berhad, created 04/07/2003, expires 04/07/2027.<br>Nameservers: dns1.unikl.edu.my (103.148.105.8), dns2.unikl.edu.my (103.148.105.9).<br>All contact info REDACTED FOR PRIVACY. | Domain has been around for 20+ years = legit institution.<br>WHOIS privacy protects contact details.<br>But internal DNS names are visible. | Keep WHOIS privacy enabled.<br>Consider using a CDN to hide DNS structure. |
| 2 | DNS Records (who.is) | A record points to 113.23.135.20.<br>SOA record shows administrator.unikl.edu.my.<br>MX record uses Outlook/Microsoft 365.<br>No AAAA (IPv6) records. | Internal admin email exposed in SOA.<br>Mail handled by Microsoft.<br>No IPv6 support yet. | Hide SOA email.<br>Enable DNSSEC.<br>Add IPv6 support. |
| 3 | WhatWeb (whatweb.net) | Server: nginx.<br>CMS: Moodle.<br>Cookies: MoodleSession, cookiesession1.<br>Email found: afadzli@unikl.edu.my.<br>Security headers present. | Moodle is open-source with known CVEs.<br>Session cookies could be hijacked.<br>Personal email exposed in HTML. | Update Moodle regularly.<br>Set cookie flags Secure + HttpOnly.<br>Remove personal emails. |
| 4 | Manual Check | URL structure shows /login/index.php, /course/index.php, /mod/forum/discuss.php.<br>SSO at cas.unikl.edu.my.<br>Mobile app on Google Play. | Login page is public = brute force risk.<br>SSO is a single point of failure.<br>Mobile app adds attack surface. | Add 2FA.<br>Rate limit login attempts.<br>Audit subdomains. |
| 5 | Overall | Public VLE with Moodle, nginx server, Malaysian hosting.<br>Subdomains: vle.unikl.edu.my, vle2.unikl.edu.my, cas.unikl.edu.my. | Large attack surface.<br>Moodle plugins may be outdated.<br>Subdomains may differ in security. | Regular vulnerability scans.<br>Keep subdomains patched.<br>Implement WAF. |


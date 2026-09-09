# DNS Lab

## Objective

Understand how DNS supports Active Directory and general network services, and practice troubleshooting name-resolution failures.

## Topics

- Forward lookup zones
- A records
- CNAME records
- PTR/reverse lookup concepts
- DNS client configuration
- DNS forwarding
- DNS caching
- Active Directory-integrated DNS

## Useful Tools

```text
nslookup
ipconfig /all
ipconfig /flushdns
ipconfig /registerdns
Resolve-DnsName
```

## Practice Incidents

- [ ] Client has network connectivity but cannot resolve hostnames
- [ ] Client is configured to use the wrong DNS server
- [ ] Domain client cannot locate the domain controller
- [ ] Stale/incorrect DNS record
- [ ] DNS cache causes unexpected behavior

## Troubleshooting Notes

For each incident, record symptoms, hypotheses, tests, evidence, root cause, and resolution under `troubleshooting/`.

# Lele Agent — Security Stack Notes

Knowledge base & tool inventory untuk bug bounty hunting + BSC audit oleh agent **@le-le** (Hermes Agent). Repo ini dokumentasi stack-nya; kode asli ada di repo sumber (lihat referensi).

## Skills ter-install di Hermes agent

| Skill | Sumber | Fungsi |
|---|---|---|
| `bug-bounty-hunting` | compiled dari Agentic-Bug-Hunter + NucleiFuzzer | Workflow recon→hunt→validate→report |
| `bsc-contract-audit` | setyamickala/bsc-contract-audit-skill | Audit BEP-20/NFT mint/lock escrow, DECISION GO/CAUTION/BLOCK |
| `launch-curve-audit` | setyamickala/bsc-contract-audit-skill | Audit bonding curve/IEO, verdict SHIP/HOLD/BLOCK |
| `repo-knowledge` | index 9 repo referensi | Ringkasan fastmint, audit, agent tooling |

## Tool inventory (sandbox Linux amd64, 2026-11-19)

### Core recon & scanning (binary release)
- subfinder 2.17.0 · httpx 1.12.0 · nuclei 3.11.1 (+templates) · katana 1.8.0 · naabu 2.7.0 · ffuf 2.3.0 · gobuster 3.8.2 · nmap (apt)

### Python (pip)
- uro (dedup URL) · arjun (hidden params) · dirsearch (dir brute) · paramspider (wrapper → 0xKayala/ParamSpider) · dnsreaper (wrapper → punk-security/dnsReaper)

### Go (go install, /root/go/bin)
- dalfox (XSS) · waybackurls · hakrawler · subjack (subdomain takeover)

### BSC audit toolkit
- `~/repos/bsc-contract-audit-skill/skills/bsc-contract-audit/scripts/rpc.py` — read-only RPC tools (meta/proxy/call/scan/holders/nbal), TERUJI live ke BSC mainnet via `RPC_URL=https://bsc-rpc.publicnode.com`

## Workflow standard

```
subfinder → httpx → (katana + gau/gauplus + waybackurls + hakrawler) → uro → nuclei
                              ↓
                    hunt manual (26 kelas vuln: IDOR, auth bypass, SSRF, XSS,
                    SQLi, smuggling, cache poisoning, dll) + ffuf/dalfox/arjun
                              ↓
              7-Question Gate (repro? in-scope? impact? dup? PoC?)
                              ↓
              Report impact-first (H1 / Bugcrowd / Intigriti / Immunefi)
```

Untuk BSC: `bsc-contract-audit` (asset) + `launch-curve-audit` (sale mechanism) → `DECISION: GO | CAUTION | BLOCK`.

## Referensi (repo asli)
- awarexone/Agentic-Bug-Hunter · 0xKayala/NucleiFuzzer · setyamickala/bsc-contract-audit-skill · setyamickala/fastmint-fcfs-agentic
- cloudflare/security-audit-skill · anthropics/skills · NVIDIA/auto-ontology · vercel-labs/agent-desktop · meta/astryx

## Aturan keras
- Scope dulu, out-of-scope jangan disentuh. Hanya program bounty resmi.
- Credential attack berhenti sebelum password spray.
- Missing tool = skip & catat, bukan error.

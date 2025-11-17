# DC Medicaid 276/277 FFS Claims – Enhanced Guide

> **Reformatted for clarity** from the official [Companion Guide v1.1 (PDF)]([276-277-v1.1.pdf](https://github.com/kopyk/gainwell-dhcf-companion-guides/blob/main/pdfs/276-277-v1.1.pdf))  
> Dense tables → clean, scannable, interactive

---

## Request Error Codes (TA1/999)

| Code | Level | Description | Action Required |
|------|-------|-------------|-----------------|
| **004** | Interchange | Missing ISA segment | Resubmit with valid ISA |
| **I5** | Functional Group | Invalid GS version | Use GS08 = `005010X212` |
| **TA1** | Ack | Interchange rejected | Fix envelope, resend |
| **999** | Ack | Functional rejection | Review 999 report |

---

## Claim Status Response (277)

| STC01-1 | STC03 | Status | Meaning |
|--------|-------|--------|--------|
| **A0** | | Accepted | Claim received |
| **A3** | E | Denied | Missing NPI |
| **A7** | P | Pended | Needs medical review |
| **F2** | | Finalized | Paid in full |

---

## Acknowledgement & Error Flow

```mermaid
graph TD
    A[Submit 276<br/>Claim Status Request] --> B{Valid Envelope?}
    B -->|Yes| C[Process 276]
    B -->|No| D[TA1 Rejection<br/>Code: 004, I5]
    C --> E[999 Functional Ack]
    E --> F[Generate 277 Response]
    F --> G{Claim Status}
    G -->|Accepted| H[STC: A0]
    G -->|Denied| I[STC: A3 + E]
    G -->|Pended| J[STC: A7 + P]

    style D fill:#ffebee,stroke:#d32f2f,color:#b71c1c
    style H fill:#e8f5e9,stroke:#388e3c
    style I fill:#fff3e0,stroke:#fb8c00
    style J fill:#e3f2fd,stroke:#1976d2

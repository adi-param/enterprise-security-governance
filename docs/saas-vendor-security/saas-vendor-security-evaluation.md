---
blog:
  publish: true
  slug: saas-vendor-security-evaluation
  title: "SaaS Vendor Security Evaluation: A Practical Guide"
  description: How to run a security evaluation when a new SaaS vendor lands on your desk — risk tiering, what to check, reading a SOC 2, Singapore and global compliance, and starter templates.
  date: 2026-06-28
  tags:
    - cybersecurity
    - governance
    - vendor-risk
    - saas
    - tprm
---

# SaaS Vendor Security Evaluation: A Practical Guide

A request lands on your desk: a team wants to start using a new SaaS tool. By the time it reaches you, the decision has usually *already been made* — someone ran a trial, the champion loves it, the budget's been sketched out, and now they just need security to "sign off." You're not being asked to help choose. You're being asked to bless something, or to be the department of no.

That's the real tension in vendor security, and it's worth naming up front. Nobody's onboarding a serious SaaS product in two days — these things take weeks of procurement, legal, and budget back-and-forth. The problem isn't speed. It's that the security review often shows up *late*, after everyone's emotionally committed, with little appetite for "actually, we have some concerns." So the goal of a good evaluation process isn't to slow things down. It's to give a clear, evidence-based, proportionate answer fast enough that you're a partner in the decision, not a roadblock at the end of it.

Let's walk through how to do that well.

> **TL;DR**
> - You're handing a third party your data and a foothold in your environment. Their security is now part of yours.
> - **Tier first.** Depth of evaluation should match data sensitivity × business criticality × access. Don't review a $20/month tool the way you'd review your new payments processor.
> - **Demand evidence, not adjectives.** A SOC 2 Type II report or ISO 27001 certificate beats a beautifully worded questionnaire every time.
> - **Standards exist so you don't reinvent the wheel** — CAIQ, SIG, SOC 2, ISO 27001, and locally MTCS, plus PDPA / MAS obligations if you're in Singapore.
> - **It's not one-and-done.** Re-assess on a cadence tied to tier, and watch for sub-processor and posture changes.
> - Templates at the bottom: a risk-tier table and a copy-paste starter questionnaire.

## Why bother evaluating at all?

If you already do this for a living, you know the answer — but it's worth keeping the *why* sharp, because you'll need to articulate it to the business champion who thinks you're just creating friction.

When you onboard a SaaS vendor, three things happen, often quietly:

1. **You hand over data.** Sometimes it's your own operational data, sometimes it's your customers' personal data, sometimes it's regulated data you're legally responsible for. The vendor becomes a processor (or "data intermediary," in PDPA language) acting on your behalf — but the accountability for that data largely stays with *you*.
2. **You grant access.** Modern SaaS doesn't sit in a corner. It gets an OAuth token into your Google Workspace, a SCIM connection to your identity provider, an API key into your data warehouse, a webhook into your production systems. Each integration is a new path into your environment that you don't fully control.
3. **You inherit their risk.** Their breach becomes your incident. Their downtime becomes your outage. Their sloppy sub-processor becomes your fourth-party exposure. The supply chain is now your attack surface, and the headlines of the last few years — MOVEit, Okta's support-system compromise, the long tail of "we were affected via a third party" disclosures — are all variations on this theme.

On top of the risk, there's the paperwork reality. If you hold a SOC 2 or ISO 27001 yourself, vendor management is an explicit control you'll be audited on. If you process personal data, GDPR and Singapore's PDPA both make you responsible for ensuring your processors protect it — with contracts to prove it. So vendor evaluation isn't optional hygiene; it's frequently a compliance obligation with your name on it.

The point to internalise: **you can outsource the work, but you can't outsource the accountability.**

## Not all vendors are equal: tier before you evaluate

Here's the single most important habit, and the one that separates a sane program from a soul-crushing one. **Tier the vendor before you decide how hard to look.**

Trying to apply the same 300-question assessment to every vendor is how teams burn out and how the business learns to route around security entirely. A digital whiteboard tool that never touches customer data does not warrant the same scrutiny as the platform that will store your customers' identity documents. Both deserve *a* look. They do not deserve the *same* look.

Tier on three axes:

- **Data sensitivity** — what's the most sensitive data this vendor will touch? Public marketing copy? Internal docs? Customer PII? Regulated data (health, financial, government)? Authentication secrets?
- **Business criticality** — if this vendor disappeared tomorrow, what breaks? Nothing? A nice-to-have workflow? A revenue-generating production dependency?
- **Access & integration depth** — does it live entirely in its own world, or does it have standing access into your identity provider, production data, or codebase?

The highest of the three usually sets the tier. A vendor with low data sensitivity but deep production access is still high-risk.

| Tier | Typical profile | Evidence you should require | Reassess |
|------|-----------------|------------------------------|----------|
| **Tier 1 — Critical** | Regulated/customer PII, deep integration, or business-critical availability | SOC 2 Type II **or** ISO 27001 + recent pen-test summary, DPA, sub-processor list, security/architecture review, possibly a live call | Annually + on major change |
| **Tier 2 — Moderate** | Internal/confidential data, some integration, important but not critical | SOC 2 / ISO cert or completed CAIQ/SIG Lite, DPA, trust-center review | Every 1–2 years |
| **Tier 3 — Low** | Public/non-sensitive data, no meaningful integration, easily replaced | Lightweight questionnaire or trust-center self-attestation, basic privacy check | At renewal / on tier change |

Tiering is also what makes the rest of this guide scale from a two-person startup to a regulated enterprise. Everyone tiers. They just calibrate the thresholds and the rigor differently — more on that later.

## What to actually evaluate

Once you know the tier, you know how deep to go. These are the domains worth covering. For Tier 1 you'll want evidence across all of them; for Tier 3 you might only care about the first two.

- **Data handling & residency** — What data will they hold, where is it stored, and in which jurisdictions? Data residency isn't just a compliance checkbox; it determines which laws apply and whether you've got a cross-border transfer problem. (Singapore's PDPA Transfer Limitation Obligation and GDPR's transfer rules both live here.)
- **Identity & access management** — Do they support SSO/SAML and SCIM provisioning? Is MFA enforced, including for *their* staff? Is there meaningful role-based access control, or is it "admin and not-admin"? SSO and SCIM aren't luxuries — they're how you keep control of access at scale and kill it instantly on offboarding.
- **Encryption & key management** — Encryption in transit (TLS 1.2+) is table stakes. At rest matters for sensitive data. For Tier 1, ask *who holds the keys* and whether customer-managed keys are an option.
- **Certifications & audits** — SOC 2 Type II, ISO 27001, and the cloud/sector-specific ones below. Certifications are a proxy for "someone independent checked," not a guarantee — but their *absence* on a Tier 1 vendor is a real signal.
- **Sub-processors & fourth-party risk** — Who do *they* rely on? A vendor is only as trustworthy as its own supply chain. You want a published sub-processor list and, ideally, advance notice of changes.
- **Incident response & breach notification** — Do they have an IR plan, and crucially, *what are their breach-notification commitments to you*? In Singapore, PDPA now mandates notification of notifiable breaches; you need contractual timelines that let you meet your own obligations.
- **Availability, BCP & DR** — Published SLA, status page, RTO/RPO targets, and evidence they actually test their disaster recovery. Matters most for business-critical tiers.
- **Secure development** — For vendors building the software themselves: SDLC practices, dependency and vulnerability management, regular penetration testing, and a way to report security issues (a disclosure policy or bug bounty is a healthy sign).
- **Data portability, deletion & exit** — How do you get your data *out*, and how is it destroyed when you leave? Exit planning is the part everyone skips and regrets.
- **Privacy & contractual posture** — Will they sign a Data Processing Agreement / Addendum? Is there a security addendum with breach-notification SLAs, a right to audit, and sub-processor change notice?

## How to evaluate: the lifecycle

Evaluation isn't a one-time gate; it's a loop. A workable lifecycle looks like this:

1. **Intake** — Capture the basics: who's the vendor, what problem do they solve, what data will they touch, what will they integrate with, who's the internal owner. A simple intake form here saves enormous back-and-forth.
2. **Tier** — Apply the table above. This decides everything downstream.
3. **Gather evidence** — Request the artifacts appropriate to the tier: SOC 2 / ISO reports, completed questionnaire, DPA, sub-processor list, pen-test summary. Pull what you can from their trust center before you make a human ask anything.
4. **Review** — Read the evidence critically (especially the SOC 2 — see the next section). Note gaps and concerns.
5. **Risk decision** — Accept, accept-with-remediation, or reject. Document the rationale and who owns the residual risk. "We accepted this risk because…" with a named owner is worth more than any checklist.
6. **Contract** — Bake the requirements into paper: DPA, security addendum, breach-notification SLA, sub-processor notice, right to audit. The contract is where your evaluation gets teeth.
7. **Monitor & reassess** — Watch their trust center and breach disclosures, and re-run the assessment on the cadence tied to the tier. Posture decays; certifications lapse; companies get acquired.

Two principles make this work in practice. **First: evidence over adjectives.** A vendor telling you they take security "very seriously" is worth nothing; an independent SOC 2 Type II report covering the right period is worth a lot. **Second: proportionality.** If your process is the same weight for every vendor, people will route around it — and a process that gets bypassed protects no one.

## The part everyone skips: how to actually read a SOC 2 Type II

Collecting a SOC 2 report and filing it is not reviewing it. If you're going to rely on one, here's what to actually look at — this is where practitioner judgment earns its keep.

- **Type I vs Type II.** A Type I describes controls *at a point in time* (design only). A Type II tests whether they *operated effectively over a period*. For anything beyond Tier 3, you want Type II. Type I is a promise; Type II is a track record.
- **The report period and its freshness.** Check the period the report covers, and the gap between its end date and today. A report covering Jan–Dec but handed to you 11 months later has a stale tail. That's what a **bridge letter** (gap letter) is for — a vendor statement that nothing material changed since the report period. Ask for one if there's a gap.
- **Scope and the Trust Services Criteria.** SOC 2 always covers Security; Availability, Confidentiality, Processing Integrity, and Privacy are *optional*. A report scoped to Security only, when you care about uptime or privacy, doesn't cover what you think it covers. Also check that the *systems in scope* actually include the product you're buying — not just the corporate environment.
- **Exceptions and qualified opinions.** Go to the test results and read the **exceptions** (deviations the auditor found). A clean opinion with a handful of minor, remediated exceptions is normal and even reassuring. A *qualified* opinion, or exceptions clustered around access control or change management, deserves questions.
- **Complementary User Entity Controls (CUECs).** This is the section people miss entirely. CUECs are the controls *you* are responsible for in order for the vendor's controls to actually work — e.g., "the customer is responsible for managing its own user access and enforcing MFA." The SOC 2 is implicitly assuming you do these. If you don't, the assurance partially evaporates. Read the CUECs as *your* to-do list.

Apply the same critical eye elsewhere: an ISO 27001 certificate has a **Statement of Applicability** and a defined scope — check that the scope covers the relevant product and that the cert is current and from an accredited body.

## Standards and questionnaires: don't reinvent the wheel

You are not the first person to ask a vendor about encryption. Lean on the established frameworks and questionnaires.

**Assurance frameworks (what the vendor gets certified/audited against):**

- **SOC 2** — the de facto standard for SaaS in North America and increasingly everywhere. Type II is what you want.
- **ISO/IEC 27001** — international information security management certification; common globally and often expected in Europe and Asia. **ISO/IEC 27701** extends it for privacy.
- **NIST CSF / 800-53** — common in US public sector and as an internal control reference.
- **PCI DSS** — mandatory if they'll touch cardholder data.
- **HIPAA / HITRUST** — relevant for health data (mostly US).

**Questionnaires (what you send the vendor, or pull pre-filled from their trust center):**

- **CAIQ** (Consensus Assessments Initiative Questionnaire, from the Cloud Security Alliance, part of the **STAR** program) — excellent, cloud-specific, and many vendors publish a completed one. A great default starting point.
- **SIG / SIG Lite** (Shared Assessments) — comprehensive; SIG Lite is the trimmed version for lower tiers.
- **VSAQ** (Vendor Security Assessment Questionnaire) — a lighter, open option.

**Trust centers** — many vendors now run a SafeBase, Whistic, Vanta, or Drata trust page where you can self-serve their SOC 2, ISO cert, pen-test summary, sub-processor list, and a completed CAIQ behind a click-through NDA. Always check here first; it can collapse a two-week evidence chase into ten minutes.

A word on **automated security ratings** (SecurityScorecard, UpGuard, Bitsight): they're useful *signal* — externally observable hygiene like expired certs, open ports, and leaked credentials — but they're outside-in guesses, not evidence of internal controls. Treat a bad score as a prompt to dig, not as a verdict, and never treat a good score as a substitute for a real assessment.

### If you're in Singapore (or serving Singapore users)

Global frameworks are necessary but not sufficient here. A few local obligations and standards should be on your radar:

- **PDPA (Personal Data Protection Act 2012)**, enforced by the **PDPC** — Singapore's core data protection law. When a vendor processes personal data on your behalf, they're a **data intermediary**, but the obligations largely stay with *you* as the organisation. Two clauses matter most for vendor work: the **Transfer Limitation Obligation** (personal data sent overseas must get comparable protection — relevant whenever a SaaS vendor stores data outside Singapore) and the **mandatory data breach notification** regime (since the 2021 amendments, notifiable breaches — broadly, those likely to cause significant harm or affecting **500 or more** individuals — must be reported to the PDPC, and to affected individuals). That second one is why your vendor's breach-notification SLA needs to be fast enough for *you* to meet *your* statutory clock.
- **MAS guidelines** — if you're a financial institution (or serving one) regulated by the **Monetary Authority of Singapore**, the **Technology Risk Management (TRM) Guidelines** and the **Outsourcing Guidelines / Notice on Outsourcing** set real expectations for third-party and cloud risk management, due diligence, and audit rights. These materially raise the bar for vendor evaluation in Singapore fintech.
- **MTCS SS 584 (Multi-Tier Cloud Security)** — Singapore's own cloud security standard, with three tiers (Level 1 → 3 for increasingly sensitive workloads). A cloud vendor certified to the appropriate MTCS level is a strong, locally-recognised signal.
- **DPTM (Data Protection Trustmark)** — a PDPC-backed certification that an organisation has sound data protection practices; a nice positive signal from a Singapore-based vendor.
- **Cybersecurity Act (and the CSA)** — most relevant if you operate **Critical Information Infrastructure**, where additional obligations and oversight apply.

The practical rule: pick the frameworks that match **your data subjects and your sector**. EU personal data → GDPR. Singapore personal data → PDPA. Regulated by MAS → TRM + Outsourcing. Cloud workloads in Singapore → look for MTCS. Don't bolt on requirements that don't apply, and don't ignore the local ones because the vendor only waved a SOC 2 at you.

## Small org vs. big org: same principle, different rigor

The framework above is the same whether you're five people or five thousand. What changes is how much machinery you put behind it.

**If you're a small or lean organisation:**

- **Tier ruthlessly.** Your scarcest resource is attention. Spend it on the handful of Tier 1 vendors and wave through the long tail of low-risk tools with a light touch.
- **Lean on what already exists.** Pull the vendor's SOC 2 and completed CAIQ from their trust center. Don't build a 200-question assessment you don't have time to read.
- **Use a lightweight questionnaire** (the one below) for Tier 2, and reserve real depth for Tier 1.
- **Get the contract basics right** — a signed DPA and a sane breach-notification clause cover most of your exposure.
- **Don't build a GRC department.** A shared spreadsheet, a clear intake form, and a consistent habit beat an expensive tool you won't maintain.

**If you're a larger or regulated organisation:**

- **Run a formal TPRM program** with defined intake, tiering, SLAs, and approval workflows.
- **Invest in tooling** — OneTrust, Whistic, Prevalent, or similar — to manage assessments, evidence, and reassessment cadences at scale.
- **Add continuous monitoring** (security ratings, breach feeds) on top of point-in-time assessments, so you learn about posture changes between reviews.
- **Bring legal and procurement in early** for negotiated DPAs, security addenda, right-to-audit clauses, and sub-processor change notice.
- **Hold a real reassessment cadence** and an exception/risk-acceptance register with named owners and expiry dates.

The failure mode for small orgs is *doing nothing* ("we're too small to worry about this"). The failure mode for big orgs is *doing everything to everyone* until the process collapses under its own weight and the business routes around it. Tiering is the cure for both.

## A starter security questionnaire

Here's a tier-aware starter you can copy and adapt. For Tier 3, the first handful is plenty. For Tier 1, treat it as the *opening* set of questions, backed by evidence (don't accept yes/no answers without the report behind them).

```text
Company & scope
1.  What data will you store or process on our behalf, and in which countries/regions is it hosted?
2.  Do you act as a sub-processor for any of our data, and can you provide your current sub-processor list?

Certifications & evidence
3.  Which certifications/audits do you hold (SOC 2 Type II, ISO 27001, MTCS, PCI DSS, etc.), and can you share the current reports/certificates?
4.  When was your most recent independent penetration test, and can you share a summary of results?

Identity & access
5.  Do you support SSO (SAML/OIDC) and automated provisioning (SCIM)?
6.  Is MFA enforced for all of your employees with access to customer data/systems?
7.  How do you implement least-privilege/role-based access internally, and how is access reviewed?

Data protection
8.  Is data encrypted in transit (TLS version?) and at rest? Who manages the encryption keys?
9.  How is our data segregated from other customers' data?
10. On termination, how do we export our data, and how/when is it securely deleted?

Resilience & response
11. What is your SLA, and what are your RTO/RPO targets? Do you test your DR plan?
12. Do you have an incident response plan, and what is your breach-notification commitment to customers (timeline)?

Privacy & contract
13. Will you sign our Data Processing Agreement / security addendum?
14. How do you handle cross-border data transfers and applicable data protection laws (GDPR/PDPA as relevant)?
15. Do you have a vulnerability disclosure policy or bug bounty program?
```

## FAQs

**The vendor won't share their SOC 2 report. Red flag?**
Not necessarily — many will only share under NDA, which is reasonable. A blanket refusal to share *anything*, even under NDA, on a Tier 1 vendor is the actual red flag. At minimum, expect a SOC 3 (the public-facing version) or a completed CAIQ.

**It's a tiny startup with no certifications. Can we still use them?**
Yes — early-stage vendors often haven't been through a SOC 2 yet, and that alone isn't disqualifying. Compensate: dig deeper on the questionnaire, look for *evidence of good practices* (SSO support, a security page, a disclosure policy, sensible architecture), tighten the contract, and tier accordingly. The question is whether the risk is *understood and accepted*, not whether they have a badge.

**It's a free tool a team picked up themselves. Procurement never saw it. Do we evaluate?**
This is shadow IT, and it's where a lot of real risk hides. Yes — if it touches company data or integrates with your systems, it needs at least a light review, regardless of who pays (or doesn't). "Free" often means *you* are the product and your data is the currency.

**What about their sub-processors — the vendor's vendors?**
Fourth-party risk is real. You can't assess every one, but you should require a published sub-processor list, advance notice of changes, and contractual flow-down of your key requirements. If a Tier 1 vendor relies on a sub-processor for something sensitive, look at that relationship too.

**How often should we re-assess?**
Tie it to tier: Tier 1 annually (plus on major changes — acquisition, breach, big architecture shift), Tier 2 every one to two years, Tier 3 at renewal or when its tier changes. Continuous monitoring fills the gaps between formal reviews.

**A completed questionnaire means they're secure, right?**
No. A questionnaire is *self-attestation* — useful for structure and for catching obvious gaps, but it's the vendor grading their own homework. Independent evidence (SOC 2 Type II, ISO 27001, pen-test results) is what turns claims into assurance.

## If you do nothing else

You won't always have time for the full treatment. When you don't, these five cover most of the risk:

1. **Tier it.** Decide if this is a big deal or a small one before you spend any effort.
2. **Get one piece of independent evidence** — a SOC 2 Type II or ISO 27001 — for anything that touches sensitive data.
3. **Sign a DPA** with a breach-notification SLA fast enough to meet your own (PDPA/GDPR) obligations.
4. **Enforce SSO + MFA** on the integration so you control access and can kill it instantly.
5. **Write down the decision** — what you accepted, why, and who owns the residual risk.

Here's what I'd actually tell a team that's eager to onboard something new: I'm not here to say no. I'm here to make sure that when this vendor inevitably has a bad day, *we* don't have a bad year. Tier it, get the evidence, paper the contract, and move on. Do that consistently and vendor security stops being the thing that slows deals down — and becomes the quiet reason none of them blow up later.

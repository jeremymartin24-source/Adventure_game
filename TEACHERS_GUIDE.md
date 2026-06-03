# Professor Martin's Network Security Adventure
## Teacher's Guide & Answer Key
### Network+ Modules 12.4 & 13.2 — IT Security Operations

---

## Table of Contents
1. [Simulation Overview](#1-simulation-overview)
2. [Learning Objectives](#2-learning-objectives)
3. [How to Assign & Grade](#3-how-to-assign--grade)
4. [Scoring Summary](#4-scoring-summary)
5. [Scene-by-Scene Answer Key](#5-scene-by-scene-answer-key)
6. [Branching Paths & Consequence Scenes](#6-branching-paths--consequence-scenes)
7. [Infrastructure Status Flags](#7-infrastructure-status-flags)
8. [Common Student Mistakes](#8-common-student-mistakes)
9. [Discussion Questions](#9-discussion-questions)
10. [Technical Reference for Instructors](#10-technical-reference-for-instructors)

---

## 1. Simulation Overview

This is a single-file, browser-based cybersecurity simulation requiring no installation or backend. Students play the role of a junior sysadmin at Meridian University, responding to a credential breach and hardening the network authentication infrastructure over a 6-week story arc.

**Scenario:** A flat-file, unsalted password database was exfiltrated over the summer. Students must stand up RADIUS/802.1X, detect and respond to a rogue access point, manage PKI operations, configure VLAN authorization, and remediate shadow IT — all before fall semester.

**Mechanics:**
- Simulated **terminal environments** (typed commands, regex-matched output)
- **Config file editors** with fill-in-the-blank fields
- **Log viewer** with click-to-flag rows
- **Drag-and-drop sequence ordering**
- **SIEM alert triage** cards
- **Multiple-choice decision scenes** with branching consequences

**Rules students are shown at the start:**
- Answers are final once submitted
- 3 free hints; each additional hint costs −1 point
- A Skip button is available (forfeits unearned points)
- The debrief report is the graded artifact (screenshot to submit)

---

## 2. Learning Objectives

| Module | Objective | Scene(s) |
|--------|-----------|---------|
| 12.4 | Identify weaknesses in flat-file / unsalted credential storage | T1 |
| 12.4 | Select centralized AAA (RADIUS + AD) over ad-hoc solutions | S2 |
| 12.4 | Configure a RADIUS client definition (clients.conf) | T3 |
| 12.4 | Verify RADIUS authentication with radtest | T4 |
| 13.2 | Detect rogue APs / evil-twin attacks in wireless auth logs | T5 |
| 13.2 | Validate certificates and identify forged trust chains | T6 |
| 13.2 | Execute a complete evil-twin incident response | S7 |
| 13.2 | Order the 802.1X supplicant/authenticator/server/store flow | T8 |
| 13.2 | Revoke a certificate and publish an updated CRL | T9 |
| 13.2 | Assign VLANs via RADIUS tunnel attributes (NPS policy) | T10 |
| 13.2 | Triage SIEM alerts including rogue-CA injection | S11 |
| 13.2 | Recognize and explain MFA fatigue / push-bombing | T12 |
| 13.2 | Communicate defense-in-depth architecture to stakeholders | S13 |
| 13.2 | Discover shadow IT devices via network scanning | T14 |
| 13.2 | Remediate shadow IT by integrating into managed 802.1X | S15 |

---

## 3. How to Assign & Grade

### Deployment
The simulation is a single file (`index.html`) deployable to any static host or GitHub Pages. Students open the URL in any modern browser — no login, no install.

### Submission
At the end, students receive a **debrief report** with their name, score, grade, and per-scene breakdown. They screenshot this page and submit it per your normal assignment workflow.

### Grading Scale

| Grade | Score |
|-------|-------|
| A | ≥ 90% |
| B | ≥ 80% |
| C | ≥ 70% |
| D | ≥ 60% |
| F | < 60% |

### Grading from the Debrief Screenshot
The debrief shows:
- **Overall score and letter grade**
- **Raw score** and any hint penalty deducted
- **Per-scene breakdown** (what they ran / chose, score per scene)
- **Topic mastery bars** across 5 knowledge areas
- **Learning objective status** (Met / Not Met) for all 11 objectives
- **Infrastructure status flags** (6 boolean outcomes tied to correct technical actions)

---

## 4. Scoring Summary

The debrief calculates points across all visited scenes. Students who take the optimal path through all 15 main scenes can earn the following:

| Scene | Type | Max Score | What Earns Full Credit |
|-------|------|-----------|----------------------|
| T1 | Terminal | 3 commands / 3 | Run all 3 key commands |
| S2 | Decision | 2 / 2 | Choose RADIUS + AD |
| T3 | Config | 4 blanks / 4 | All 4 blanks correct |
| T4 | Terminal | 1 command / 1 | Run radtest with correct syntax |
| T5 | Log Viewer | 2 / 2 | Flag both rogue entries, no extras |
| T6 | Terminal | 4 commands / 4 | Run all 4 openssl commands |
| S7 | Decision | 2 / 2 | Choose full incident response |
| T8 | Sequence | 2 / 2 | Correct order first try |
| T9 | Terminal | 3 commands / 3 | Run revoke + gencrl (+ inspect) |
| T10 | Config | 4 blanks / 4 | All 4 blanks correct |
| S11 | Triage | 2 / 2 | Correctly classify the rogue-CA alert |
| T12 | Terminal | 4 commands / 4 | Run all 4 grep/tail commands |
| S13 | Decision | 2 / 2 | Choose the defense-in-depth framing |
| T14 | Terminal | 2 commands / 2 | Run nmap + ping |
| S15 | Decision | 2 / 2 | Choose managed 802.1X integration |

> **Note on terminal scoring:** The debrief records *number of key commands discovered* out of the total available, not raw command points. Students can earn partial credit by running some but not all commands in a terminal scene.

> **Hint penalty:** Deducted from the raw total at the end. Visible on the debrief as "hint penalty: −N pts."

> **Skipped scenes:** Recorded as "(skipped)" in the debrief with whatever partial credit the student had accumulated before skipping.

---

## 5. Scene-by-Scene Answer Key

---

### T1 — Breach Forensics
**Type:** Terminal | **Day 1 | Module 12.4**

**Context:** Student inherits a forensic image of the breached credential store.

**Goal:** Discover the scope and root cause of the breach.

#### Correct Commands (3 key commands)

| Command | Points | What It Reveals |
|---------|--------|-----------------|
| `cat breach-report.txt` | 1 | 47 accounts compromised; flat-file, unsalted SHA-256 |
| `grep -c COMPROMISED auth.log` | 2 | Confirms 47 COMPROMISED export events |
| `cat users.txt` | 1 | Salt column is NONE for every account — confirms no salting |

> `ls` and `help` are available but not scored.

**Key Concepts Being Tested:**
- Why unsalted hashes are dangerous (identical passwords → identical hashes → rainbow table attacks)
- Reading forensic log files
- Using `grep -c` to count pattern occurrences

**Continues to:** S2 (regardless of score)

---

### S2 — Architecture Recommendation
**Type:** Decision | **Day 2 | Module 12.4**

**Context:** Dana needs a recommendation for the replacement authentication system before students return.

| Option | Verdict | Score |
|--------|---------|-------|
| A. Third-party cloud login portal | Bad | 0/2 |
| **B. RADIUS (FreeRADIUS/NPS) integrated with Active Directory** | **Good** | **2/2** |
| C. Custom in-house authentication system | Bad | 0/2 |

**Why B is correct:** RADIUS provides centralized Authentication, Authorization, and Accounting (AAA). It reuses the existing Active Directory identity store, avoids creating a new credential silo, and is the standard backbone for 802.1X wireless authentication. This is the textbook enterprise pattern for Module 12.4.

**If wrong:** Students are routed to S2_fail (narrative consequence) and then continue to T3.

**Key Concepts Being Tested:**
- AAA (Authentication, Authorization, Accounting)
- RADIUS as the industry-standard centralized AAA for 802.1X
- Why custom auth systems and external silos are riskier

---

### T3 — FreeRADIUS clients.conf
**Type:** Config Editor | **Day 2 | Module 12.4**

**Context:** Student fills in the FreeRADIUS `clients.conf` to define the campus AP controllers as RADIUS clients.

**All values are given in the scenario narrative (runbook).**

#### Correct Answers (4 blanks)

| Field | Correct Value | Notes |
|-------|--------------|-------|
| `ipaddr` | `10.0.0.0/8` | AP management subnet in CIDR notation |
| `secret` | `MeridianSecret2024` | Shared secret (8+ chars, no spaces) — any valid strong secret accepted |
| `shortname` | `meridian-aps` | Any alphanumeric label (letters, numbers, hyphens, underscores) |
| `nastype` | `cisco` | `cisco` or `other` both accepted |

> The `shortname` field accepts any valid identifier — students are not penalized for using a different label like `campus-aps`.

**If all 4 blanks correct:** Routes to T4  
**If any blank wrong:** Routes to T3_fail narrative → T3_fix (guided retry with values pre-filled)

**Key Concepts Being Tested:**
- RADIUS client definition syntax
- The role of the NAS (Network Access Server) shared secret
- CIDR subnet notation

---

### T4 — radtest Verification
**Type:** Terminal | **Day 2 | Module 12.4**

**Context:** Verify the RADIUS deployment actually authenticates users.

#### Correct Command

```
radtest alice.test Pass123 127.0.0.1 0 MeridianSecret2024
```

Any command matching the pattern `radtest <user> <pass> <server> <port> <secret>` is accepted (5 arguments after `radtest`).

**Expected Output:** `Received Access-Accept` with VLAN 20 tunnel attributes.

**Sets gameState flag:** `radiusWorking = true`

**Key Concepts Being Tested:**
- radtest syntax: `radtest <user> <password> <radius-server> <nas-port> <shared-secret>`
- Interpreting an `Access-Accept` response
- VLAN assignment via tunnel attributes in the radtest response

---

### T5 — Spot the Rogue Access Point
**Type:** Log Viewer | **Week 1 | Module 13.2**

**Context:** A TA reports connecting to a suspicious "Meridian-WiFi" network. Student reviews wireless auth logs.

#### The Two Suspicious Entries (click to flag)

| Timestamp | Source | Why It's Suspicious |
|-----------|--------|-------------------|
| `09:11:19` | AP **192.168.50.77** | IP is outside the managed 10.0.x.x range; uses **EAP-PEAP** instead of EAP-TLS |
| `09:17:42` | AP **192.168.50.77** | Same rogue AP; accepts **MSCHAPv2** (transmits credentials to AP) |

**All legitimate entries** are from APs on `10.0.x.x` and use `EAP-TLS`.

| Score | Condition |
|-------|-----------|
| 2/2 | Both rogue entries flagged, ≤2 false positives |
| 1/2 | One rogue entry flagged |
| 0/2 | No rogue entries flagged |

> False positives > 2 reduce the score by 1.

**Sets gameState flag:** `evilTwinCaught = true` (if score ≥ 1)

**If score 0:** Routes to T5_miss (consequence narrative) before T6.

**Key Concepts Being Tested:**
- Evil twin / rogue AP detection
- IP address anomalies (out-of-range source)
- EAP method downgrade attack (EAP-TLS vs. PEAP/MSCHAPv2)
- Why MSCHAPv2 exposes credentials but EAP-TLS does not

---

### T6 — Inspect the Rogue Certificate
**Type:** Terminal | **Week 1 | Module 13.2**

**Context:** Certificate captured from the rogue AP. Student must use `openssl` to analyze it and compare against legitimate infrastructure.

#### Correct Commands (4 key commands)

| Command | Points | What It Shows |
|---------|--------|--------------|
| `openssl x509 -in rogue-ap.pem -text -noout` | 2 | **Issuer is ACME-TestCA** (not Meridian CA) but **Subject claims CN=radius.meridian.edu** — forgery confirmed |
| `openssl verify -CAfile meridian-ca.crt rogue-ap.pem` | 1 | Verification fails — rogue cert doesn't chain to Meridian CA |
| `openssl x509 -in legitimate-radius.pem -text -noout` | 1 | Legitimate cert shows Issuer = CN=Meridian-IssuingCA |
| `openssl verify -CAfile meridian-ca.crt legitimate-radius.pem` | 1 | Returns `OK` — legitimate cert chains correctly |

> **Minimum required:** At least `x509_rogue` (inspect the rogue cert) to continue.

**Key Finding:** The rogue cert impersonates `CN=radius.meridian.edu` but is signed by `ACME-TestCA` — a different CA not in the Meridian trust store. Clients properly configured with EAP-TLS would reject it.

**Key Concepts Being Tested:**
- `openssl x509 -in <file> -text -noout` output interpretation
- Issuer vs. Subject in X.509 certificates
- Certificate chain validation with `openssl verify -CAfile`
- How evil twins use forged certs and why EAP-TLS client-side validation defeats this

---

### S7 — Respond to the Evil Twin
**Type:** Decision | **Week 1 | Module 13.2**

**Context:** Rogue AP confirmed. Marcus Chen's credential was captured. Student must choose the incident response.

| Option | Verdict | Score |
|--------|---------|-------|
| A. Change the SSID password | Bad | 0/2 |
| **B. Reset Marcus's account + physically remove rogue AP + migrate to EAP-TLS** | **Good** | **2/2** |
| C. Block 192.168.50.77 at the firewall | Partial | 1/2 |

**Why B is correct:** The response must address three distinct problems simultaneously:
1. **Compromised identity** → disable/reset Marcus's account
2. **Active threat** → physically locate and remove the rogue AP (a firewall block doesn't stop wireless harvesting)
3. **Root cause** → migrate to EAP-TLS so there's no harvestable password even if a client connects to a fake AP

**Key Concepts Being Tested:**
- Incident response: containment (account), eradication (physical removal), recovery (EAP-TLS)
- Why firewalling a wireless rogue AP is insufficient
- Why 802.1X with EAP-TLS (certificate auth) eliminates password-harvesting attacks

---

### T8 — Order the 802.1X Authentication Flow
**Type:** Sequence | **Week 2 | Module 13.2**

**Context:** Student orders the four roles in an 802.1X authentication exchange.

#### Correct Order

```
1. Supplicant (student's laptop)
2. Authenticator (Wi-Fi access point)
3. Authentication Server (RADIUS / NPS)
4. Identity Store (Active Directory)
```

**Explanation:** The supplicant initiates the EAP identity request → the authenticator (AP) relays it without making any decision → RADIUS validates the credential/certificate and issues Access-Accept or Reject → AD confirms the account and returns group membership and VLAN attributes. The AP only opens the port after RADIUS says yes.

| Score | Condition |
|-------|-----------|
| 2/2 | Correct order on first attempt |
| 0/2 | Incorrect order |

**If wrong:** Routes to T8_explain (narrative review) before T9.

**Key Concepts Being Tested:**
- The four roles in 802.1X: supplicant, authenticator, authentication server, identity store
- Why the authenticator relays but does not decide
- Port-based access control (port stays closed until Access-Accept)

---

### T9 — Revoke a Stolen Certificate + Publish CRL
**Type:** Terminal | **Week 3 | Module 13.2**

**Context:** A laptop with a valid EAP-TLS client certificate was stolen. Student must revoke the cert and publish a new CRL.

#### Required Commands (both must be run to pass)

| Command | Points | Effect |
|---------|--------|--------|
| `openssl ca -revoke stolen-laptop.pem -keyfile issuing-ca.key -cert issuing-ca.crt` | 2 | Marks serial 010000001A revoked in the CA database |
| `openssl ca -gencrl -out meridian.crl -keyfile issuing-ca.key -cert issuing-ca.crt` | 2 | Generates an updated CRL |
| `openssl crl -in meridian.crl -text -noout` *(optional)* | 1 | Inspects the CRL to confirm revocation |

> **Both `revoke` and `gencrl` are required to pass.** Running only one routes to T9_fail.

**Sets gameState flags:**
- `certRevoked = true` (if `revoke` command run)
- `crlPublished = true` (if `gencrl` command run)

**Key Concepts Being Tested:**
- Two-step revocation: revoke the cert, *then* regenerate the CRL
- Why a revoked cert without a published CRL is ineffective
- CRL (Certificate Revocation List) vs. OCSP
- `openssl ca` syntax for PKI operations

---

### T10 — NPS Network Policy: Nursing Clinical VLAN
**Type:** Config Editor | **Week 3 | Module 13.2**

**Context:** Student configures a Windows NPS network policy to assign VLAN 30 to nursing students via RADIUS tunnel attributes.

**All values are stated in the scenario narrative.**

#### Correct Answers (4 blanks)

| Field | Correct Value | Notes |
|-------|--------------|-------|
| `Condition.Windows-Groups` | `MERIDIAN\Nursing-Clinical` | Any case variation accepted; `Nursing-Clinical` alone also accepted |
| `Tunnel-Type` | `VLAN` | Also accepts `13` (the RADIUS integer value) |
| `Tunnel-Medium-Type` | `802` | Also accepts `6`, `IEEE-802`, `ieee-802` |
| `Tunnel-Private-Group-ID` | `30` | Must be exactly `30` |

**If all correct:** Routes to S11; sets `vlanFixed = true`  
**If any wrong:** Routes to T10_fail narrative → T10_fix (radtest debug terminal)

**Key Concepts Being Tested:**
- RFC 2868 RADIUS tunnel attributes for VLAN assignment
- The three required attributes: Tunnel-Type (VLAN/13), Tunnel-Medium-Type (802/6), Tunnel-Private-Group-ID (VLAN number)
- NPS Network Policy conditions (Windows-Groups)
- How RADIUS authorization attributes control network segment assignment

---

### S11 — SIEM Alert Triage
**Type:** Triage | **Week 4 | Module 13.2**

**Context:** Student classifies 6 SIEM alerts as Real Threat, False Positive, or Investigate.

#### Correct Classifications

| Alert | Severity | Correct Answer | Explanation |
|-------|----------|---------------|-------------|
| 47 failed logins from 185.220.101.47 in 90 sec | HIGH | **Real Threat** | Classic brute-force / password spray — block and investigate |
| TLS cert for webmail expires in 30 days | LOW | **Investigate** | Not an attack; schedule renewal before it breaks service |
| **New Root CA added to NPS trust store by tech01** | **HIGH** | **Real Threat** ⭐ | **Rogue CA injection — most dangerous alert in the queue** |
| Prof.Chen login at 11 PM from usual workstation | MED | **False Positive** | After-hours from expected device/location is routine |
| TCP port scan on 192.168.1.x | MED | **Real Threat** | Active reconnaissance — investigate and contain source |
| Guest VLAN client using 8.8.8.8 DNS | LOW | **False Positive** | Expected behavior on an isolated guest VLAN |

> ⭐ **The rogue CA alert (row 3) is the critical catch.** Injecting a Root CA into the NPS trust store lets an attacker forge certificates that RADIUS will accept — the same attack vector as the evil twin. Correctly identifying this alert sets `rogueCAFound = true` and earns full score (2/2).

**Scoring:**
| Score | Condition |
|-------|-----------|
| 2/2 | Rogue CA alert correctly classified as Real Threat |
| 0–1 | Based on total correct, capped at 1 if rogue CA missed |

**Key Concepts Being Tested:**
- Distinguishing high-signal alerts from operational noise
- Rogue CA injection as a trust-chain attack
- Why an insider-modified trust store is more dangerous than a failed login
- Alert fatigue and false positive management

---

### T12 — MFA Fatigue Attack Analysis
**Type:** Terminal | **Week 5 | Module 13.2**

**Context:** Marcus Chen's account (compromised in the evil twin) reports push notification spam. Student analyzes Duo MFA logs.

#### Correct Commands (4 key commands)

| Command | Points | What It Reveals |
|---------|--------|-----------------|
| `grep marcus.chen duo.log` | 1 | All push requests from 185.220.101.47 (same hostile IP as brute-force alert) |
| `grep -c DENY duo.log` | 2 | **47 denials** — textbook MFA fatigue / push-bombing pattern |
| `grep APPROVED duo.log` | 2 | **One APPROVED at 02:17:33** — the breach moment (exhausted user approved) |
| `tail duo.log` (or `tail -n X duo.log`) | 1 | Last entries show the fatigue crescendo ending in approval |

> **Minimum required:** `grep_approved` OR `grep_deny` to continue.

**Key Finding:** After 47 push denials, a half-asleep Marcus tapped "Approve" at 2:17 AM, granting access to the attacker. This is the textbook definition of an MFA fatigue / push-bombing attack.

**Key Concepts Being Tested:**
- MFA fatigue: attacker hammers push notifications hoping for an exhausted approval
- Using `grep -c` to count events, `grep PATTERN` to find specific events
- Why number-matching MFA (where users must enter a code, not just tap) defeats this attack
- Correlation: same source IP (185.220.101.47) appears in T5, S11 brute-force, and T12

---

### S13 — Explain the Architecture to the Board
**Type:** Decision | **Week 5 | Module 13.2**

**Context:** The university board wants a plain-English explanation of the PKI + RADIUS + MFA layered system.

| Option | Verdict | Score |
|--------|---------|-------|
| A. "Three security products that together make us more secure." | Bad | 0/2 |
| **B. "PKI proves identity (no password to steal); RADIUS checks identity against AD and decides access; MFA backstops the human factor."** | **Good** | **2/2** |
| C. Walk through EAP-TLS packet-by-packet and X.509 ASN.1 structure | Partial | 1/2 |

**Why B is correct:** Defense-in-depth requires each layer to answer a specific question:
- **PKI (certificates):** proves identity without a transmittable password
- **RADIUS:** centralizes the access decision against a directory; returns authorization attributes
- **MFA:** backstops the credential even if phished

**Key Concepts Being Tested:**
- Defense in depth as layered complementary controls
- Communicating security architecture to non-technical stakeholders
- Matching technical explanation depth to audience

---

### T14 — Scan the Nursing Center Network
**Type:** Terminal | **Week 6 | Module 13.2**

**Context:** Unverified "new Wi-Fi" reported at an off-campus nursing center. Student discovers the scope.

#### Correct Commands (2 key commands)

| Command | Points | What It Finds |
|---------|--------|---------------|
| `nmap -sV 192.168.10.0/24` (or any `nmap` command) | 2 | 12 unmanaged APs broadcasting SSID "NursingNet" on WPA2-Personal (PSK) — none in managed inventory |
| `ping 192.168.10.1` | 1 | Unmanaged gateway responds — confirms live shadow infrastructure |

> Any `nmap` command is accepted (pattern match on `nmap`).

**Key Finding:** 12 access points using WPA2-Personal (shared passphrase) with no 802.1X, not in the managed AP inventory, co-located with clinical systems handling sensitive data.

**Key Concepts Being Tested:**
- Using nmap for network inventory and device discovery
- WPA2-Personal vs. WPA2-Enterprise (802.1X) authentication models
- Shadow IT: unauthorized infrastructure built to meet a legitimate need

---

### S15 — The Nursing Center's Shadow Wi-Fi (Capstone)
**Type:** Decision | **Week 6 | Module 13.2**

**Context:** 12 unmanaged WPA2-PSK APs near clinical systems, installed by staff who got tired of waiting for IT. Final decision of the simulation.

| Option | Verdict | Score |
|--------|---------|-------|
| **A. Integrate the nursing center into managed 802.1X/EAP-TLS infrastructure** | **Good** | **2/2** |
| B. Block the shadow APs at the firewall | Partial | 1/2 |
| C. Leave it isolated — not your department | Bad | 0/2 |

**Why A is correct:** Shadow IT exists because a legitimate need wasn't met. The correct response addresses the need *and* brings the infrastructure under secure management — replacing unmanaged WPA2-PSK APs with managed units under 802.1X/EAP-TLS, with the same central monitoring as the rest of campus. Simply blocking without a replacement strands users and guarantees they'll build something worse.

**Key Concepts Being Tested:**
- Shadow IT root cause: unmet legitimate need
- Why firewall blocks without managed replacements are half-solutions
- WPA2-Enterprise (802.1X) as the appropriate replacement for WPA2-Personal in an institutional context
- Governance: bringing infrastructure under policy and monitoring

---

## 6. Branching Paths & Consequence Scenes

These scenes are not scored but carry narrative consequences and may include additional learning moments.

| Scene | Triggered By | Content |
|-------|-------------|---------|
| **S2_fail** | Wrong choice in S2 | Cloud portal suffers breach; custom build stalls. Returns to T3. |
| **T3_fail** | Any blank wrong in T3 | Entire campus network goes offline from RADIUS misconfiguration. |
| **T3_fix** | After T3_fail | Guided retry with all values pre-filled — confirms correct values. |
| **T5_miss** | 0 flags correct in T5 | Evil twin ran 3 more days; credentials captured from dozens of clients. |
| **T8_explain** | Wrong sequence in T8 | Step-by-step 802.1X flow walkthrough before T9. |
| **T9_fail** | Only revoke or only gencrl | Stolen laptop authenticates 8 more times; explains why both steps are needed. |
| **T10_fail** | Any blank wrong in T10 | 40 nursing students locked out of clinical software during graded practical. |
| **T10_fix** | After T10_fail | radtest debug terminal to confirm wrong VLAN is being returned. |

---

## 7. Infrastructure Status Flags

The debrief shows 6 boolean flags reflecting whether students completed critical technical actions correctly. These provide a quick at-a-glance check for instructors.

| Flag | Set When | Indicates |
|------|----------|-----------|
| ✓ RADIUS authentication | radtest returns Access-Accept (T4) | Student verified end-to-end AAA |
| ✓ Evil twin detected | Score ≥ 1 on T5 | Student identified the rogue AP |
| ✓ Stolen cert revoked | `openssl ca -revoke` run in T9 | Step 1 of revocation complete |
| ✓ CRL published | `openssl ca -gencrl` run in T9 | Step 2 of revocation complete |
| ✓ Nursing VLAN correct | All 4 blanks correct in T10 | VLAN authorization configured properly |
| ✓ Rogue CA caught | Rogue CA alert classified as threat in S11 | Student recognized the trust-chain attack |

---

## 8. Common Student Mistakes

### T1 — Breach Forensics
- Running `ls` but not reading the files (ls is not scored)
- Trying `grep COMPROMISED auth.log` without `-c` (still scores — matches the regex)

### S2 — Architecture
- Choosing the cloud portal (fastest ≠ most secure or most appropriate)

### T3 — clients.conf
- Typing the IP without CIDR notation (`10.0.0.0` instead of `10.0.0.0/8`) — the validator accepts both, so this is fine
- Leaving blanks empty and clicking Apply without filling them in

### T5 — Log Viewer
- Flagging all entries "just to be safe" — false positives > 2 reduce the score
- Missing the second suspicious row (the WARN at 09:17:42 showing cleartext MSCHAPv2)
- Flagging legitimate EAP-TLS entries from 10.0.x.x APs

### T6 — Certificate Inspection
- Only running the `openssl x509` command on the rogue cert and missing the `verify` commands
- Confusing Issuer and Subject — the forgery is in the Issuer field (ACME-TestCA)

### T9 — Certificate Revocation
- Running only `openssl ca -revoke` without the `gencrl` step (routes to T9_fail consequence)
- Running `gencrl` first (order matters for the minimum-commands check — both must be run)

### T10 — VLAN Policy
- Entering `20` instead of `30` for Tunnel-Private-Group-ID (confusing the staff VLAN with clinical)
- Entering the full `MERIDIAN\Nursing-Clinical` with extra spaces — trim whitespace

### S11 — SIEM Triage
- Classifying the rogue-CA injection as "Investigate" instead of "Real Threat" (the most common miss)
- Treating the professor's late-night login as a threat (no anomalous indicators)

### T12 — MFA Fatigue
- Running `grep marcus.chen duo.log` but missing the `grep APPROVED` command that shows the actual breach
- Not connecting the 185.220.101.47 source IP to the brute-force alert from S11

### S13 — Board Communication
- Choosing option C (the technical deep-dive) — students who understand the content sometimes over-explain to the wrong audience

---

## 9. Discussion Questions

These are suitable for post-simulation debrief or written reflection:

1. **T1/T3:** What specific property of unsalted SHA-256 hashes makes offline cracking trivial? How does salting mitigate this?

2. **T5:** An evil twin needs to downgrade clients from EAP-TLS to PEAP/MSCHAPv2 to harvest credentials. Why? What would have happened if the client had strict EAP-TLS validation enabled?

3. **T6:** The rogue cert has `Subject: CN=radius.meridian.edu` — the same as the real cert. Why isn't the Subject sufficient for authentication? What makes the Issuer the critical field?

4. **T8:** Why does the authenticator (AP/switch) not make the authentication decision itself? What would be the security and operational implications if it did?

5. **T9:** Revocation without CRL distribution is useless. What are the trade-offs between CRL and OCSP for revocation checking? What would need to change in the infrastructure to enable OCSP?

6. **S11:** The rogue CA injection (new Root CA in NPS trust store) is the most dangerous alert in the queue even though the brute-force appears more dramatic. Explain why infrastructure-level trust manipulation is more severe than a credential attack.

7. **T12:** What specific MFA mechanism would have prevented the push-bombing attack on Marcus Chen? (Answer: number matching, where the user must type a code displayed on-screen rather than just tapping approve.)

8. **S15:** The nursing staff built shadow IT because their need wasn't being met. What process failures contributed to this situation, and how do you prevent recurrence while also fixing the technical problem?

---

## 10. Technical Reference for Instructors

### RADIUS / 802.1X
- **Port:** UDP 1812 (authentication), 1813 (accounting)
- **NAS:** Network Access Server — the device (AP or switch) that sends auth requests to RADIUS
- **Shared secret:** symmetric key between NAS and RADIUS server — used to sign/encrypt traffic
- **EAP-TLS:** mutual certificate authentication — neither side needs to send a password
- **PEAP/MSCHAPv2:** client authenticates with username/password inside a TLS tunnel — password sent to the authenticator
- **Access-Accept:** RADIUS response granting access, can carry authorization attributes (VLAN, session timeout)
- **Access-Reject:** RADIUS response denying access

### PKI
- **Issuer:** the CA that signed the certificate (authenticates the cert's legitimacy)
- **Subject:** the entity the certificate represents
- **CRL:** Certificate Revocation List — a signed file listing revoked serial numbers
- **`openssl ca -revoke`:** marks a cert as revoked in the CA's database
- **`openssl ca -gencrl`:** generates a new CRL file incorporating all revocations
- **OCSP:** Online Certificate Status Protocol — real-time revocation check alternative to CRL distribution

### VLAN Assignment via RADIUS (RFC 2868 Tunnel Attributes)
```
Tunnel-Type             = VLAN (13)
Tunnel-Medium-Type      = IEEE-802 (6)
Tunnel-Private-Group-ID = <VLAN ID as string>
```
All three attributes are required; missing one causes the authenticator to ignore the VLAN assignment.

### radtest Syntax
```
radtest <username> <password> <radius-server> <nas-port-number> <shared-secret>
```
Example: `radtest alice.test Pass123 127.0.0.1 0 MeridianSecret2024`

### openssl Commands Used in Simulation
```bash
# Inspect a certificate
openssl x509 -in <cert.pem> -text -noout

# Verify a cert against a CA
openssl verify -CAfile <ca.crt> <cert.pem>

# Revoke a certificate
openssl ca -revoke <cert.pem> -keyfile <ca.key> -cert <ca.crt>

# Generate an updated CRL
openssl ca -gencrl -out <output.crl> -keyfile <ca.key> -cert <ca.crt>

# Inspect a CRL
openssl crl -in <file.crl> -text -noout
```

---

*Teacher's Guide for Professor Martin's Network Security Adventure — Network+ Modules 12.4 & 13.2*  
*Single-file static simulation — no backend, no student data stored*

# Shared App Build Standards

**Version:** 1.2  
**Reviewed:** 2026-11-09 (Africa/Lagos)  
**Canonical source:** this file in the SaaS Product Build Lab.  
**Applies from day one:** every active standalone app or SaaS build, including new work in other projects. Existing project maps, build plans, and prompts must link to this standard. A product owner can add stricter product-specific controls, but must not weaken these shared rules without an explicit decision record.

These standards describe required practice. They do not certify that an existing app complies. Verify the source and record evidence before making a compliance claim.

## 1. Begin with the existing product

- Read the product brief, project map, build plan, current code, known issues, and handoff before changing anything.
- Search the relevant workspace and canonical repository for existing pages, components, data operations, helpers, and features. Extend the correct existing implementation instead of creating a duplicate.
- Record the user problem, intended users, core outcome, in-scope work, excluded work, assumptions, dependencies, and acceptance criteria before implementation.
- For research-based decisions, record the source, access date, version, and the decision it supports. Label inference separately from sourced fact.
- Identify data, trust boundaries, external services, failure modes, and sensitive actions during planning. Do not connect accounts, publish, send messages, spend money, or create live cloud resources without authorization.

## 2. UI and user experience

- Start from the user's task and information architecture. Keep a clear tree of navigation and use focused screens, routes, dialogs, or pop-ups for distinct tasks. Do not force unrelated work into one crowded page.
- Make page titles, actions, empty states, loading states, saved states, validation errors, network failures, and recovery steps clear.
- Use semantic controls and headings. Label inputs; associate errors with fields; provide useful alternative text for informative images; keep keyboard access and logical focus order; show visible focus.
- Ensure text and controls remain readable on small screens, at increased zoom, and with text resizing. Avoid color-only meaning, motion that cannot be paused where needed, and time limits without a user need.
- Use WCAG 2.2 AA as the baseline for web interfaces. Include its keyboard, contrast, focus-not-obscured, target-size, accessible-authentication, and error-prevention requirements where applicable. A 44 CSS-pixel touch target is a preferred design heuristic where practical; it is not a substitute for checking the WCAG success criterion and its exceptions.
- Keep a product-specific accessibility checklist and record what was checked, with which tools and devices. Automated scans are useful but do not establish full WCAG conformance.
- Do not claim a design is accessible or WCAG-conformant without a documented review of the relevant criteria and interaction paths.

## 3. Code organization and maintainability

- **Screens belong in `pages/`.** Give each major screen or route its own page file.
- **Shared interface pieces belong in `components/`.** Keep page-specific flows in their page and reusable controls in shared components.
- **Saving and loading belong in `data/`.** Put persistence, data access, loading, saving, and storage adapters here; pages call the data layer and components present data and report user actions.
- **One job per file.** Use names that tell a beginner what the file owns. Keep domain rules, validation, types, styles, and integration adapters in appropriately named files.
- **Hard limit: 300 lines per maintained application source file.** This limit applies to code, not documentation, generated files, lockfiles, or vendored dependencies. Count a source file before and after edits. If an existing maintained source file exceeds the limit, do not grow it; plan a behavior-preserving split with the next relevant change.
- Before the first screen or major refactor, sketch the folder and navigation tree in the project map. Keep that map aligned with the actual repository.
- Avoid unnecessary frameworks, dependencies, abstractions, and agents. Record significant architectural choices as short ADRs with context, decision, alternatives, trade-offs, and consequences.

## 4. Security and privacy

- Use OWASP ASVS as the web-application security checklist; select a verification depth appropriate to the app's risk. Use NIST SSDF as the secure-development lifecycle reference.
- Before implementation, record the main assets, sensitive data, trust boundaries, roles, high-impact actions, and plausible abuse or failure cases. Revisit this threat model when scope or integrations change.
- Default to least privilege and deny by default. Enforce authorization for every protected action and record/object, including tenant boundaries where applicable. Authentication does not imply authorization.
- Validate input at the server or trusted boundary; encode output; use safe parameterized data access; protect sessions and state-changing requests; handle file uploads and webhooks as hostile input; use bounded timeouts, retries, and idempotency for integrations.
- Minimize collection and retention. Use synthetic records in local demos, tests, screenshots, and recordings. Do not expose real student, client, contact, financial, health, or private-message data in public evidence.
- Keep API keys, passwords, tokens, private URLs, WhatsApp QR/session material, and recovery codes out of source, issues, documentation, screenshots, and logs. Use environment configuration or a secrets manager. Commit only a clearly fake `.env.example`; ignore real local secret files.
- If a real secret is exposed, stop using it, revoke or rotate it promptly, and record the remediation without copying the secret into the incident record.
- Record only operationally useful logs. Redact secrets and unnecessary personal data; log security-relevant events and failures without recording private message content or credentials.
- Check dependency names, licenses, provenance, and known vulnerabilities before adopting them. Pin versions where practical and record how dependency updates are reviewed.
- Keep standalone app repositories private by default until the owner explicitly approves publication. Before making a repository public, review source, Git history, issues, screenshots, demo data, and metadata for secrets and private information. GitHub security features vary by repository type and plan; do not assume secret scanning or push protection is enabled.
- Never imply that a checklist or static scan proves an app secure. Record the controls reviewed, tools used, uncovered risks, and remaining work.

## 5. Reliability and data integrity

- Name the authority/source of truth for every important piece of state. Avoid silent conflicting writes or success messages that appear before authoritative state is confirmed.
- Define schema, validation, state transitions, and ownership for important records. Use migrations or an equivalent reversible change process when persisted data formats change.
- Specify timeout, retry/backoff, duplicate-event, partial-failure, and recovery behavior for every external integration. Use idempotency where retries could duplicate an action.
- Keep high-impact external effects behind explicit user confirmation and a clear review/audit trail. Local demonstrations must say when actions are simulated.
- Define backup and restore expectations for user-created data; verify restoration before a release or major data change. Do not treat a running server, sync indicator, or successful write request as proof of a restorable backup.

## 6. Work tracking and status

- Give each bounded feature or research batch one issue/task with purpose, scope, acceptance criteria, priority, dependencies, and owner.
- Use the statuses **Planned**, **In progress**, **Review**, **Blocked**, and **Complete**. A task is Complete only when its acceptance criteria and evidence are recorded.
- Keep one canonical task ledger per build. Link related GitHub issues and Drive decisions rather than maintaining competing unsynced lists.
- For each milestone record: what changed, changed files or artifact links, commit SHA/revision, checks run and exact results, checks not run, remaining risk, blocker, and next action.
- Separate source inspection, automated/static checks, runtime/integration checks, visual/accessibility review, deployment, external-user use, and business outcomes. Do not upgrade one evidence level into another.

## 7. Verification and release

- Define verification criteria before implementation. Choose checks proportionate to risk: focused unit checks, integration or workflow checks, end-to-end/user-path checks, accessibility review, dependency/security review, and recovery checks as appropriate.
- Check critical happy paths and failure paths, including invalid input, empty data, loading, errors, unauthorized access, retry/duplicate events, and recovery.
- Record command/tool, date, environment, result, and evidence reference. Mark unrun checks **Not run**; never infer a pass from code inspection or an owner plan.
- Before a release, reconcile the changed code, docs, task status, known limitations, security/privacy review, and recovery snapshot. A local demo is not production; a screenshot is not proof of working behavior.

## 8. Documentation required for every active build

Each active standalone build keeps these records in its own repository:

1. **README.md:** what the product does, current maturity, how to run it, required configuration, safe sample data, known limitations, and links to project records.
2. **PROJECT_MAP.md:** actual directory and screen tree; responsibilities of important files; current source of truth; related app boundaries; links to Drive and GitHub.
3. **BUILD_PLAN.md:** outcome, scope, exclusions, sequence, acceptance criteria, risks, dependencies, and owner.
4. **DECISIONS.md** or short ADR files: durable product and architecture decisions, dates, source links, and consequences.
5. **HANDOFF.md:** latest state, branch and commit SHA, artifacts changed, checks run/not run, blockers, and the single next action.
6. **CHANGELOG.md** or milestone history: meaningful user-visible and architectural changes.
7. **SECURITY.md** for public repositories: vulnerability reporting route and response expectations. Do not publish personal contact details without the owner's approval.

Keep project records readable and current. Prefer a link to the canonical record over copying whole documents into multiple locations. Do not store credentials in any of these files.

## 9. GitHub, Drive, and subscription-independent continuity

The work must remain usable if a chat, AI account, subscription, or single device becomes unavailable. No essential requirement or decision may live only in a conversation.

### Canonical locations

- **GitHub repository:** version-controlled source code, configuration examples, project README, decisions, handoff, and code issue history. Each active standalone product uses its existing canonical repository; inspect before creating a new one. Keep it private by default.
- **Google Drive:** product briefs, research and source notes, approvals, user-facing records, evidence index, and a dated source snapshot/export for recovery. Keep sensitive access limited; do not put secrets in Drive.
- **Local device:** current working copy. It is not the only copy and must not be treated as a remote backup.

### Repository readiness gate

- Each active standalone build has its own canonical GitHub repository. Before code work, inspect the account and local project for an existing repository; reuse the canonical match and avoid duplicates.
- If no repository exists, create or designate a private repository for that standalone build before treating implementation work as backed up. Do not bundle unrelated products into the portfolio-coordination repository just to claim a backup.
- Record the repository URL, default branch, local remote, and access status in the product project map and Drive manifest. If setup or access is blocked, work may be saved locally, but the milestone remains **Sync pending** until a remote copy is verified.

- Maintain a small manifest for each product with its canonical GitHub URL, Drive project-record URL, default branch, latest confirmed commit SHA, last sync date, and current sync state.

### Required sync routine

After each meaningful milestone and before ending a work session:

1. Save the implementation and required docs locally.
2. Review the diff for accidental secrets, private data, generated clutter, and unrelated edits.
3. Commit the coherent change with a descriptive message. Push it to the existing canonical GitHub repository when the connection is available and the change is safe to sync.
4. Update the Drive project map, decision/evidence record, and handoff as relevant. Include the GitHub commit SHA so the record identifies exactly which source version it describes.
5. Save or upload a dated source snapshot/export to Drive when a milestone adds or materially changes build artifacts. Label it with the product, date, and commit SHA. Keep only one clearly named canonical record plus dated snapshots where recovery requires them.
6. Read back the GitHub commit and Drive record/snapshot and confirm that the SHA, links, and status match. Mark **Synced** only after both sides are verified.

If GitHub, Drive, the network, storage, or permissions are unavailable, preserve the local change, make a clearly labeled **Sync pending** handoff, state the exact failed step, and retry when access is restored. Never claim that the work is backed up when only a local save or unverified upload exists.

### Recovery handoff

At every milestone, HANDOFF.md must make it possible for a new human or tool to resume without chat history:

- product name and goal;
- canonical repository and branch;
- latest confirmed commit SHA;
- Drive record and latest snapshot links;
- current status and what works;
- last checks performed, with date and exact result;
- checks not run and known limitations;
- unresolved decisions, blockers, and safety gates;
- one concrete next action and how to verify it.

Keep account recovery information and MFA recovery codes in the account owner's secure password/recovery system, never in GitHub or Drive. Once per release or major data change, perform a restore check in a separate temporary location without overwriting the working copy. This policy does not rely on a paid AI subscription or conversation memory.

## 10. Day-one and milestone checklist

- [ ] Read the product map, plan, latest handoff, current source, and canonical repo.
- [ ] Search for existing functionality before adding a file or duplicate feature.
- [ ] Record problem, users, scope, exclusions, assumptions, risks, and acceptance criteria.
- [ ] Map focused screens/routes and the `pages/`, `components/`, and `data/` responsibilities.
- [ ] Check each maintained app source file has one clear responsibility and stays within 300 lines.
- [ ] Identify sensitive data, access boundaries, external effects, threat cases, and safe demo data.
- [ ] Add or update the appropriate GitHub work item and Drive research/decision record.
- [ ] Define verification; label checks not performed as Not run.
- [ ] At milestone end, update the code, README/map/plan/handoff, GitHub commit, Drive record/snapshot, and matching SHA.
- [ ] Read back both remote records and mark Synced only when verified.

## Research references

Sources checked 2026-11-09. Recheck versioned sources when starting a new project or when a requirement may have changed.

- [W3C Web Content Accessibility Guidelines (WCAG) 2.2](https://www.w3.org/TR/WCAG22/) — current accessibility baseline and testable success criteria.
- [OWASP Application Security Verification Standard (ASVS)](https://asvs.owasp.org/) — web application security requirements; 5.0.0 is the stable release shown by OWASP at review time.
- [NIST SP 800-218, Secure Software Development Framework (SSDF) v1.1](https://csrc.nist.gov/pubs/sp/800/218/final) — secure-development practices integrated into the lifecycle.
- [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet) and [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet) — secret lifecycle, recovery, audit, and sensitive-log handling.
- [GitHub repository documentation](https://docs.github.com/en/repositories/creating-and-managing-repositories) — repository contents, revision history, cloning, and sync.
- [GitHub repository security features](https://docs.github.com/en/code-security/getting-started/quickstart-for-securing-your-repository) — security options and plan-dependent availability.
- [Google Docs version history](https://support.google.com/docs/answer/190843?hl=en-en) and [Drive file versions](https://support.google.com/drive/answer/2409045?hl=en-11) — review and restore prior document/file versions.
- [Drive for desktop: stream or mirror files](https://support.google.com/drive/answer/13401938?hl=en-Link) — sync behavior; sync is not a substitute for checking a recoverable source snapshot.

---
This is a shared process standard and checklist. A checked box is not proof by itself; attach the artifact or verification result that supports it.




---
description: |
  Standalone full-spectrum pull request review triggered by the sfl-review
  label. Performs security, correctness and reliability, and quality and
  maintainability passes, posts one inline thread per finding, submits a
  consolidated review, and publishes the SFL Reviewer Approval check.

on:
  pull_request:
    types: [labeled]
    names: [sfl-review]

permissions:
  contents: read
  pull-requests: read
  copilot-requests: write

engine:
  id: copilot

model: gpt-5.5?effort=high

network: defaults

tools:
  github:
    toolsets: [pull_requests, repos]
    github-app:
      client-id: ${{ vars.SFL_APP_CLIENT_ID }}
      private-key: ${{ secrets.SFL_APP_PRIVATE_KEY }}

safe-outputs:
  github-app:
    client-id: ${{ vars.SFL_APP_CLIENT_ID }}
    private-key: ${{ secrets.SFL_APP_PRIVATE_KEY }}
  create-pull-request-review-comment:
    side: RIGHT
    max: 20
  submit-pull-request-review:
    allowed-events: [APPROVE, REQUEST_CHANGES]
    supersede-older-reviews: true
    footer: always
  create-check-run:
    max: 1
    name: "SFL Reviewer Approval"
  remove-labels:
    allowed: [sfl-review]
    max: 1
    target: triggering
---
# Deployed from: HemSoft/set-it-free-loop/deployment/workflows/sfl-pr-review.md@380fe0edc7a87cfc7b31233a955b37df0223a3a8
# To upgrade: re-run deploy-workflow.ps1 at the desired SHA

<!-- sfl:
  status: active
  version: "1.0.0"
  category: review
  risk-class: trivial
  target-labels: [sfl-review]
  outcome-definition: |
    The triggering pull request receives a current-head structured review,
    one inline thread per finding, and an SFL Reviewer Approval check.
  acceptance-criteria:
    - The sfl-review label triggers exactly one current-head review run
    - The trigger label is removed through App-authenticated safe outputs
    - Security, correctness/reliability, and quality/maintainability are reviewed
    - Every finding is an inline thread classified Critical, High, Medium, or Low
    - The review body reports the run ID, head SHA, verdict, and severity counts
    - Critical or High findings fail the approval check and request changes
    - Medium or Low findings do not fail the approval check
    - Zero findings produce an approving review and successful approval check
  source-repo: HemSoft/set-it-free-loop
-->

# SFL Review - Full-Spectrum Pull Request Review

Review only the pull request that triggered this workflow. The reviewed commit
must be `${{ github.event.pull_request.head.sha }}` and the SFL run ID is
`${{ github.run_id }}`.

Use the GitHub pull request tools to read the triggering PR, its changed files,
and the complete diff. Before creating comments, list existing review comments
and unresolved threads on the current head so you do not repeat a finding.

## Required review passes

Perform all three evidence-based passes independently before producing output.

1. **Security**
   - Injection, unsafe command or path construction, XSS, SSRF, and deserialization
   - Authentication, authorization, privilege boundaries, and secret exposure
   - Dependency, workflow, and supply-chain risks
2. **Correctness and Reliability**
   - Logic errors, regressions, incorrect assumptions, null and boundary cases
   - Error handling, races, resource leaks, data loss, and compatibility
   - Whether tests cover every meaningful new or changed behavior
3. **Quality and Maintainability**
   - Excessive complexity, duplication, coupling, unclear ownership, and dead code
   - Type safety, performance regressions, operational risk, and repository conventions
   - Whether the implementation is the smallest complete and defensible change

## Finding policy

Classify every finding into exactly one severity:

- **CRITICAL** - exploitable security issue, data loss, production crash,
  public API break, race, or deadlock
- **HIGH** - serious correctness, authorization, reliability, or operational defect
- **MEDIUM** - material bug avenue, missing logic-branch tests, performance
  regression, or maintainability problem
- **LOW** - actionable improvement with concrete value and low implementation risk

Do not report style preferences, speculative concerns, or findings without
specific evidence from the changed code.

For each finding, call `create-pull-request-review-comment` on the most precise
changed line. The comment body must begin with one of these exact prefixes:

- `**CRITICAL Finding**`
- `**HIGH Finding**`
- `**MEDIUM Finding**`
- `**LOW Finding**`

After the prefix, state the defect, impact, evidence, and a concrete fix.
Create exactly one inline thread per finding. If there are no findings, create
no inline comments.

## Approval policy

Count all inline findings by severity.

- If any Critical or High finding exists, submit `REQUEST_CHANGES` and create
  the `SFL Reviewer Approval` check with conclusion `failure`.
- If only Medium or Low findings exist, submit `APPROVE` and create the check
  with conclusion `success`.
- If no findings exist, submit `APPROVE` and create the check with conclusion
  `success`.

Submit exactly one consolidated review with this body:

```markdown
## SFL Full-Spectrum Review

SFL run ID: ${{ github.run_id }}
Head SHA: ${{ github.event.pull_request.head.sha }}
Verdict: APPROVE

| Severity | Count |
| --- | ---: |
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 0 |

### Review passes

- Security: complete
- Correctness and Reliability: complete
- Quality and Maintainability: complete

### Summary

Concise evidence-based summary of the review result.
```

Replace the verdict and counts with the actual result. Use
`Verdict: CHANGES_REQUESTED` when Critical or High findings exist.

Create exactly one check run named `SFL Reviewer Approval` with:

- `title`: `SFL full-spectrum review complete`
- `summary`: the verdict, head SHA, run ID, and severity counts
- `conclusion`: the approval-policy result above

After requesting the consolidated review and check run, call `remove-labels`
for `sfl-review` on the triggering pull request. This must be the final safe
output request so the label can be applied again for a later re-review.

Do not modify code, branches, or pull request metadata. The only permitted
label change is removing `sfl-review` through the configured safe output.

---
name: incident-response
description: Structured production incident diagnosis and resolution with postmortem generation.
activation:
  - "production"
  - "outage"
  - "down"
  - "broken"
  - "500 error"
  - "incident"
  - "emergency"
  - "users affected"
  - "not working"
  - "crashed"
---

# Incident Response: Production Issue Resolution

Structured approach to production incidents.

## Activation Triggers

Engage for: production issues, outages, "it's broken", 500 errors, "users affected".

## Response Phases

### Phase 1: Immediate Assessment (0-5 min)

```
Severity: SEV1 (critical) / SEV2 (major) / SEV3 (minor)
Impact: [users/revenue affected]
Started: [timestamp]
Symptoms: [observable behavior]
```

**Quick diagnostics:**
```bash
# Recent deploys
git log --oneline -5 --since="24 hours ago"
glab mr list --merged --after=$(date -d "24 hours ago" +%Y-%m-%d)

# CI status
glab ci status

# Service health
curl -I $PROD_URL/health
```

### Phase 2: Root Cause Analysis (5-30 min)

Generate 3 hypotheses ranked by probability:
```
H1: [cause] - P: X%
    Test: [how to confirm]

H2: [cause] - P: Y%
    Test: [how to confirm]

H3: [cause] - P: Z%
    Test: [how to confirm]
```

**Diagnostic tools:**
```bash
# Git bisect for regressions
git bisect start
git bisect bad HEAD
git bisect good <last-known-good>

# Check logs
glab ci view [job-id]
```

### Phase 3: Resolution Options

Ranked by speed to resolution:
```
1. ROLLBACK (fastest)
   git revert <commit> && git push
   Time: ~5 min

2. HOTFIX (fast)
   [minimal targeted fix]
   Time: ~15-30 min

3. PROPER FIX (slow but complete)
   [full solution]
   Time: ~1-2 hours
```

### Phase 4: Stabilization

- [ ] Fix deployed and verified
- [ ] Monitor 30 min for recurrence
- [ ] Communicate resolution
- [ ] Revert any temporary workarounds

### Phase 5: Postmortem

```markdown
## Incident: [Title]
Date: [date] | Duration: [time] | Severity: [SEV]

## Summary
[1-2 sentences]

## Timeline
- HH:MM [event]
- HH:MM [event]

## Root Cause
[Technical explanation]

## Resolution
[What fixed it]

## Action Items
- [ ] Preventive measure 1
- [ ] Preventive measure 2
```

## Behavior

- Act with urgency, bias toward action
- Prefer rollback over hotfix when safe
- Document as you go (timeline)
- After resolution, offer to generate postmortem

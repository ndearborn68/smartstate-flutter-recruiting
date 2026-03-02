# SmartState Flutter Developer Recruiting

LinkedIn profile evaluator + candidate sourcing pipeline for finding qualified Flutter developers at SmartState (Fort Lee, NJ).

---

## Quick Start - Use in Clay

### Step 1: Evaluate a Candidate (LinkedIn URL → Score)
- **Prompt**: `prompts/linkedin-evaluator-v1.9.md`
- **Schema**: `prompts/linkedin-evaluator-v1.9-schema.json`
- **Model**: Claude 3.5 Sonnet (NOT GPT-4 Nano)
- **Input**: `{{prompt}}` = LinkedIn URL column; optionally pipe in Clay LinkedIn Enrichment columns

### Step 2: Find Candidates (People Search)
- **Prompt**: `prompts/people-search-claygent-v1.0.md`
- **Best approach**: Use Clay's native LinkedIn People Search or Apollo as Column 1, pipe LinkedIn URLs into the Evaluator Claygent

### Webhook (Push results to Clay)
```
https://api.clay.com/v3/sources/webhook/pull-in-data-from-a-webhook-c9aa92fe-35a0-42ea-a388-0213627c4051
```

---

## Evaluation Criteria

### Education Requirements
- ✅ **US university graduate** → APPROVED
- ✅ **Foreign university (non-South Asian) + 12+ years US work experience** → APPROVED
- ❌ **South Asian university** (India, Pakistan, Bangladesh, Sri Lanka, Nepal) → DISQUALIFIED
- ❌ **Foreign university with <12 years US experience** → DISQUALIFIED

### Technical Requirements
- Flutter experience (strong/mentioned)
- Mobile development background
- Dart, native mobile (iOS/Android)
- REST API, CI/CD, testing frameworks

### Stability Requirements
- Average job tenure >12 months
- Not a contractor (detect overlapping employment, staffing agencies)
- Stable employment history

### Location
- Current location: United States
- Target: Within 50 miles of Fort Lee, NJ
- Distance is **estimated by Claygent** (approximate — see TODO #1)

---

## Pipeline Architecture

```
[SOURCING]                          [ENRICHMENT]              [EVALUATION]                  [OUTPUT]
Clay People Search          →       LinkedIn Enrichment    →  LinkedIn Evaluator v1.9   →   Webhook
(LinkedIn/Apollo/Hunter)            (Clay native column)      (Claygent - Claude 3.5)        to Clay table
                                    ↑ optional                ↑ dual-mode: reads enriched
                                    Enrichment populates      columns OR browses URL directly
                                    {{First Name}}, etc.      ↓
                                                              Scores: STRONG_MATCH
                                                                      POTENTIAL_MATCH
                                                                      WEAK_MATCH
                                                                      NO_MATCH
                                                              + referral_candidates array
```

---

## Prompts & Versions

| File | Version | Status | Purpose |
|------|---------|--------|---------|
| `linkedin-evaluator-v1.9.md` | 1.9 | ✅ **Active** | Universal dual-mode: enriched columns OR direct URL browsing |
| `linkedin-evaluator-v1.9-schema.json` | 1.9 | ✅ **Active** | JSON output schema (same as v1.8) |
| `linkedin-evaluator-v1.8.md` | 1.8 | archived | Added referral_candidates extraction |
| `linkedin-evaluator-v1.7.md` | 1.7 | archived | Fixed Antonin Kolar URL bug |
| `linkedin-evaluator-v1.6.md` | 1.6 | archived | Contractor STOP logic |
| `people-search-claygent-v1.0.md` | 1.0 | ✅ Active | Find candidates via web search |

---

## TODO List

### 🔴 High Priority
- [ ] **TODO-001**: Set up Google Maps API key for precise distance calculation
  - Replace Claygent distance estimation with exact driving distance
  - API: Google Maps Distance Matrix API
  - Origin: candidate's `formatted_location` field
  - Destination: Fort Lee, NJ (fixed)
  - Add as Clay HTTP column after evaluator runs

### 🟡 Medium Priority
- [ ] **TODO-002**: Build GitHub Code Quality Analyzer Claygent
  - Input: `technical_evaluation.github_profile` URL from evaluator output
  - Evaluate: repo quality, Flutter projects, commit activity, code style
  - Output: `github_score`, `flutter_repo_count`, `code_quality_rating`
- [ ] **TODO-003**: Set up full end-to-end Clay table
  - Column 1: People Search (LinkedIn/Apollo/Hunter native integration)
  - Column 2: Evaluator Claygent v1.3
  - Column 3: GitHub Analyzer (when built)
  - Column 4: Webhook push to results table
- [ ] **TODO-004**: Test People Search Claygent with real Clay run
  - Run `people-search-claygent-v1.0.md` in Clay
  - Validate LinkedIn URLs returned are real/valid
  - Tune search queries based on results

### 🟢 Nice to Have
- [ ] **TODO-005**: Add outreach template Claygent
  - Auto-generate personalized outreach message for STRONG_MATCH candidates
  - Personalize based on their Flutter projects and recommendations

---

## Test Data

Sample LinkedIn profiles in `test_data.json`:
1. Dardan Bala - NO_MATCH (Kosovo university, only 5yr US experience, contractor)
2. John Markham - NO_MATCH (contractor via Robert Half, self-employed)
3. Samario Torres - NO_MATCH (contractor, overlapping self-employment)

---

## Version History

- **v1.0**: Initial basic evaluator
- **v1.1**: Added job stability and contractor detection
- **v1.2**: Added comprehensive education verification, US work experience tracking, and recommendations analysis
- **v1.3**: Added approximate distance estimation to Fort Lee, NJ (Claygent-based, no API required)
- **v1.4**: Added `pursue` boolean and `distance_estimation` structured object
- **v1.5**: Tightened contractor detection logic
- **v1.6**: Added STEP 0 hard STOP for contractors (outputs minimal JSON and stops — no more rationalization)
- **v1.7**: Fixed Antonin Kolar bug — Claygent visiting placeholder URLs from examples instead of actual input
- **v1.8**: Added `referral_candidates` array — Claygent visits every recommender profile and surfaces qualified Flutter leads
- **v1.9**: Universal dual-mode — reads Clay LinkedIn Enrichment columns if populated; falls back to direct URL browsing for clean vanity URLs from any other source
- **v1.0 (People Search)**: Initial candidate sourcing Claygent

---

## Repository

**GitHub**: https://github.com/ndearborn68/smartstate-flutter-recruiting

---

Built with AutoClaygent by Jordan Crawford | Blueprint GTM

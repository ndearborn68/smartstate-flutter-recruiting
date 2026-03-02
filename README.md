# SmartState Flutter Developer Recruiting

LinkedIn profile evaluator + candidate sourcing pipeline for finding qualified Flutter developers at SmartState (Fort Lee, NJ).

---

## Quick Start - Use in Clay

### Step 1: Evaluate a Candidate (LinkedIn URL → Score)
- **Prompt**: `prompts/linkedin-evaluator-v1.3.md`
- **Schema**: `prompts/linkedin-evaluator-v1.3-schema.json`
- **Model**: Claude 3.5 Sonnet (NOT GPT-4 Nano)
- **Input**: LinkedIn profile URL column

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
[SOURCING]                          [EVALUATION]                    [OUTPUT]
Clay People Search          →       LinkedIn Evaluator v1.3    →    Webhook
(LinkedIn/Apollo/Hunter)            (Claygent - Claude 3.5)         to Clay table
                                    ↓
                                    Scores: STRONG_MATCH
                                            POTENTIAL_MATCH
                                            WEAK_MATCH
                                            NO_MATCH
```

---

## Prompts & Versions

| File | Version | Status | Purpose |
|------|---------|--------|---------|
| `linkedin-evaluator-v1.3.md` | 1.3 | ✅ Active | Full profile eval + distance estimate |
| `linkedin-evaluator-v1.3-schema.json` | 1.3 | ✅ Active | JSON output schema |
| `linkedin-evaluator-v1.2.md` | 1.2 | archived | Previous version |
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
- **v1.0 (People Search)**: Initial candidate sourcing Claygent

---

## Repository

**GitHub**: https://github.com/ndearborn68/smartstate-flutter-recruiting

---

Built with AutoClaygent by Jordan Crawford | Blueprint GTM

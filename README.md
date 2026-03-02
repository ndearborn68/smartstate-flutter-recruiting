# SmartState Flutter Developer Recruiting

LinkedIn profile evaluator for finding qualified Flutter developers at SmartState (Fort Lee, NJ).

## Quick Start - Use in Clay

### Prompt
Copy content from: `prompts/linkedin-evaluator-v1.2.md`

### JSON Schema
Copy content from: `prompts/linkedin-evaluator-v1.2-schema.json`

### Model Selection
Select **Claude 3.5 Sonnet** in Clay (NOT GPT-4 Nano - causes hallucination)

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

## Test Data

Sample LinkedIn profiles in `test_data.json`:
1. Dardan Bala - NO_MATCH (Kosovo university, only 5yr US experience, contractor)
2. John Markham - NO_MATCH (contractor via Robert Half, self-employed)
3. Samario Torres - NO_MATCH (contractor, overlapping self-employment)

## Version History

- **v1.0**: Initial basic evaluator
- **v1.1**: Added job stability and contractor detection
- **v1.2**: Added comprehensive education verification, US work experience tracking, and recommendations analysis

## Next Steps

1. **Google Maps Distance Calculation** - In progress (API key setup)
2. **Clay People Search Integration** - Find candidates automatically
3. **GitHub Code Quality Analyzer** - Evaluate candidate GitHub profiles

## Continue Conversation on Desktop

To pick up where you left off:

1. **Clone this repo on your desktop**:
   ```bash
   git clone https://github.com/ndearborn68/smartstate-flutter-recruiting.git
   cd smartstate-flutter-recruiting
   ```

2. **The conversation backup is saved here**:
   ```
   conversation-backup-20260301.jsonl
   ```

3. **Reference it when continuing work**:
   - Open Claude Code
   - Navigate to this directory
   - Say "I want to continue from the conversation backup file"
   - Or just reference specific context you need

## Repository

**GitHub**: https://github.com/ndearborn68/smartstate-flutter-recruiting

---

Built with AutoClaygent by Jordan Crawford | Blueprint GTM

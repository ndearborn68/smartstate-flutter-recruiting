# Clay Table Setup - SmartState Flutter Recruiting Pipeline

## What You're Building

A Clay table that:
1. Pulls Flutter developer candidates from LinkedIn People Search
2. Runs each candidate through the Evaluator Claygent (scores them)
3. Pushes scored results to your webhook

---

## Step 1: Create a New Clay Table

1. Go to [app.clay.com](https://app.clay.com)
2. Click **New Table**
3. Name it: `SmartState Flutter Recruiting`

---

## Step 2: Set Up LinkedIn People Search (Column 1)

This is your candidate sourcing column.

1. Click **+ Add Column** → **Integrations** → **LinkedIn People Search**
2. Configure the search filters:

| Filter | Value |
|--------|-------|
| **Job Title** | `Flutter Developer` OR `Flutter Engineer` OR `Mobile Developer` OR `Senior Flutter` |
| **Location** | United States |
| **Geography** | New York, New Jersey, Connecticut, Pennsylvania |
| **Keywords** | `Flutter` |

3. Set **Results per run**: 25–50 (start small, scale up after testing)
4. Click **Run** — this populates rows with candidate profiles

> Each row = one candidate. LinkedIn People Search returns: name, headline, location, LinkedIn URL, and basic profile info.

---

## Step 3: Add the Evaluator Claygent (Column 2)

This column scores each candidate.

1. Click **+ Add Column** → **Claygent**
2. Select model: **Claude 3.5 Sonnet**
3. Paste the full contents of `prompts/linkedin-evaluator-v1.3.md` as the prompt
4. Set **Input**: map to the `LinkedIn URL` field from Column 1
5. Set **JSON Schema**: paste the full contents of `prompts/linkedin-evaluator-v1.3-schema.json`
6. Click **Run**

> The Claygent visits each LinkedIn profile, evaluates the candidate against all criteria, and returns structured JSON including match score, distance estimate, education verification, and stability analysis.

---

## Step 4: Add Webhook Push (Column 3)

This sends scored results to your Clay results table.

1. Click **+ Add Column** → **HTTP API**
2. Method: **POST**
3. URL:
```
https://api.clay.com/v3/sources/webhook/pull-in-data-from-a-webhook-c9aa92fe-35a0-42ea-a388-0213627c4051
```
4. Body: map the full Claygent output JSON
5. Run after Claygent completes

---

## Step 5: Add Filter Column (Optional but Recommended)

Add a formula column to flag only STRONG_MATCH and POTENTIAL_MATCH candidates:

- Column name: `Worth Reviewing`
- Formula: `IF({{evaluator.overall_evaluation.recommendation}} = "STRONG_MATCH" OR {{evaluator.overall_evaluation.recommendation}} = "POTENTIAL_MATCH", true, false)`

This lets you sort and focus on the candidates worth reviewing.

---

## Running the Pipeline

1. Run **LinkedIn People Search** → wait for rows to populate
2. Run **Evaluator Claygent** → wait 2–5 min per candidate
3. Filter by `Worth Reviewing = true`
4. Review scored candidates

---

## Key Output Fields to Surface as Columns

After the Claygent runs, add these as visible columns for quick review:

| Field Path | Column Label |
|-----------|--------------|
| `overall_evaluation.recommendation` | Match |
| `overall_evaluation.match_score` | Score |
| `distance_estimation.estimated_miles_to_fort_lee` | Miles to Fort Lee |
| `distance_estimation.within_50_miles` | Within 50mi |
| `education.qualification_path` | Education Status |
| `stability_analysis.is_contractor` | Is Contractor |
| `stability_analysis.is_job_hopper` | Is Job Hopper |
| `technical_evaluation.flutter_skill_level` | Flutter Level |
| `overall_evaluation.next_steps` | Next Steps |

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Claygent returns raw text instead of JSON | Make sure JSON Schema is pasted in the schema field |
| LinkedIn People Search returns no results | Broaden location to full "United States", remove keyword filters |
| Claygent scores everyone as NO_MATCH | Check that LinkedIn URL column is correctly mapped as input |
| Distance shows UNABLE_TO_ESTIMATE | Candidate location is too vague — check their profile manually |

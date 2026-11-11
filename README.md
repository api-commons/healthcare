# Healthcare

The common healthcare vocabulary -- patients and the practitioners who treat them, the encounters where care happens, the observations, conditions and prescriptions recorded, and the coverage and claims that pay for it -- distilled from the APIs that already exist, the FHIR standard included. Appointments belong to the scheduling context. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Healthcare, Digital Health, Telehealth, Health Tech, FHIR, Electronic Health Records, EHR, Health Insurance, Healthtech in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| patient | 38 | 29 | date_of_birth 83%, gender 79%, id 76%, first_name 69%, last_name 69%, email 66% | patients 25, patient 17 |
| practitioner | 21 | 18 | id 78%, last_name 67%, npi 67%, first_name 61%, specialty 56%, name 50% | providers 8, practitioner 7, practitioners 3, clinicians 2 |
| encounter | 11 | 8 | id 75%, status 62%, type 62% | encounter 8, visits 3, encounters 2 |
| observation | 16 | 10 | code 70%, id 60%, patient_id 50% | observation 8, vitals 4, observations 2, measurements 1 |
| condition | 13 | 10 | code 60%, id 60%, status 60%, description 50%, patient_id 50% | condition 8, problems 3, diagnoses 2, conditions 1 |
| prescription | 13 | 9 | code 56%, dosage 56%, id 56% | medicationrequest 7, prescriptions 4, prescription 3, medications 1 |
| coverage | 9 | 8 | id 75%, code 62%, patient_id 62%, payer 62%, relationship 50%, status 50% | coverage 6, eligibility 3, eligibility_checks 1, insurances 1 |
| health_claim | 8 | 6 | id 67%, status 67%, code 50%, patient_id 50%, payer 50%, practitioner_id 50%, service_date 50% | claims 5, explanationofbenefit 3, claim 1 |

Cohort: 259 providers with harvested OpenAPI, 69 crosswalks. Of the providers exposing a patient: 63% create, 90% list, 76% fetch, 47% update, 24% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- FHIR is to healthcare what Open Banking is to banking: 32 providers in this cohort publish nothing but FHIR resources. Counted once as HL7's voice, the market's own agreement shows.
- A patient agrees on date of birth, gender, name and email -- the demographic core every EHR, telehealth platform and payer API carries.
- A practitioner agrees on name, NPI and specialty: the National Provider Identifier is the one identifier the US market shares.
- Coverage agrees on the payer, the member's relationship and status, and a claim on its patient, practitioner, payer and date of service -- the payer side of healthcare is more uniform than the clinical side.
- Clinical records (observations, conditions, prescriptions) agree only on a code and the patient: the code is the vocabulary (LOINC, SNOMED, ICD-10, RxNorm), and the structure around it is the provider's.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Every provider that publishes only FHIR resources counts once, as `hl7-fhir`, with its own crosswalk. Providers with FHIR alongside a proprietary API (athenahealth, Tebra, AdvancedMD, Particle, the NHS and VA) are counted on their own.
- Appointments are the scheduling context's; health insurance -- coverage and claims -- is this context's, handed over from insurance.
- A practitioner is `/practitioners` (FHIR's word), though eight providers say `providers`: `/providers` means an API provider or a service provider in other contexts.
- A prescription covers FHIR's MedicationRequest and medication orders beneath a patient.
- A health claim is `/health-claims` and covers Explanation of Benefit; `/claims` is the insurance context's.
- Visits and consultations are encounters; vitals, lab results and measurements are observations; problems and diagnoses are conditions; eligibility checks are coverage.
- Duplicates merged: Cerner into Oracle Health, CDC, EHRbase into vitagroup, Vital into Junction, three NHS listings into NHS England, Thatch.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).

# BTD 1 - PHA pre-notification and multiparty sharing on non-compliance

## Contents

- [About](https://github.com/border-trade-demonstrators/btd-1#about)
- [Use cases](https://github.com/border-trade-demonstrators/btd-1#use-cases)
- [Paticipants](https://github.com/border-trade-demonstrators/btd-1#participants)
- [Required goods categories](https://github.com/border-trade-demonstrators/btd-1#required-goods-categories)
- [Required capabilities](https://github.com/border-trade-demonstrators/btd-1#required-capabilities)
- [Mapping to report content and recommendations](https://github.com/border-trade-demonstrators/btd-1#mapping-to-eot-report-content-and-recommendations)
- [Timeline](https://github.com/border-trade-demonstrators/btd-1#timeline)

## About

PHAs will undertake a number of checks at the border (e.g. identity and physical checks). The ability to receive pre-notifications and share non-compliance insight via signals rather than raw data with other PHAs, BCP, LAs, FSA, and Defra in a timely manner, particularly for RoRo loads which move quickly will enable more effective goods control and interventions.

## Use cases

### Use case 1

PHA will request a precise selection of fields (e.g. commodity code, country of origin, provider-mapping to SoR UUID, ETA if possible, human readable description of goods) from an EoT SoR which will be posted on to ISN messaging infrastructure from the EoT operators ISN site via either multipart form data or JSON authenticated via Oauth Bearer token.

PHA may receive signal information via a number of CO BIT supplied ISN interoperability options including:
- dashboard
- RSS
- vanilla JSON feed

Signals will be prioritised according to a local model per PHA/FSA/Defra requirements and will have an expiry date to ensure information accuracy.


PHA will close a feedback loop by transmitting information to selected BTD participants indicating whether the information was useful, not useful and what the result of the intervention was - this information will be **anonymised**.

### Use case 2

Interventions inititated through processing pre-notification signals will yield information outlining business details and the nature of any non-compliance. This information will enable the updating of risk models and targeted risk interventions locally.

Sharing this non compliance efficiently to all relevant border agencies across the country will help mitigate negative impacts of potential port shopping  (traders avoiding targeted controls due to local intelligence not picking up historic non-compliance occuring at other ports) or traders using triangular trade routes to avoid controls (where goods real origin, which dictates whether controls are applicable or not, is obscured).

Where appropriate it may be useful to share examples of non compliance such as incorrect goods information with supply chain actors or other jurisdictions to reduce delay and administrative cost and increase predictability of future goods movements involving these actors.

There will likely be some quicker wins with this use case by linking with existing systems or systems in development.

## Participants

Lead: PHA

UKG: CO, Defra, FSA

Trade associations, standards bodies: C4DTI
Consortia:
Experts: TBC (Legal)

## Required goods categories

N/A

## Required capabilities

- Pre-notification via precise selection of fields from underlying EoT or similar system of record transmitted as signals
- Operationalisation of notification signal if possible/sensible at key border site
- Multiparty bi-directional exchange of subsequent border process 'insight' as signals
- Multilateral share (Collaboration Agreement) form of governance framework for interested parties
- Possibility of access to underlying SoR supply chain data if needed

## Mapping to EoT report content and recommendations

- [Report section 5](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#measuring-the-value-of-an-eot-model): Overview of the UK biosecurity regimes - pre-notification
- [Report section 4](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#the-ecosystem-of-trust-model): usecases
- [Report section 6 B](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#recommendations-for-how-we-address-the-challenges-to-eot-adoption): Signals, Collab Agreement


## Timeline

BTD 1 will run from an introductory internal User Research session week beginning 25 September for approximately 8 weeks.


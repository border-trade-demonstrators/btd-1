ℹ️ See [EoT Home](https://github.com/ecosystem-of-trust) for background on the Ecosystem or Trust (EoT), terms of reference and other artefacts and important links. See [Border Trade Demonstrators](https://github.com/border-trade-demonstrators) for background information on BTDs and the [dasbhoard](https://github.com/border-trade-demonstrators/dashboard) for details on the different BTDs. Please note this page may link to closed and private repositories which you will need to be invited into.

# BTD 1 - PHA pre-notification and multiparty sharing on non-compliance

## Contents

- [About](https://github.com/border-trade-demonstrators/btd-1#about)
- [Use cases](https://github.com/border-trade-demonstrators/btd-1#use-cases)
- [Paticipants](https://github.com/border-trade-demonstrators/btd-1#participants)
- [Required goods categories](https://github.com/border-trade-demonstrators/btd-1#required-goods-categories)
- [Required capabilities](https://github.com/border-trade-demonstrators/btd-1#required-capabilities)
- [Mapping to report content and recommendations](https://github.com/border-trade-demonstrators/btd-1#mapping-to-eot-report-content-and-recommendations)
- [Next steps](https://github.com/border-trade-demonstrators/btd-1#next-steps)
- [Deliverables](https://github.com/border-trade-demonstrators/btd-1#deliverables)
- [Timeline](https://github.com/border-trade-demonstrators/btd-1#timeline)

## About

PHAs will undertake a number of checks at the border (e.g. identity and physical checks). The ability to receive pre-notifications and share non-compliance insight via signals rather than raw data with other PHAs, BCP, LAs, FSA, and Defra in a timely manner, particularly for RoRo loads which move quickly will enable more effective goods control and interventions. It will be useful for these notifications to move bi-directionally e.g. FSA to PHA and the other way round.

## Use cases

### Use case 1

PHA will request a precise selection of fields (e.g. commodity code, country of origin, provider-mapping which provides a link to supply chain data in the underlying EoT product System of Record 'SoR', container number, trailer number, seal id, ETA if possible, human readable description of goods) from an EoT SoR which will be posted on to ISN messaging infrastructure from the EoT operators ISN site via either multipart form data or JSON authenticated via Oauth 2 Bearer token.

PHA may receive signal information via a number of CO BIT supplied ISN interoperability options including:
- dashboard
- vanilla JSON feed
- Server Sent Event (SSE) push (coming soon)
- RSS or feed (potentially if required)

Signals will be prioritised according to a local model per PHA/FSA/Defra requirements and will have an expiry date to ensure information accuracy.


PHA will close a feedback loop by transmitting information to selected BTD participants (initially just CO) indicating whether the information was useful, not useful and what the result of the intervention was - this information will be **anonymised**. This feedback loop will work as part of the evaluation.

A draft of the [signal payload](https://github.com/information-sharing-networks/signals#example-3---a-signal-and-its-metadata-which-is-associated-to-a-payload-of-information-in-a-given-domain) fields and suggested optionality is outlined below.

| Field name | Description | Data type | Optionality |
| --- | --- | --- | --- |
| Commodity code | The specific commodity code for the goods | String | Required |
| Commodity description | Plain text description of goods | String | Required |
| Country of origin | Country goods/sample originated from | ISO3166 (E.G. GB) | Required |
| Unit identifier | The identifier for an incoming unit | May be one of a set of identifiers (e.g. container number, trailer number, VRN, TRN etc) | Required |
| Unit type | The type of an incoming unit | Enumeration (e.g. Container, Trailer, TruckAccompanied etc) | Required |
| TBC Seal ID | A seal identifier | TBC | Optional |
| CHED ID | A CHED identifier | TBC | Optional |
| Mode | The goods movement mode | Enumeration (e.g. RORO,TBC) | Required |

An example of a pre-notification signal could therefore look something like the below:

```clojure
{
  :provider "organisation-a.my-example.xyz"
  :start "2024-01-10T16:51:51.379676Z" ; N.B. can be used to indicate an ETA
  :end "2024-01-20T18:00:00Z"
  :published "2024-01-08T12:51:51.379072Z"
  :signalId "704e851a-9ab4-40d6-b995-765f64104072"
  :correlationId "734713bc04"
  :category "a-useful-category"
  :object "Brazil nuts"
  :predicate "moving to PortA"
  :providerMapping {
    :id "804e851b-9ab4-40d6-b995-765f64104072"
    :uri "https://uri-to-system-which-provided.io/xyz/"
  }
  :payload {
    :cnCode "a-cn-code"
    :countryOfOrigin "GB"
    :commodityDescription "Brazil nuts"
    :unitIdentifier "TBC containerNumberAB12"
    :unitType "container"
    :mode "RORO"
  }
}

### Use case 2

Interventions inititated through processing pre-notification signals will yield information outlining business details and the nature of any non-compliance. This information will enable the updating of risk models and better targeted risk interventions locally.

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

For detail see the [use cases](https://github.com/border-trade-demonstrators/btd-1#use-cases) above and the [deliverables]() below. N.B. both use cases and deliverables will be developed through user research and workshops.

- Pre-notification via precise selection of fields from underlying EoT or similar system of record transmitted as signals
- Operationalisation of notification signal if possible/sensible at key border site
- Multiparty bi-directional exchange of subsequent border process 'insight' as signals
- Multilateral share (Collaboration Agreement) form of governance framework for interested parties
- Possibility of access to underlying SoR supply chain data if needed

## Mapping to EoT report content and recommendations

- [Report section 5](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#measuring-the-value-of-an-eot-model): Overview of the UK biosecurity regimes - pre-notification
- [Report section 4](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#the-ecosystem-of-trust-model): usecases
- [Report section 6 B](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#recommendations-for-how-we-address-the-challenges-to-eot-adoption): Signals, Collab Agreement

## Next steps

We are currently in discovery and will be running a series of informal user research workshops. We will let you know when sessions are being scheduled.

## Deliverables

TODO: from user research sessions and workshops

## Timeline

BTD 1 will run for approximately 8 weeks. Start date TBC.


ℹ️ See [EoT Home](https://github.com/ecosystem-of-trust) for background on the Ecosystem or Trust (EoT), terms of reference and other artefacts and important links. See [Border Trade Demonstrators](https://github.com/border-trade-demonstrators) for background information on BTDs and the [dasbhoard](https://github.com/border-trade-demonstrators/dashboard) for details on the different BTDs. Please note this page may link to closed and private repositories which you will need to be invited into.

# BTD 1 - PHA SPS import notifications

## Contents

- [About](https://github.com/border-trade-demonstrators/btd-1#about)
- [Use case](https://github.com/border-trade-demonstrators/btd-1#use-case)
- [Paticipants](https://github.com/border-trade-demonstrators/btd-1#participants)
- [Required goods categories](https://github.com/border-trade-demonstrators/btd-1#required-goods-categories)
- [Required capabilities](https://github.com/border-trade-demonstrators/btd-1#required-capabilities)
- [Mapping to report content and recommendations](https://github.com/border-trade-demonstrators/btd-1#mapping-to-eot-report-content-and-recommendations)

## About

PHAs will undertake a number of checks at the border (e.g. identity and physical checks). The ability to receive supplementary notification data via signals in a timely manner, particularly for RoRo loads which move quickly, will enable more effective goods control and interventions. 

## Use case

PHA will request a selection of fields (see below) from an EoT System of Record which will be posted on to ISN messaging infrastructure from the EoT operators ISN site via either multipart form data or JSON authenticated via Oauth 2 Bearer token.

PHA may receive signal information via a number of options including:
- dashboard
- vanilla JSON feed
- Server Sent Event (SSE) push (coming soon)
- RSS or feed (potentially if required)

Signals will be prioritised according to a local model per PHA/FSA/Defra requirements and will have an expiry date to ensure information accuracy.

PHA will close a feedback loop by transmitting information to selected BTD participants (initially just CO) indicating whether the information was useful, not useful and what the result of the intervention was - this information will be **anonymised**. This feedback loop will work as part of the evaluation.

Two payloads are accepted and the [signal payloads](https://github.com/information-sharing-networks/signals#example-3---a-signal-and-its-metadata-which-is-associated-to-a-payload-of-information-in-a-given-domain) fields are outlined below.

## Pre-notification Payload 

| Field name | Description | Data type | Optionality | Notes |
| --- | --- | --- | --- | --- |
| Commodity code(s) | Specific commodity codes for the goods | (Multiple cnCodes where the resolution or no. digits varies depending on how much is known about the goods at any time) smallest 4 digits String | Required | |
| Commodity description | Plain text description of goods | String | Required | If there are multiple cnCodes how useful is this field ? |
| Country of origin | Country goods/sample originated from | ISO3166 (E.G. GB) | Required | |
| Unit identification | A map of identifiers and identifier types (as key/value pairs) for an incoming unit | May be multiples from a set of identifiers (e.g. container number, trailer registration number, VRN, TRN etc) | Required | |
| TBC Seal ID | A seal identifier | TBC | Optional | |
| CHED Numbers | A set of CHED identifiers | CHED-P or CHED-D | Optional | |
| Exporter EORI Number | TBC | TBC | Optional | Must not be provided if it pertains to a sole trader setup or similar - (consortia will need to guarantee they will not provide if this is the case) |
| Importer EORI Number | TBC | TBC | Optional | Must not be provided if it pertains to a sole trader setup or similar - (consortia will need to guarantee they will not provide if this is the case) |
| Mode | The goods movement mode | Enumeration (e.g. RORO,TBC) | Required | |

An example of a pre-notification signal could therefore look something like the below:

```clojure
{
  :provider "organisation-a.my-example.xyz"
  :start "2024-04-01T15:00:00.00Z" ; Indicates an ETA
  :end "2024-04-20T18:00:00Z"
  :published "2024-01-08T12:51:51.379072Z"
  :signalId "704e851a-9ab4-40d6-b995-765f64104072"
  :correlationId "734713bc04"
  :category #{"pre-notification" "isn@btd-1.info-sharing.network"}
  :object "Chicken plus Beef shipment"
  :predicate "arriving at Port A with ETA 2024-04-01T15:00:00.00Z"
  :payload {
    :cnCodes #{"cnchicken01" "cnbeef02"} ; e.g. minimally resolved cncodes will be four characters/digits long (may be longer or more resolved)
    :countryOfOrigin "GB"
    :commodityDescription "Chicken 40%, beef 60%"
    :chedNumbers #{"CN010203"} ; e.g. either a CHED-D or a CHED-P number
    ; N.B. unitIdentification is not exhaustive
    :unitIdentification {:containerNumber "containerNo123" :trailerRegistrationNumber "trailerRegNo123"}
    :mode "RORO"
    :exporterEORI "EORI-exp-01"
    :importerEORI "EORI-imp-01"
  }
}
```

### Dispatch Payload

Optionally, a dispatch signal payload is sent as an update when the vehicle has set off to the port. The signal will carry additional information to support the identification of the unit such as trailer number, expected/actual time of departure to the port of exit. As this information which is more likely to be avaiable closer to the unit being ready and loaded.

The dispatch signal should be linked to the original pre-notificaton signal using the original correlationId.

| Field name | Description | Data type | Optionality | Notes |
| --- | --- | --- | --- | --- |
Ched Numbers|A set of CHED identifiers|CHED-P or CHED-D|Optional||
cnCodes|Classification of the goods reference |String |TBC ||
Commodity Description|Plain text description of goods |String|Required|If there are multiple cnCodes how useful is this field ?
Country Of Origin|Country goods/sample originated from|ISO3166 (E.G. GB)|Required||
Exporter EORI|Exporter registration number |String|TBC ||
Importer EORI|Importer registration number |String|TBC ||
Location|Location of where goods are loaded|String |TBC ||
Mode|The goods movement mode|Enumeration (e.g. RORO/TBC)|Required||
Seal numbers|Serial number or reference of seals on unit|String |TBC ||
Destination Plant|Place of destination of goods |String |TBC ||
Planned departure time|Time of expected departure from loading location |Date Time ISO 8601|Required||
Actual departure time|Confirmed time of actaul detarture of goods from loading location |Date Time ISO 8601|Required||
Port of Exit|Port where the goods are exiting |String (e.g. Calais)|Required||
Port of Entry|Port where the goods are entering in UK|String (e.g. Dover)|Required||
Unit Identification|A map of identifiers and identifier types (as key/value pairs) for an incoming unit.|May be multiples from a set of identifiers (e.g. container number trailer registration number/VRN/ TRN etc)|Required||
Trailer registration number|Registration number of the trailer|String (e.g. WGM033P)|Required||
Start|Signal creation time and date|Date Time ISO 8601|Required||
Summary |Combined summary using the predicate, object and subject|string (e.g. Load departed 'Rolpek 2' at 16:07 (local time) on 08/08/2024 bound for 'Calais' with an ETA of 22:00 09/08/2024. Port of entry ‘Dover’|Required||



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

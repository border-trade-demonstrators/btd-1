# Signal creation examples

N.B. Sample signals listed on this page use Extensible Data Notation (EDN) as a format this is to simplify them and provide full semantics e.g. sets as distinct from vectors or other collection types. The API will in fact by default return JSON.

## About

In order to create [signals](https://github.com/information-sharing-networks/signals) within the ISN a participant may use their participant site and its API.
Signals are created using an Indieweb 'event' post type by calling the ISN site 'micropub' endpoint internally this creates a 'signal' from the information-sharing-network signals spec.

## Sample signals
The BTD 1 domain is pre-notifications and compliance.
At the time of writing signal examples below are compatible with 0.4.77+ of the ISN reference implementation.

### Creating an original simple BTD 1 relevant signal

ISN participants create [spec compliant signals](https://github.com/information-sharing-networks/signals) by using their Participant Site API.

The simplest possible BTD 1 relevant signal would look something like the below (N.B. swap out cnCode, unitId and other fields for relevant values etc):

```bash
curl -i -X POST -H "Authorization: Bearer YOUR-TOKEN" -d h=event -d "name=brazil nuts" -d "summary=moving to PortA" -d category=pre-notification -d category=isn@sample-isn.my-example.xyz -d "description=cnCode=cnNuts^countryOfOrigin=GB^unitId=134149^unitType=container^mode=RORO" https://your-site.my-example.xyz/micropub
```

It is also possible to create a signal by passing JSON to the micropub endpoint:

```bash
curl -i -X POST -H "Content-Type: application/json" -H "Authorization: Bearer YOUR-TOKEN" -d '{"h": "event", "name": "brazil nuts", "summary": "moving to PortA", "category": ["pre-notification", "isn@sample-isn.my-example.xyz"], "description": "cnCode=cnNuts^countryOfOrigin=GB^unitId=134149^unitType=container^mode=RORO"}' https://your-site.my-example.xyz/micropub
```

As an illustration the signal created might look something like:

```clojure
{
 :object "brazil nuts"
 :predicate "moving to PortA"
 :provider "sample-provider.my-example.xyz"
 :signalId "ba37897c-d8b8-49c9-a7ba-cfadbadcbec4"
 :correlation-id "b0302884-296a-4a1e-bc09-e07db93e65b9"
 :publishedDate "2024-02-14"
 :publishedDateTime "2024-02-14T22:13:41.371194Z"
 :end "2024-02-21T22:13:41.372409Z"
 :category #{"pre-notification" "isn@sample-isn.my-example.xyz"}
 :payload {
   :cnCode "cnNuts"
   :countryOfOrigin "GB"
   :unitId "134149"
   :unitType "container"
   :mode "RORO"
 }
}
```

### Creating a signal specifying an ETA for arrival at port

Within the context of BTD 1 if the information is available to participants for a given goods movement it is possible to specify an ETA for arrival at destination port, this is useful information for the Port Health Authority team. An ETA can be specified using the 'start' field as below:

```bash
curl -i -X POST -H "Authorization: Bearer YOUR-TOKEN" -d h=event -d "name=brazil nuts" -d "summary=moving to PortA" -d category=pre-notification -d category=isn@sample-isn.my-example.xyz -d start=2024-03-07T09:00:00Z -d "description=cnCode=cnNuts^countryOfOrigin=GB^unitId=134149^unitType=container^mode=RORO" https://your-site.my-example.xyz/micropub
```

As an illustration the signal created might look something like:

```clojure
{
 :object "brazil nuts"
 :predicate "moving to PortA"
 :provider "sample-provider.my-example.xyz"
 :signalId "ba37897c-d8b8-49c9-a7ba-cfadbadcbec4"
 :correlation-id "b0302884-296a-4a1e-bc09-e07db93e65b9"
 :publishedDate "2024-02-14"
 :publishedDateTime "2024-02-14T22:13:41.371194Z"
 :start "2024-03-07T09:00:00Z"
 :end "2024-02-21T22:13:41.372409Z"
 :category #{"pre-notification" "isn@sample-isn.my-example.xyz"}
 :payload {
   :cnCode "cnNuts"
   :countryOfOrigin "GB"
   :unitId "134149"
   :unitType "container"
   :mode "RORO"
 }
}
```

### Creating a workflow with multiple signals on the same thread

#### Creating the original signal

It is possible to create a 'golden thread' of related signals. An original signal is created as below (N.B. swap out cnCode, unitId and other fields for relevant values etc):

```bash
curl -i -X POST -H "Authorization: Bearer YOUR-TOKEN" -d h=event -d "name=brazil nuts" -d "summary=moving to PortA" -d category=pre-notification -d category=isn@sample-isn.my-example.xyz -d "description=cnCode=cnNuts^countryOfOrigin=GB^unitId=134149^unitType=container^mode=RORO" https://your-site.my-example.xyz/micropub
```

As an illustration the signal created might look something like:

TBD

and in this case it has a correlation-id 0e1fbb0a-f212-44c9-b546-da3014ba1624

#### Attaching a second signal to the original to share an opinion

A second participant can use the correlation-id from the original signal to attach an opinion to it:

```bash
curl -i -X POST -H "Authorization: Bearer YOUR-TOKEN" -d h=event -d "name=nuts and bolts" -d "summary=reclassified as nuts and bolts" -d category=pre-notification -d category=isn@sample-isn.my-example.xyz -d "description=correlation-id=0e1fbb0a-f212-44c9-b546-da3014ba1624^cnCode=cnNutsBolts^countryOfOrigin=GB^unitId=134149^unitType=container^mode=RORO" https://your-site.my-example.xyz/micropub
```
In the web dashboard this signal will be attached to the original in a list (it will be indented indicated there is a thread). When retrieving signals with the API they are grouped by correlation-id to preserve this relationship.

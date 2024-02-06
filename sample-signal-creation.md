# Signal creation examples

## About
In order to create signals within the ISN a participant may use their participant site and its API.
Signals are created using an Indieweb 'event' post type by calling the ISN site 'micropub' endpoint internally this creates a 'signal' from the information-sharing-network signals spec.

## Sample signals
The BTD-1 domain is pre-notifications and compliance.
At the time of writing signal examples below are compatible with 0.3.34+ of the ISN reference implementation.

### Creating an original BTD-1 relevant signal
A first ISN participant creates their initial signal as below (N.B. swap out cnCode, unitId and other fields for sensible values etc:

```bash
curl -i -X POST -H "Authorization: Bearer YOUR-TOKEN" -d h=event -d "name=brazil nuts" -d "summary=moving to PortA" -d category=domain -d "description=cnCode=cnNuts^countryOfOrigin=GB^unitId=134149^unitType=container^mode=RORO" https://your-site.my-example.xyz/micropub
```

As an illustration the signal created might look something like:

TBD

and in this case it has a correlation-id 0e1fbb0a-f212-44c9-b546-da3014ba1624

### Attaching a second signal to the original to share an opinion

A second participant can use the correlation-id from the original signal to attach an opinion to it:

```bash
curl -i -X POST -H "Authorization: Bearer YOUR-TOKEN" -d h=event -d "name=nuts and bolts" -d "summary=reclassified as nuts and bolts" -d category=domain -d "description=correlation-id=0e1fbb0a-f212-44c9-b546-da3014ba1624^cnCode=cnNutsBolts^countryOfOrigin=GB^unitId=134149^unitType=container^mode=RORO" https://your-site.my-example.xyz/micropub
```
In the web dashboard this signal will be attached to the original in a list (it will be indented indicated there is a thread).

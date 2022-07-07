## OTP's GraphQL API

OTP has a very comprehensive and flexible GraphQL API.

Its endpoint is available at `https://otp.trapeze.leonard.io/otp/routers/default/index/graphql`.

There is a GraphQL Playground available at [https://https://otp.trapeze.leonard.io/graphiql](https://otp.trapeze.leonard.io/graphiql)

### Example queries

#### Getting the next 10 departures at Friedrichstr. platform 4

```graphql
query {
    stop(id: "2:de:11000:900100001:2:53") {
        name
        platformCode
        gtfsId
        stoptimesWithoutPatterns(numberOfDepartures: 10) {
            scheduledDeparture
            realtime
            trip {
                gtfsId
                tripHeadsign
                route {
                    shortName
                }
            }
        }
    }
}
```

#### Getting a route from Potsdam to Berlin: walking and public transport

```graphql
query {
  plan(
    time: "10:30"
    from: { lat: 52.3995, lon: 13.0583 }
    to: { lat: 52.5135, lon: 13.3949 }
    transportModes: [{ mode: TRANSIT }, { mode: WALK }]
  ) {
    itineraries {
      legs {
        mode
        distance
        startTime
        endTime
        route {
          url
          mode
          shortName
        }
        trip {
          gtfsId
          tripShortName
        }
        from {
          name
        }
        to {
          name
        }
      }
    }
  }
}
```

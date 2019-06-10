# Microgateway

Subscribes to a Kafka topic for real-time events, post-processes the message, and then streams the refined payload to a Websocket.

## Testing

Install and initialize an Apache Kafka cluster: https://kafka.apache.org/quickstart

Create a topic called: flightalerts

In a new terminal window establish a new Producer connection to the Kafka cluster's "flightalerts" topic


Start a local WS echo server:
```bash
go run main.go -server
```

Build the microgateway and start it with the following commands:

```bash
flogo create -f microgateway.json
cd microgateway
flogo build
bin/microgateway
```

In your Kafka producer session, send the following message:
```bash
{"Airline":"DL","FlightNumber":"1642","Aircraft":"B753","DepartingTo":"JFK","Duration":"4:25h","Route":"TRUKN2 SYRAH Q128 JSICA MLF DVC ALS TBE LBL RZC MEM HUTCC KNSAW RUSSA GLAVN1","ScheduledDeparture":"0900","EstimatedDeparture":"0930","Cabin":"Business: Lunch / Economy: Food for sale","Terminal":"C","Gate":"91","TaxiTime":"25m","AverageDelay":"10m"}
```

You should see your microgateway receive the Kafka event, post-process it to filter out content not needed by the downstream WS service, and then relay the new message to the local WS service:

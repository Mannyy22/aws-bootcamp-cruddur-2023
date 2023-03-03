# Week 2 — Distributed Tracing

## Todo list
- [x] Watch weekly videos
- [x] Instrument Honeycomb with OTEL
- [x] Instrument AWS X-Ray
- [x] Configure custom logger to send to CloudWatch Logs
- [x] Integrate Rollbar and capture and error

## Instrument Honeycomb with OTEL
This week we learned about Distributed Tracing. Distributed Tracing is basically how we track/manage all the request that go through our Software Systems


Setup OTEL_SERVICE_NAME in my docker-compose file:
![image](https://user-images.githubusercontent.com/46639580/222627605-5147bcbc-c4b4-409c-b91c-51c1535668a4.png)

My Honeycomb account
![image](https://user-images.githubusercontent.com/46639580/222625715-44b3442a-cea9-417a-afc6-b2283e2fc853.png)
When first setting up a Honeycomb, we had to add the following below to our `requirements.txt`

```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests
```
Then we installed it using these dependencies:

```sh
pip install -r requirements.txt
```

Then added Add to the following below to our `app.py` This setup tracing so we could send Data to honey comb

```py
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
```


```py
# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)
```

![image](https://user-images.githubusercontent.com/46639580/222627239-86c8a9be-9a0d-488f-a074-3a61a95043cf.png)

 Grabbed the API key from your honeycomb account and assigned enviroment variable:

```sh
export HONEYCOMB_API_KEY=""
gp env HONEYCOMB_API_KEY=""
```
![honey ENV](https://user-images.githubusercontent.com/46639580/222626817-30b8d8c1-c8e1-49ee-b718-1e0482d91da7.png)

After the above steps Data was finally able to get to Honeycomb and we were able to query the data and visualize it

![image](https://user-images.githubusercontent.com/46639580/222335529-a7936216-b90f-43a9-a576-858e2d216803.png)

![image](https://user-images.githubusercontent.com/46639580/222627474-ad0dd1e7-0f0e-406d-b86c-34f51b733133.png)

## Instrument AWS X-Ray

## Configure custom logger to send to CloudWatch Logs

## Integrate Rollbar and capture and error

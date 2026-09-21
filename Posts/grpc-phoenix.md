---
blog-title: OpenTelemetry gRPC Exporter
blog-published: 2025-04-12
blog-tags:
  - EN
  - TypeScript
  - LLM
---

This is an update / followup for last weeks post on [OpenTelemetry LLM App Tracing with Phoenix](https://marc-julian.com/blog/posts/phoenixllmtracing/) which ended with following statement:

> because I noticed some span exporting issues to Phoenix I tried a different span/traces exporter: `ZipkinExporter` with the [Zipkin UI](https://zipkin.io/) and there is still an [open discussion](https://github.com/Arize-ai/phoenix/discussions/7041#discussioncomment-12736322) on GitHub about the problems I noticed with Phoenix. Will update this post accordingly.

After further debugging (breaking at every single `span.end()` call) I was able to see that during some span creations, the error `Exception has occurred: Error: read ECONNRESET` and `Exception has occurred: Error: socket hang up` showed up. These spans were missing in Phoenix. [Roger Yang](https://github.com/RogerHYang) suggested to use the [experimental gRPC exporter](https://www.npmjs.com/package/@opentelemetry/exporter-trace-otlp-grpc) like this:

```ts
import { OTLPTraceExporter } from "@opentelemetry/exporter-trace-otlp-grpc";

const provider = new NodeTracerProvider({
  spanProcessors: [
    new SimpleSpanProcessor(
      new OTLPTraceExporter({
        url: "http://localhost:4317",
      }),
    ),
  ],
});

registerInstrumentations({
  instrumentations: [new OpenAIInstrumentation()],
});

provider.register();
```

This exports spans directly via gRPC to port `4317` of your Phoenix app. Make sure to use the [Docker images provided by Phoenix](https://docs.arize.com/phoenix/self-hosting) which automatically expose this port next to the default `6006` port.

With the above exporter configured, no spans go missing anymore and Phoenix captures even complex long running traces.
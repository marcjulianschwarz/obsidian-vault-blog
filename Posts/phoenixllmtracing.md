---
blog-title: OpenTelemetry LLM App Tracing with Phoenix
blog-tags:
  - EN
  - TypeScript
  - LLM
blog-published: 2025-04-06
---

So I wanted to setup tracing for an LLM app with [Phoenix](https://phoenix.arize.com/) and while doing so I had some learnings that I wanted to document:

- you have to take `SimpleSpanProcessor` by its name. It really only is a **simple** span processor. It is nice for development but for production you should be using the `BatchSpanProcessor` because it batches span exports, saving tons of network requests and probably avoiding some concurrency issues
- always make sure to end spans at the correct position, not too early, not too late, with `span.end()`. If you are using one central place to create spans, end them right there after you added attributes to it
- if you use `SemanticConventions`, make sure to check which attributes actually need to be set to get any visual on some dashboards (like Phoenix)
- tracing should be separated from business logic as much as possible. I am using a decorator for that specific purpose. It has access to the functions input, output and is called when the function it decorates is called without being in the way. This also makes it *really easy* to trace new functions.

Here is an example implementation of such a `Trace` decorator.

```ts
export function Trace<A, R>(
   traceName: string,
   traceFunction: (span: Span, args: A, result: R) => void,
 ) {
   return function (
     target: unknown,
     propertyKey: string,
     descriptor: PropertyDescriptor,
   ) {
     const originalMethod = descriptor.value;

     descriptor.value = async function (args: A) {
       return await tracer.startActiveSpan(traceName, async (span: Span) => {
         const result = await originalMethod.call(this, args);
         traceFunction(span, args, result);
         span.end();
         return result;
       });
     };
     return descriptor;
   };
 }
```

And here is an example on how you can use it. As you can see the business logic in `doingSomeWork()` is completely free of any tracing logic. Removing the `@Trace()` decorator completely removes any tracing from it.

```ts
type DoingSomeWorkParams = {
	input: string;
}

@Trace("Doing Some Work", DoingSomeWorkParams, string)
async function doingSomeWork(args: DoingSomeWorkParams): Promise<string> {
	// ... doing some work with args.input
	return result
}

function traceDoingSomeWork(span: Span, args: DoingSomeWorkParams, result: string) {
	span.setAttributes({
     [SemanticConventions.OPENINFERENCE_SPAN_KIND]: OpenInferenceSpanKind.AGENT,
     [SemanticConventions.INPUT_VALUE]: args.input,
     [SemanticConventions.OUTPUT_VALUE]: result,
    });
}
```

Going on with the list of things I learned:
- either use a `ConsoleSpanExporter` or `diag.setLogger(new DiagConsoleLogger(), DiagLogLevel.DEBUG);` to see a lot of details about the tracing. Especially interesting are the `traceId`, `parentId` and `id` attributes of each span. Here you can check whether the context got propagated nicely between the spans.
- because I noticed some span exporting issues to Phoenix I tried a different span/traces exporter: `ZipkinExporter` with the [Zipkin UI](https://zipkin.io/) and there is still an [open discussion](https://github.com/Arize-ai/phoenix/discussions/7041#discussioncomment-12736322) on GitHub about the problems I noticed with Phoenix. Will update this post accordingly.






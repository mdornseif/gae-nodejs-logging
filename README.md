# gae-nodejs-logging
Logging on Google App Engine Node.js Standard Environment

Google App Engine Python Standard Environment provides nice logging where all log entries related to a HTTP request are grouped. 
For Node.js stdout and stderr are collected in the log viewer but with out grouping - on entry per line.

Google suggests:

> To write log entries, we recommend that you integrate the Bunyan or Winston plugins with Cloud Logging.  ([source](https://cloud.google.com/appengine/docs/standard/nodejs/writing-application-logs#writing_app_logs))

But using an in-process cloud logging client is generally an problematic approach for the async / single process server approach offered by node.js / Express.

Logging to stdout / stderr and transporting logs from there to the log viewer in a separate process provides much better decoupeling.  This shoud be possible, as stated by Google:

> you can send simple text strings to stdout and stderr. The strings will appear as messages in the Logs Viewer, the command line, and the Cloud Logging API, and will be associated with the App Engine service and version that emitted them.
> If you want to filter these strings in the Logs Viewer by severity level, you need to format them as structured data. For more information, see Structured logging.
> If you want to correlate entries in the app log with the request log, your structured app log entries need to contain the request's trace identifier. You can extract the trace identifier from the X-Cloud-Trace-Context request header. In your structured log entry, write the ID to a field named logging.googleapis.com/trace. For more information about the X-Cloud-Trace-Context header, see Forcing a request to be traced.
> See an example of writing structured log entries with a trace ID in the Cloud Run documentation. You can use the same technique in your App Engine apps. ([source(https://cloud.google.com/appengine/docs/standard/nodejs/writing-application-logs#writing_structured_logs))

So this sould be easy. Documentation for `trace` explains:

> Optional. Resource name of the trace associated with the log entry, if any. If it contains a relative resource name, the name is assumed to be relative to //tracing.googleapis.com. Example: projects/my-projectid/traces/06796866738c859f2f19b7cfb3214824

Other interisting doumentation is:

* [severity](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logseverity)
* [httpRequest](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest)
* [operation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentryoperation)



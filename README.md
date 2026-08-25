# Unity Build public API

This repository distributes the curated `UBEAPIService` protobuf contract for
Unity Build integrations. It contains the service and the service-free financial
reporting types required to compile it; private QBO and control-plane services are
not part of this module.

## Authentication

Send a user-bound Unity Build API key as gRPC metadata:

```text
authorization: Bearer ube_live_…
```

`x-api-key` is also accepted. Request-body `apiKey` fields are deprecated and
must not be used for new integrations.

## Generate clients

Install [Buf](https://buf.build/docs/cli/installation/) and run:

```text
buf generate
```

The checked-in generation configuration produces Go, Python, and TypeScript
bindings. Pin a tagged release for production integrations.

The contract is published automatically to the public
[`buf.build/unity-build/api`](https://buf.build/unity-build/api) module. No
enable/disable repository variable is required. Tagged GitHub releases remain an
additional supported distribution channel.

Developer documentation is available at
[unitybuildestimation.com/api](https://unitybuildestimation.com/api).

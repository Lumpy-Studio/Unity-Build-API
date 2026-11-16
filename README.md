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
must not be used for new integrations. When metadata is supplied it is
authoritative; an invalid metadata credential is never replaced by a body key.

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

`APIListAgentCapabilities` and `APIDescribeOperation` are retained for v1 wire
compatibility but deprecated. Generated SDK and BSR descriptors are the supported
discovery mechanism for this curated, strongly typed API.

## License

The public protobuf contract is licensed under the Apache License 2.0. See
[`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).

Developer documentation is available at
[unitybuildestimation.com/api](https://unitybuildestimation.com/api).

## Account user identity lookup

Administrator-authorized delegated company keys can call `GetAccountUsers` to
retrieve account members as email/`userId` pairs, or `GetAccountUser` with an exact
case-insensitive email to retrieve a single `userId`. Both require API-key
metadata and derive the account from that key. IDs can be cached. An ID is not an
authorization credential; inactive or unlinked users cannot use delegated MCP.

Use `X-API-Key` plus `X-Unity-User-ID` on every MCP request. These directory RPCs
are gRPC methods, not REST routes, and do not administer users. Delegation must
be explicitly enabled by an account administrator on a newly created key. Keep
that key in the company's trusted backend, which authenticates the represented
user. Unity enforces the represented user's current permissions and project
access. No extra OAuth connection is required for this integration mode.

## Change-order intake submission

`CreateChangeOrderIntake` accepts a stable request UUID, project UUID, title and
description using `x-api-key` plus `x-unity-user-id` metadata. Optional requester,
urgency, work-status, supporting-link and complete contract/trade scope fields
are described in the protobuf contract. It creates an ordinary **New** Intake
Staging record; Unity handles review and conversion to formal change orders.
It does not create a change order directly. The represented user's current
change-order-create and project-read/access permissions apply on every request.

Keep the exact request payload and UUID for retries. The response contains
`intakeId`, `receiptId`, `state` and `replayed`; retries return the original IDs,
with the current state or `committed_unavailable` after intake deletion. A changed
payload under the same key/user/request identity is rejected. Raw attachments
are outside this RPC; use a supporting link or existing Unity attachment flows.

# Changelog

## 1.0.7

- Standardize public RPC authentication failures on canonical gRPC statuses.
- Restrict deprecated capability discovery to the published UBEAPI contract.
- Require successful Buf Schema Registry publication for every public release.
- License the public contract under Apache-2.0.

## 1.0.3

- Restore public documentation comments for the extracted financial reporting types.

## 1.0.1

- Remove backend-local compilation instructions from the distributed source header.

## 1.0.0

- Initial supported distribution of the existing `UBEAPIService` contract.
- API keys are documented through request metadata; legacy request-body fields
  remain wire-compatible but deprecated.

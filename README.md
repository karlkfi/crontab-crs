# crontab-crs

This repo includes a namespace and 5000 CronTab CRs under the namespace.

- `configs/ns.yaml` defines the `crontab-ns` Namespace
- `configs/crs.yaml` defines CronTab CRs `cr-1` to `cr-5000`

## Usage

If you're using GKE, the CronTab CRD should already be installed. Otherwise,
apply it with `kubectl apply -f ./crontab-crd.yaml`.

Apply the `root-sync` RootSync with `kubectl apply -f ./root-sync-cr.yaml`.

## Error Cases

### ResourceGroup failed to apply

With 5001 objects in the source config directory, Config Sync will error.
GKE supports etcd objects up to 3MiB. This can result in one of two errors,
either from the API Server validating the API request size, or the etcd storage
layer validating the object size in JSON.

### Failed to create typed patch object

If you add an extra field to the CronTab CRs, Config Sync will error.
For example:

`KNV2009: failed to apply CronTab.stable.example.com, crontab-ns/cr-1320: failed to create typed patch object (crontab-ns/cr-1320; stable.example.com/v1, Kind=CronTab): .spec.extra: field not declared in schema`

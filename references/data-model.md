# Data Model and Statuses

## Store Fields

Each store should keep at least:

- `brand`: fixed `迷客夏`
- `storeName`
- `county`
- `district`
- `address`
- `phone`
- `hours`
- `officialSourceUrl`
- `officialPlannedSourceUrl`
- `officialMapUrl`
- `gmbUrl`
- `gmbStatus`
- `takeoutAvailable`
- `deliveryAvailable`
- `takeoutProviders`
- `deliveryProviders`
- `otherProviders`
- `evidenceNotes`
- `checkedAt`

Provider-audit scripts may add:

- `providerEvidenceUrl`
- `orderAuditStatus`
- `nidinOrderStatus`
- `nidinStoreId`
- `nidinOrderUrl`

## Summary Fields

`data/summary.json` should include:

- `generatedAt`
- `officialStoreCount`
- `gmbFoundCount`
- `takeoutCount`
- `deliveryCount`
- `unknownCount`
- `providerCounts`
- `takeoutProviderCounts`
- `deliveryProviderCounts`
- `otherProviderCounts`
- `gmbStatusCounts`
- `source`
- `nidinMatchedCount`
- `nidinSource`

`providerCounts` is a compatibility overview. Use the split counts for charts and analysis.

## Status Values

GMB status:

- `confirmed`
- `no_gmb_found`
- `closed_or_moved`
- `unavailable_or_blocked`
- `needs_manual_review`

Order audit status:

- `delivery_confirmed_by_footinder`
- `needs_manual_review`
- `unavailable_or_blocked`

Nidin order status:

- `confirmed`
- `not_found`

## Counting Rules

- `takeoutCount`: stores where `takeoutAvailable is True`
- `deliveryCount`: stores where `deliveryAvailable is True`
- `unknownCount`: stores where either takeout or delivery is still `None`
- Provider counts: count a provider once per store, not once per evidence URL.

## Evidence Rules

Prefer public evidence URLs in this order:

1. `nidinOrderUrl`
2. `providerEvidenceUrl`
3. `gmbUrl`
4. `officialSourceUrl`

If evidence conflicts, do not overwrite confirmed data silently. Add a note and prefer official Milksha/Nidin source for official ordering availability.

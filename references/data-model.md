# Data Model and Statuses

## Store Fields

Each store should keep at least:

- `brand`: fixed `迷客夏`
- `storeName`
- `county`: normalized Taiwan county/city name, such as `台北市` or `新竹縣`
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
- `region`: one of the configured analysis regions, when applicable
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
- `countyCounts`
- `regionCounts`
- `regionDefinitions`
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

## County and Region Rules

Normalize Taiwan county/city names before counting:

- Convert `臺` to `台`.
- Correct known variants such as `桃園巿` to `桃園市`.
- Prefer address-derived county when available, because it can split ambiguous groups such as `新竹門市` into `新竹市` and `新竹縣`.
- Keep all 22 county/city buckets in the site map output, even when the count is `0`.

Default regional grouping:

- `台北基宜`: `台北市`, `新北市`, `基隆市`, `宜蘭縣`
- `桃竹苗`: `桃園市`, `新竹市`, `新竹縣`, `苗栗縣`
- `中彰投`: `台中市`, `彰化縣`, `南投縣`
- `雲嘉南`: `雲林縣`, `嘉義市`, `嘉義縣`, `台南市`
- `高屏`: `高雄市`, `屏東縣`
- `花東`: `花蓮縣`, `台東縣`

Map and region counts should respect the active site filter. With no county filter, county counts should sum to `officialStoreCount`; after filtering to one county group, the displayed map and region cards should sum to the filtered store count. If stores exist in `澎湖縣`, `金門縣`, or `連江縣`, include them in the county map and either show an explicit `離島` bucket or document that six-region cards exclude offshore counties.

## Evidence Rules

Prefer public evidence URLs in this order:

1. `nidinOrderUrl`
2. `providerEvidenceUrl`
3. `gmbUrl`
4. `officialSourceUrl`

If evidence conflicts, do not overwrite confirmed data silently. Add a note and prefer official Milksha/Nidin source for official ordering availability.

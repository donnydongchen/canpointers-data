# CANPointers Data Feed

Live data feed for the [CANPointers](https://github.com/donnydongchen/CANPointers) iOS app (private repo): Canadian credit card earn rates, points-program valuations, and welcome offers.

The app fetches this file on launch and via Settings → Refresh:

```
https://raw.githubusercontent.com/donnydongchen/canpointers-data/main/feed.json
```

A feed is only adopted by the app when its `asOf` date is **newer** than the currently loaded one and `schemaVersion` is 1.

## Updating the data

1. Edit `feed.json` — earn rates, caps, valuations, offers. Keep per-card `sourceURL` pointing at the issuer page you verified against.
2. Bump `asOf` to today's date (`yyyy-MM-dd`).
3. Validate: copy the file into the app repo at `Sources/CANPointersKit/Resources/feed.json` and run `swift run kit-tests` (checks references, rates, caps, formats).
4. Commit and push here. Every installed app picks it up on next launch — no App Store release needed.

## Schema (v1)

- `programs[]` — `id`, `name`, `centsPerPoint` (CAD average redemption value), `notes`
- `cards[]` — `id`, `name`, `issuer`, `network`, `programID` (`null` = cash back), `annualFeeCAD`, `baseRate`, `earnRates[]` (`category`, `rate`, `capAnnualSpendCAD`, `notes`), `sourceURL`
- `offers[]` — `id`, `cardID`, `headline`, `spendRequirement`, `expiry` (`yyyy-MM-dd` or `null`), `estimatedFirstYearValueCAD`, `sourceURL`

This file contains only public information (published card rates and offers). Not financial advice — verify with issuers.

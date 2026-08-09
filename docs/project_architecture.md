# Project Architecture

## Data flow

```text
BTS DB1B / DB1C
        ↓
Base Parquet preparation
        ↓
Normalized Itineraries
        ↓
Physical Segments
        ↓
Aggregated cubes
        ↓
Final Analytics
        ├── Itineraries
        ├── Segments
        ├── bridge_od_city_segment
        └── Dimensions
```

## Layer responsibilities

| Layer | Responsibility | Result |
|---|---|---|
| Program 1 | Ingestion, archiving, extraction and base Parquet preparation | Traceable BTS base files |
| Program 2 — Itineraries | Build the complete purchased O&D journey | Itinerary-level records |
| Program 2 — Segments | Expand itineraries into physical legs | Segment-level records linked to their source itinerary |
| Program 3 | YAML-defined aggregation | Monthly and quarterly analytical cubes |
| Program 4 | Final summaries, mappings, concentration metrics and scores | Itinerary, segment, bridge and dimension outputs |

## Two different relationships

The architecture contains two useful levels of connection between itineraries and segments.

### Row-level relationship

At the clean-data level, each generated segment can retain `itinerary_id`, allowing a physical segment row to be traced back to the individual itinerary from which it was created.

### Analytical relationship

At the final Analytics level, `bridge_od_city_segment` aggregates the relationship to:

**period + O&D city market + physical segment**

This is the correct level for interactive market/network analysis because it provides additive traffic measures and two directional shares.

## Why the bridge is not a dimension

Dimensions contain descriptive or filtering attributes such as airport descriptions, city-market descriptions, WAC descriptions and periods.

The bridge instead contains:

- `segment_passengers`;
- `segment_passenger_distance`;
- `segment_mix_share_within_od`;
- `od_feed_share_within_segment`.

It therefore represents a many-to-many analytical fact relationship and is kept with the segment/integrated outputs.

## Final outputs

```text
examples/
├── itineraries/
│   ├── market_core_summary.csv
│   ├── market_competition_summary.csv
│   ├── market_connectivity_summary.csv
│   ├── market_booking_summary.csv
│   ├── market_airport_summary.csv
│   ├── market_wac_summary.csv
│   ├── market_seasonality_summary.csv
│   └── market_opportunity_summary.csv
├── segments/
│   ├── segment_core_summary.csv
│   ├── segment_competition_summary.csv
│   ├── segment_feed_summary.csv
│   ├── market_segment_summary.csv
│   ├── market_connection_summary.csv
│   ├── segment_network_summary.csv
│   └── bridge_od_city_segment.csv
└── dimensions/
    ├── dim_airport.csv
    ├── dim_city_market.csv
    ├── dim_wac.csv
    └── dim_period.csv
```

The production pipeline uses Parquet. The CSVs in this repository are lightweight five-row examples for GitHub preview.

# Project Overview

## Purpose

**Traffic Market & Network Analysis** presents the expanded analytical results of the original Traffic Market Analysis project. The emphasis is on what can now be understood from the data rather than on exposing the processing code.

The analytical model separates:

- **Itineraries** — purchased O&D demand;
- **Segments** — physical network use;
- **Dimensions** — shared geographic and temporal references;
- **O&D–segment bridge** — the measurable many-to-many relationship between city markets and physical legs.

## Main analytical layers

### Itineraries

Used for passengers, revenue, fare, yield, booking, competition, connectivity, seasonality and opportunity rankings.

### Segments

Used for passenger-segment traffic, passenger-distance, marketing/operating carriers, connection dependency, O&D feeds, hubs and network scores.

### Integrated relationship

`bridge_od_city_segment` links an aggregated O&D city market to the physical segments used by that market.

Its two shares support both directions:

- segment importance within the O&D market;
- O&D feed importance within the segment.

This complements the row-level `itinerary_id` relationship available earlier in the pipeline.

### Dimensions

Airport, city-market, WAC and period lookup tables support common filtering and interpretation. The bridge is not placed here because it contains traffic measures and shares rather than descriptive attributes.

## Detailed documentation

- [Itineraries Analytics](itineraries_analytics.md)
- [Segments Analytics](segments_analytics.md)
- [Integrated Market & Network Analytics](integrated_analytics.md)
- [Possible Uses](possible_uses.md)
- [Project Architecture](project_architecture.md)

## Interpretation limits

- itinerary passengers and passenger-segments have different grains;
- ticket revenue is not assigned to segments;
- monthly and quarterly records require `period_type` filtering;
- scores are relative rankings rather than predictions;
- same-origin/destination flags do not explain the operational cause of a record.

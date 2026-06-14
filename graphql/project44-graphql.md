# project44 GraphQL Schema

## Overview

This conceptual GraphQL schema represents the domain model for the project44 supply chain visibility platform. project44 connects, automates, and provides predictive analytics across multimodal shipments worldwide. The platform covers shipment tracking, LTL (Less-than-Truckload), TL (Truckload), multimodal services, capacity management, yard management system (YMS) appointments, and webhook event delivery.

The schema is derived from the project44 REST API v4 surface (https://developers.project44.com/) and models the core entities, relationships, and operations available through the platform.

## Schema Source

- **Type:** Conceptual GraphQL schema
- **Derived from:** project44 REST API v4 documentation and OpenAPI specification
- **API Documentation:** https://developers.project44.com/
- **Base URL (Americas):** https://na12.api.project44.com/api/v4
- **Base URL (Europe):** https://eu12.api.project44.com/api/v4

## Named Types

The schema defines 57 named types covering the full project44 domain:

### Shipment and Tracking
- `Shipment` — core shipment record with identifiers, status, and relationships
- `ShipmentTrackingData` — aggregated tracking data for a shipment
- `TrackingEvent` — individual position or status event along the route
- `StatusUpdate` — state transition record for a shipment
- `StatusHistory` — ordered list of all status updates for a shipment
- `ETAWindow` — estimated time of arrival range with confidence score
- `Milestone` — named checkpoint in the shipment lifecycle
- `MilestonePlan` — planned sequence of milestones for a shipment
- `Deviation` — variance from plan (time, location, or condition)
- `Alert` — triggered notification based on a configured rule
- `AlertConfig` — alert configuration attached to a shipment or lane
- `AlertRule` — reusable rule definition for alert triggers
- `SensorData` — IoT sensor reading (temperature, humidity, shock)
- `Temperature` — temperature reading with unit and timestamp

### Location and Geography
- `Location` — named place with address and coordinates
- `Address` — structured postal address
- `GeoCoordinate` — latitude/longitude pair with optional accuracy
- `AccessPoint` — facility or terminal where cargo can be accessed
- `Pickup` — scheduled or actual pickup event at origin
- `Delivery` — scheduled or actual delivery event at destination
- `POD` — proof of delivery record

### Carriers and Modes
- `Carrier` — carrier entity with SCAC, DOT, MC, and contact information
- `CarrierContract` — contract terms between a shipper and carrier
- `Mode` — enumeration of transport modes
- `TransportMode` — detailed transport mode record with attributes

### Freight and Capacity
- `Container` — intermodal container record with type and current status
- `ContainerEvent` — event specific to a container (gate-in, gate-out, loaded, discharged)
- `OrderTender` — tender of a freight order to a carrier
- `LoadTender` — EDI 204-equivalent load tender record
- `FreightBid` — bid submitted by a carrier in response to a rate request
- `FreightBidLine` — individual line item within a freight bid
- `FreightAward` — award of a freight bid to a carrier
- `LaneRate` — contracted or spot rate for a lane
- `LaneRateLine` — accessorial or line-item component of a lane rate
- `RateQuote` — real-time rate quote for a shipment move
- `RateRule` — rule governing how rates are calculated or applied

### Documents and Compliance
- `DocumentUpload` — uploaded document (BOL, POD, invoice) attached to a shipment
- `Customs` — customs record for an international shipment
- `CustomsDeclaration` — individual declaration line within a customs record
- `Commodity` — commodity description including weight, dimensions, and classification
- `HazMat` — hazardous materials declaration record

### Platform and Integration
- `Organization` — customer organization account on the platform
- `Team` — team within an organization with scoped permissions
- `Role` — named role with permission grants
- `Integration` — configured third-party system integration
- `Connector` — reusable connector definition for an integration type
- `APIToken` — API access token with scope and expiry
- `Webhook` — registered webhook endpoint
- `WebhookSubscription` — subscription linking a webhook to event types
- `API` — API surface descriptor for platform introspection

## Queries

- `shipment(id: ID!)` — fetch a single shipment by identifier
- `shipments(filter: ShipmentFilter, page: Int, pageSize: Int)` — paginated shipment list
- `trackingData(shipmentId: ID!)` — retrieve all tracking data for a shipment
- `carrier(scac: String!)` — look up a carrier by SCAC code
- `carriers(filter: CarrierFilter)` — search carriers
- `rateQuote(input: RateQuoteInput!)` — request a real-time rate quote
- `laneRates(origin: AddressInput!, destination: AddressInput!, mode: Mode)` — query lane rates
- `alerts(shipmentId: ID)` — list alerts, optionally scoped to a shipment
- `webhooks` — list registered webhooks for the authenticated organization
- `organization` — retrieve the authenticated organization record

## Mutations

- `createShipment(input: ShipmentInput!)` — create a new shipment record
- `updateShipment(id: ID!, input: ShipmentUpdateInput!)` — update shipment details
- `cancelShipment(id: ID!)` — cancel an active shipment
- `createOrderTender(input: OrderTenderInput!)` — tender a freight order
- `createAlertConfig(input: AlertConfigInput!)` — configure alerts for a shipment
- `registerWebhook(input: WebhookInput!)` — register a new webhook endpoint
- `deleteWebhook(id: ID!)` — remove a webhook registration
- `uploadDocument(input: DocumentUploadInput!)` — attach a document to a shipment
- `createAPIToken(input: APITokenInput!)` — generate a new API token

## Subscriptions

- `shipmentStatusUpdated(shipmentId: ID!)` — real-time status change events
- `trackingEventReceived(shipmentId: ID!)` — real-time tracking position events
- `alertTriggered(organizationId: ID!)` — real-time alert delivery

## Authentication

project44 uses OAuth 2.0 client credentials flow. Obtain a bearer token from the token endpoint and include it as `Authorization: Bearer <token>` on all requests. See https://developers.project44.com/authentication for details.

## Regions

project44 operates region-specific API clusters:
- **Americas:** `na12.api.project44.com`
- **Europe:** `eu12.api.project44.com`

Sandbox environments mirror production endpoints for each region.

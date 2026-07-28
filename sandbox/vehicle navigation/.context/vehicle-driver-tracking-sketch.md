# Vehicle & Driver Tracking — Courier Company
# Scaffold: all four perspectives active
# Context: web dispatcher dashboard + driver mobile app · segmented geo points

---

## STORIES

# Track Vehicles and Drivers                                              < scaffold

## Assign Fleet Resources                                                 < scaffold

## Track Route Progress

### Driver --> Capture Route Geo Point
given an active TrackingSession with an open SegmentTrail

#### geo point recorded on active segment
given a TrackingSession with status active
    and a SegmentTrail with status active
when the Driver records a geo point with lat lng capturedAt
then the GeoPoint is appended to the SegmentTrail.points
    and the GeoPoint.sequenceIndex is assigned from points.count
    and the PositionState.currentLat and currentLng are updated

#### geo point rejected when no active session
given no TrackingSession for the vehicle
when the Driver records a geo point
then the geo point is rejected with reason no_active_session

### Driver --> Complete Delivery Segment
given a SegmentTrail with status active and at least one GeoPoint

#### segment closed on stop completion
given a SegmentTrail with status active
when the Driver completes the stop
then the SegmentTrail.status becomes closed
    and a DeliveryEvent is created with type segment_completed
    and the next RouteSegment becomes the active SegmentTrail

#### segment completion blocked when trail is empty
given a SegmentTrail with status active and no points
when the Driver attempts to complete the stop
then the completion is blocked with reason no_points_recorded

### Dispatcher --> View Vehicle Position on Map
* approx 2–3 more stories (refresh live position, filter by vehicle, select trail history)

### Driver --> View Segment Trail on Map
* approx 1–2 more stories (see recorded trail dots, zoom to current position)

~> Increment 1: Driver can record geo points and close a segment: Capture Route Geo Point, Complete Delivery Segment

## Monitor Live Fleet                                                     < scaffold
## Report Delivery Events                                                 < scaffold
## Manage Geo Segments                                                    < scaffold

---

## BDD

# a vehicle assigned to a route with geo segments                        < scaffold

# a driver reporting a position geo point
  ## that the driver is on an active route segment
    it should record the geo point under that segment's trail
    it should advance the vehicle's current position
    with the segment trail already containing points
      it should assign the next sequenceIndex in order
  ## that the driver submits a geo point with no active session
    it should reject the point with no_active_session
  ## that the reported point falls outside the segment boundary
    it should still record the geo point
    it should flag the segment trail status as deviated

# a dispatcher viewing the live fleet map                                < scaffold
# a delivery event recorded at a segment boundary                        < scaffold
# a route deviation detected from the planned path                       < scaffold

---

## MODULES

# fleet/                                                                 < scaffold  // vehicle & driver registry
# routes/                                                                < scaffold  // route plans and geo segment definitions

# tracking/
  ## GeoPoint
    lat
    lng
    capturedAt
    sequenceIndex

  ## SegmentTrail
    routeSegmentId
    vehicleId
    driverId
    points                   // ordered list of GeoPoint
    status                   // active | closed | deviated
    appendPoint geoPoint
      // assigns sequenceIndex = points.count before appending
    close
    flagDeviation

  ## TrackingSession
    vehicleId
    driverId
    activeSegmentTrail
    startSegment routeSegmentId
    recordPoint lat lng capturedAt
      -> activeSegmentTrail.appendPoint
      -> positionState.update
    closeSegment
      -> activeSegmentTrail.close

  ## PositionState
    vehicleId
    currentLat
    currentLng
    lastUpdatedAt
    segmentTrailRef
    update geoPoint

# dispatch/                                                              < scaffold  // job assignment and scheduling
# notifications/                                                         < scaffold  // deviation alerts and status events
# reporting/                                                             < scaffold  // delivery event log and audit trail

---

## UX

Fidelity: ia

Dispatcher Dashboard                                                      < scaffold
  ├─ [top nav] select vehicle ──────────────→ Vehicle Detail              < scaffold
  ├─ [top nav] open fleet list ─────────────→ Fleet List                  < scaffold
  └─ [action] open route detail ────────────→ Route Detail                < scaffold

Fleet List                                                                < scaffold
  └─ [action] select vehicle ───────────────→ Vehicle Detail              < scaffold

Vehicle Detail                                                            < scaffold
  └─ [action] view route ───────────────────→ Route Detail                < scaffold

Route Detail                                                              < scaffold

Driver Mobile — Active Route
  ├─ [action] capture geo point ────────────→ Driver Mobile — Geo Capture
  └─ [action] complete stop ────────────────→ Driver Mobile — Event Log

Driver Mobile — Geo Capture                                               < scaffold
  └─ [system] submit confirmed ─────────────→ Driver Mobile — Active Route < scaffold

Driver Mobile — Event Log                                                 < scaffold
  └─ [action] back ─────────────────────────→ Driver Mobile — Active Route < scaffold

═══════════════════════════════════════════════════════════
  SCREENS
═══════════════════════════════════════════════════════════

[ Driver Mobile — Active Route ]                         dashboard
  ┌─────────────────────────────────┐
  │ Route R-042  ·  Seg 2 of 5      │
  │ Next stop: 14 Maple St          │
  ├─────────────────────────────────┤
  │ [map: segment trail dots        │
  │  + current position pin]        │
  ├─────────────────────────────────┤
  │ Points recorded: 4              │
  │ [ Capture Point ] [ Complete Stop ] │
  └─────────────────────────────────┘
  Stories (~4): Capture Route Geo Point · Complete Delivery Segment · View Segment Trail · View Next Stop
  Domain terms: geo point · segment trail · route segment · stop
  key:
    [ btn ] button · map = read-only segment trail view
    on [ Capture Point ] → Driver Mobile — Geo Capture (records lat/lng)
    on [ Complete Stop ] → Driver Mobile — Event Log (closes segment, opens next)

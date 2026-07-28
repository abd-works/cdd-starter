# Vehicle & Driver Tracking System — Sketch
> Small fleet (≤50 vehicles) · Driver mobile app → GPS pings · Track-only with geographic segments
> Active perspectives: Stories · Modules · BDD · UX

---

## Stories

# Track Courier Fleet
* approx 12–16 total stories

## Broadcast Live Position

### Driver --> Start Shift
given a Driver with status offline and an assigned Vehicle

#### shift started places vehicle on live map
given a Driver with status offline
    and a Vehicle assigned to the Driver
when the Driver starts their shift
then the Driver.status becomes online
    and the Vehicle appears on the Live Map as an active pin

#### shift start rejected when no vehicle assigned
given a Driver with no assigned Vehicle
when the Driver starts their shift
then the shift is rejected with reason no_vehicle_assigned

### Driver --> Send Location Ping
given a Driver with status online and an assigned Vehicle

#### ping recorded and position updated
given a Driver with status online
    and a Vehicle with a last known position
when the Driver app sends a location ping with coordinates
then the Vehicle.position is updated to the new coordinates
    and the Vehicle.last_seen timestamp is updated

#### ping discarded when driver is offline
given a Driver with status offline
when the Driver app sends a location ping
then the ping is discarded
    and the Vehicle.position is unchanged

### Dispatcher --> View Live Vehicle Positions

#### live map shows all active vehicle pins
given one or more Vehicles with status active
    and each Vehicle has a known position
when the Dispatcher opens the Live Map
then each active Vehicle is shown as a pin at its last known position
    and each pin shows Vehicle.id and Driver.name

## Record Position History
* approx 3–4 more stories (trail replay, history window, stale pin detection)

## Monitor Geographic Segments
* approx 3–4 more stories (vehicle enters segment, exits segment, unassigned)

## Manage Fleet Configuration
* approx 4–5 more stories (assign vehicle to driver, add vehicle, retire vehicle)

~> Increment 1: Dispatcher can see active vehicles on the live map: Start Shift, Send Location Ping, View Live Vehicle Positions

---

## Modules

# tracking/
  ## LocationPing
    driverId
    vehicleId
    coordinates
    receivedAt

  ## VehiclePosition
    vehicleId
    coordinates
    lastSeen
    segmentId                              // null when outside all known segments
    updateFrom ping
    -> SegmentLocator.findSegment coordinates

  ## PingIngester
    accept ping
    -> VehiclePosition.updateFrom ping
    -> PositionStore.save position
    // discard ping if driver.status is offline

  ## PositionStore
    save position
    findByVehicle vehicleId
    listActive

# segments/                                                               < scaffold  // define and evaluate geographic zones
# drivers/                                                                < scaffold  // driver identity and session state
# dispatch/                                                               < scaffold  // dispatcher read model and live view

---

## BDD

# a driver location ping
  ## that is received from the driver app
    it should record the vehicle's current coordinates
    it should update the vehicle's last-seen timestamp
    with the driver offline
      it should be discarded without updating position
    with coordinates inside a geographic segment
      it should mark the vehicle as within that segment
    with coordinates outside all known segments
      it should clear the vehicle's segment assignment

# a vehicle inside a geographic segment                                   < scaffold
# a vehicle that has left a geographic segment                            < scaffold
# a driver session                                                        < scaffold
# the dispatcher live map                                                 < scaffold

---

## UX

Fidelity: ia

Live Map
  ├─ [action] select vehicle pin ──────→ Vehicle Detail panel
  ├─ [action] select segment boundary ─→ Segment Detail panel
  └─ [top nav] Segments ───────────────→ Segment Manager

Segment Manager                                                           < scaffold
  ├─ [action] create segment ──────────→ Segment Editor                  < scaffold
  ├─ [action] edit segment ────────────→ Segment Editor                  < scaffold
  └─ [top nav] Live Map ───────────────→ Live Map                        < scaffold

Segment Editor                                                            < scaffold
  └─ [action] save ────────────────────→ Segment Manager                 < scaffold

Driver Session Panel                                                      < scaffold
  └─ [system] session ends ────────────→ Live Map                        < scaffold

═══════════════════════════════════════════════════════════
  SCREENS
═══════════════════════════════════════════════════════════

[ Live Map ]                                                 dashboard
  ┌─────────────────────────────────────────────────────────┐
  │ Live Map          [ Segments ]                          │  top nav
  ├──────────┬──────────────────────────────────────────────┤
  │ Vehicles │  map canvas · vehicle pins · segment zones   │
  │ › V-12 ‹ │                                              │
  │   V-07   │   ○ pin: V-12 · A. Smith  (selected)        │
  │   V-03   │   · pin: V-07 · B. Jones                    │
  │          │   · pin: V-03 · C. Patel                    │
  │          │   ─── segment boundary: City Centre ───      │
  └──────────┴──────────────────────────────────────────────┘
  Stories (~4): Start Shift · Send Location Ping · View Live Vehicle Positions · End Shift
  Domain terms: vehicle pin · segment zone · last-seen · active vehicle
  key:
    left panel = active vehicle list · right = map canvas
    ›sel‹ selected vehicle · ○ pin selected state
    on vehicle pin → Vehicle Detail panel (flyout)
    on segment boundary → Segment Detail panel (flyout)
    on [ Segments ] → Segment Manager

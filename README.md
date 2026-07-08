# CustomMapsDemo

**Build indoor navigation for any building — no beacons, no backend, no GPS required.**

customMaps is a toolkit for turning ordinary floor plan images into fully navigable indoor maps. You draw the map once in a web editor, then export it as a self-contained viewer that gives users turn-by-turn walking directions on the web or in a React Native app — including live positioning indoors, where GPS doesn't work.

## How it works

### 1. Map Editor (web)

A React + Vite app for authoring maps from scratch:

- **Upload floor plans** — photos or scans work; a built-in cleanup tool converts messy scanned floor plans into crisp, consistent line art so multi-floor maps look uniform.
- **Draw the navigation graph** — place points (rooms, hallways, stairs, elevators) and connect them with edges. The editor validates the graph so you can't export a map with unreachable rooms.
- **Multi-floor support** — stairs and elevators link floors, so routes can span the whole building.
- **Georeferencing** — optionally align the map to real-world coordinates on an interactive map, enabling GPS-assisted positioning near entrances.
- **Export anywhere**:
  - a **standalone web viewer** (ZIP of plain HTML/JS — unzip and open, no build step), with an optional **kiosk mode** for lobby touchscreens
  - a **drop-in React Native `MapScreen` component** for mobile apps
  - raw JSON for custom integrations

### 2. Map Viewer (mobile + web)

The exported viewer handles the actual navigation:

- **Search & route** — pick a destination, get the shortest path via Dijkstra's algorithm, rendered on the floor plan with turn-by-turn directions ("Turn left toward the stairs, go up to Floor 2…").
- **Multi-floor routing** — the view automatically switches floors as the route progresses.
- **Live indoor positioning without GPS** — a pedestrian dead reckoning (PDR) system uses the phone's accelerometer for step detection and the compass for heading, advancing your position along the route and snapping it to the nearest hallway. Map scale and north alignment from the editor make the step-to-pixel conversion accurate.
- **Compass guidance** — a live heading indicator shows which way to turn to face the route.
- **GPS handoff** — outdoors or near entrances, georeferenced maps can use real GPS before PDR takes over inside.

## Repository layout

| Path | What it is |
|---|---|
| `app/editor/` | The web-based map editor (React + Vite) |
| `app/` | Expo (React Native) app hosting the mobile viewer |
| `viewer/` | The standalone `MapScreen` viewer component and navigation utilities |
| `test-app/` | Expo test bed app with an exported `MapScreen` bundle |

## Tech

React, React Native (Expo), react-native-svg, Vite, Leaflet (georeferencing), JSZip (exports). Pathfinding, direction generation, geometry, dead reckoning, and image processing are all implemented from scratch — no mapping SDK required.

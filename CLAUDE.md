# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static web application that displays an interactive map with Maidenhead grid square overlays. The entire application is contained within a single `index.html` file with embedded JavaScript and CSS.

## Architecture

### Key Components

1. **GridSquare Class** - Core logic for Maidenhead grid calculations
   - Located in index.html:45-208
   - Handles encoding lat/lon to grid squares
   - Handles decoding grid squares to boundaries
   - Supports 4, 6, and 8-character precision

2. **Map Rendering** - Uses Leaflet.js for interactive mapping
   - OpenStreetMap tiles as base layer
   - Dynamic grid overlay that adapts to zoom level
   - Grid labels sized appropriately for visibility

3. **Geolocation** - Real-time GPS tracking
   - Updates user position marker
   - Displays current grid square
   - Handles location errors gracefully

### Important Implementation Details

- The Maidenhead grid system uses:
  - Field: 18x18 grid (A-R) covering 20° lon x 10° lat
  - Square: 10x10 grid (0-9) covering 2° lon x 1° lat  
  - Subsquare: 24x24 grid (a-x) covering 5' lon x 2.5' lat
  - Extended: 10x10 grid (0-9) covering 30" lon x 15" lat

- Grid rendering optimizes performance by:
  - Only showing appropriate precision based on zoom
  - Limiting number of grid squares rendered
  - Reusing layer groups for efficiency

## Development Commands

### Running Locally
```bash
# This is a static site - simply open index.html in a browser
open index.html

# Or use any static file server
python3 -m http.server 8000
# Then navigate to http://localhost:8000
```

### Docker Development
```bash
# Build the Docker image
docker build -t gridmap .

# Run locally
docker run -p 8080:8080 gridmap
```

### Deployment
```bash
# Deploy to Fly.io (requires fly CLI)
fly deploy
```

## Code Style

- All JavaScript is currently embedded in index.html
- Uses ES6+ features (const, let, arrow functions, classes)
- No build process or transpilation
- No linting or formatting tools configured

## Testing

Currently no automated tests. When testing manually:
- Verify grid calculations with known coordinates
- Test geolocation in different browsers
- Check grid rendering at various zoom levels
- Ensure grid wrapping works at antimeridian (±180°)

## Common Tasks

### Modifying Grid Calculations
The GridSquare class in index.html handles all grid math. Key methods:
- `encode(precision)` - Convert lat/lon to grid square
- `decode(grid)` - Convert grid square to boundaries
- `getBounds()` - Get geographic boundaries for rendering

### Updating Map Behavior
Map interaction logic starts around line 210 in index.html:
- `updateGrid()` - Renders grid squares based on map view
- `map.on('moveend')` - Triggers grid updates
- Zoom-based precision logic in `getGridPrecision()`

### Debugging Tips
- Use browser console to test GridSquare calculations directly
- Check network tab for tile loading issues
- Geolocation errors appear in the grid display div
# D&D Map Hosting Website - Design Document

## Architecture Overview

### Technology Stack
- **Framework**: Next.js 14+ (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS v3 (custom dark/parchment theme)
- **Data Storage**: 
  - **MVP**: JSON files (local filesystem)
  - **Future**: DynamoDB (abstracted via repository pattern)
- **Authentication**: Simple password-based admin access

### Color Theme Configuration
```typescript
// Tailwind custom colors
colors: {
  parchment: {
    50: '#faf8f3',
    100: '#f5f0e6',
    200: '#e8dcc4',
    300: '#d4c19a',
    400: '#c4a876',
  },
  fantasy: {
    dark: '#1a1410',
    brown: '#3d2817',
    red: '#8b3a3a',
    gold: '#d4a574',
  }
}
```

### Application Structure

```
public/
  dnd-maps/
    app/
      (routes and pages)
    components/
      MapViewer.tsx       # Main map display with pin interaction
      PinMarker.tsx       # Individual pin component
      PinInfoBox.tsx      # Popup information box
      WorldList.tsx       # Home page world listing
      MapNavigation.tsx   # Bottom navigation for related maps
      SideNav.tsx         # Side navigation
      AdminPanel.tsx      # Admin controls for creating/editing
    data/
      worlds.json         # World definitions
      maps.json           # Map metadata
      pins.json           # Pin data
    public/
      uploads/            # Uploaded map images
```

## Data Models

### World
```typescript
{
  id: string
  name: string
  description: string
  mapIds: string[]  // Related maps (time periods)
}
```

### Map
```typescript
{
  id: string
  worldId: string
  name: string
  imageUrl: string
  timePeriod?: string
  width: number
  height: number
}
```

### Pin
```typescript
{
  id: string
  mapId: string
  x: number  // percentage position
  y: number  // percentage position
  color: 'red' | 'blue' | 'green' | 'yellow' | 'parchment' | 'black'
  title: string
  boxText: string      // Short preview
  articleText: string  // Full content
}
```

## Data Abstraction Layer

To support future DynamoDB migration, implement repository pattern:

```typescript
// lib/repositories/interface.ts
interface IRepository<T> {
  getAll(): Promise<T[]>
  getById(id: string): Promise<T | null>
  create(item: T): Promise<T>
  update(id: string, item: Partial<T>): Promise<T>
  delete(id: string): Promise<void>
}

// lib/repositories/json/ - MVP implementation
// lib/repositories/dynamodb/ - Future implementation
```

Switch between implementations via environment variable.

## Key Features Implementation

### 1. Map Viewer Component
- Canvas-based or image with absolute positioned pins
- Responsive scaling
- Dark/parchment themed interface

### 2. Pin Interaction (Guest Mode)
- Click/hover pins to show info box
- Info box styled with parchment background
- "Read More" button opens modal with full article
- Color-coded pins for visual categorization

### 3. Pin Creation (Admin Mode)
- Fixed "Add Pin" button on right side of screen
- Click button to activate pin placement mode
- Color picker modal appears with 6 color options (visual swatches)
- Click map to place pin at that location
- Form modal opens for title, box text, and article content
- Cancel option to exit placement mode

### 3. Navigation System
- **Home**: Grid of world cards with parchment-style cards
- **Side Nav**: Dark background, world selector + current world's maps
- **Bottom Nav**: Related maps carousel (time periods), semi-transparent overlay

### 4. Admin Mode
- Password protection (simple auth)
- Fixed "Add Pin" button (right side)
- Pin placement workflow:
  1. Click "Add Pin" button
  2. Select color from picker
  3. Click map location
  4. Fill in pin details form
- Forms for creating worlds, uploading maps
- Edit existing pins by clicking them

### 5. Data Persistence
- JSON files stored in `data/` directory
- File system operations via Next.js API routes
- Image uploads stored in `public/uploads/`

## Page Routes

- `/` - Home page (world list)
- `/world/[worldId]` - World view with first map
- `/world/[worldId]/map/[mapId]` - Specific map view
- `/admin/login` - Admin authentication
- `/api/worlds` - CRUD operations for worlds
- `/api/maps` - CRUD operations for maps
- `/api/pins` - CRUD operations for pins
- `/api/upload` - Image upload handler

## Security Considerations
- Admin password stored as environment variable
- API routes check authentication before mutations
- File upload validation (image types only)
- Input sanitization for user-generated content

## Deployment
- Build output to `/home/ec2-user/dev-server/public/dnd-maps/`
- Static export where possible
- API routes for data operations

## Development Phases

### Phase 1: Core Structure
- Next.js setup
- Data models and JSON storage
- Basic routing

### Phase 2: Map Viewing
- Map viewer component
- Pin display
- Info box interaction

### Phase 3: Navigation
- Home page world list
- Side and bottom navigation
- Map switching

### Phase 4: Admin Features
- Authentication
- World/map creation
- Pin creation and editing
- Image upload

### Phase 5: Polish
- Fantasy-themed styling (dark + parchment colors)
- Responsive design
- Error handling
- Smooth transitions and animations

### Phase 6: Future Enhancement
- DynamoDB repository implementation
- Environment-based data source switching
- Migration script from JSON to DynamoDB

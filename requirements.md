# D&D Map Hosting Website - Requirements

## Project Overview
A web application for hosting interactive D&D maps with location/event pins, supporting multiple worlds and time periods.

## Core Features

### 1. Interactive Map Display
- Display uploaded map images as the main canvas
- Support for placing colored pins on maps
- Pin interaction: hover/click to show information box
- "Read more" option to view full article for each pin

### 2. Pin System
- Multiple pin colors for categorization
- Each pin contains:
  - Short information box (preview text)
  - Full article content (detailed description)
  - Position coordinates on the map

### 3. User Roles

#### Guest (Read-Only)
- View maps and pins
- Read pin information boxes
- Read full articles
- Navigate between worlds and maps

#### Admin
- All guest capabilities, plus:
- Create new "worlds"
- Upload map images
- Create and place pins on maps
- Write/edit pin box text and full articles
- Manage related maps (time periods)

### 4. Navigation Structure

#### Home Page
- List of all "worlds"
- Access to any world's maps

#### Side Navigation
- Access to distantly related maps
- World selection

#### Bottom Navigation
- Navigate between related maps (e.g., different time periods of same world)

### 5. Admin Workflow
1. Create a "world"
2. Upload a map image
3. Add pins to the map
4. Write box text (short preview)
5. Write full article content

## Technical Requirements
- Deploy to `/home/ec2-user/dev-server/public/` folder
- TypeScript-based Next.js application
- Tailwind CSS for styling
- Minimal dependencies
- Authentication for admin access
- Data persistence:
  - **MVP**: Local JSON files
  - **Future**: DynamoDB migration support (abstracted data layer)

## Design & Theme
- **Color Palette**: Dark backgrounds with parchment/beige tones, red/brown accents
- **Aesthetic**: Fantasy + old-time feel with modern UI patterns
- **Inspiration**: Medieval maps, aged paper, warm earthy tones
- **Pin Colors**: Red, Blue, Green, Yellow, Parchment, Black

## Pin Creation UX
- Static "Add Pin" button fixed on right side of screen (admin mode)
- Click button to enter pin placement mode
- Color picker appears to select pin color (6 options)
- Click map location to place pin
- Form appears to add title, box text, and article

## User Experience Goals
- Intuitive pin placement and interaction
- Clean, immersive map viewing experience
- Easy navigation between related and unrelated maps
- Clear distinction between admin and guest modes

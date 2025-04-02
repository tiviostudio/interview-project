# Space Data Visualization Application

## Overview
This application transforms and visualizes astronomical data from NASA's Astronomy Picture of the Day (APOD) API. The app processes raw space data through a transformation pipeline and presents it in an interactive dashboard with various filtering and visualization capabilities.

## Features

### Data Transformation Pipeline
- Fetches the last 50 Astronomy Pictures of the Day from NASA's public API (api_key: y7jxOwYJkbKT1cZU4YdzpnCCmZ4p6rf0sphYqgEj)
- Groups images by celestial categories based on keyword detection in the description and title
- Categorizes into: Galaxies, Vehicles/Rockets, Planets and moons, Stars, Comets and asteroids, and Other
- Calculates metrics for each category (count, frequency, average title length)
- Extracts dominant color palettes from each image
- Generates a "complexity score" for each astronomical object based on:
    - Text length (longer descriptions typically indicate more complex objects)
    - Keyword diversity (number of unique astronomical terms in the description)

### Interactive Dashboard
- Grid view of transformed image data with previews and key metrics
- Data visualization charts showing distribution across categories
- Responsive design for both desktop and mobile viewing
- Detail view with enhanced information on click

### Control Panel
- Category filter buttons for selecting specific types of celestial objects
- Real-time date and time display that updates every second
- Toggle for enabling/disabling the automatic time-based filtering feature

## Time-Based Filtering System

The application includes a dynamic time-based filter that automatically changes which celestial objects are displayed based on the current time. This feature works as follows:

### How the Time-Based Filter Works:
1. **10-Second Intervals**: The application divides each minute into six 10-second intervals.
2. **Category Rotation**: Each 10-second interval corresponds to a different category of celestial objects:
   - 0-9 seconds: Galaxies
   - 10-19 seconds: Vehicles/Rockers
   - 20-29 seconds: Planets
   - 30-39 seconds: Stars
   - 40-49 seconds: Moon/Earth views
   - 50-59 seconds: Other space phenomena

3. **Visual Indicator**: The current active category is highlighted in the control panel.
4. **Manual Override**: Users can disable automatic filtering by toggling the "Auto Filter" switch in the control panel.
5. **Persistent Selection**: When a user clicks on an object to view details, the automatic filtering is temporarily paused to allow exploration.

### Implementation Details:
- When the current second crosses into a new 10-second boundary, the filter category is updated
- The filter is applied using a custom selector function that efficiently filters the data based on the current active category

### Example:
If the current time is 3:45:23, the filter will automatically show only Planets (since we're in the 20-29 second interval). When the clock reaches 3:45:30, the display will transition to show only Stars.

## Getting Started

### Prerequisites
- Node.js (version 20 or higher)
- npm or yarn or pnpm

### Installation
2. Navigate to the project directory
   ```
   cd interview-projecct
   ```

3. Install dependencies
   ```
   npm install
   # or
   yarn install
   # or 
   pnpm install
   ```

4. Start the development server
   ```
   npm run dev
   # or
   yarn dev
   # or
   pnpm dev
   ```

5. Open [http://localhost:3000](http://localhost:3000) in your browser

## Technical Implementation

### Categorization Algorithm
This is an example of categorization (you can come up with your own solution, this is just an example which can be used)

```typescript
function categorizeAstronomyImage(item: APODItem): Category {
  const text = (item.title + " " + item.explanation).toLowerCase();
  
  // Category definitions with their keywords
  const categories = [
    { 
      name: "galaxies", 
      keywords: ["galaxy", "galaxies", "milky way", "andromeda", "galactic", "intergalactic"]
    },
    { 
      name: "nebulae", 
      keywords: ["nebula", "nebulae", "emission", "supernova remnant", "dust cloud", "molecular cloud"] 
    },
    { 
      name: "planets", 
      keywords: ["planet", "mercury", "venus", "earth", "mars", "jupiter", "saturn", "uranus", 
                "neptune", "pluto", "moon", "lunar"] 
    },
    { 
      name: "stars", 
      keywords: ["star", "stellar", "sun", "solar", "supernova", "nova", "constellation", "cluster"] 
    },
    { 
      name: "comets", 
      keywords: ["comet", "asteroid", "meteor", "meteorite", "shooting star"] 
    },
    { 
      name: "spaceflight", 
      keywords: ["spacecraft", "astronaut", "mission", "rover", "lander", "launch", "iss", "telescope"] 
    }
  ];
  
  // Find matches based on priority
  for (const category of categories) {
    if (category.keywords.some(keyword => text.includes(keyword))) {
      return category.name;
    }
  }
  
  // Default category
  return "other";
}
```

### Requirements

* The task should be completed in 3 hours. If you do not finish in time, you can still create a pull request and let us know what you couldn't finish in time.
* There is a lot of work but it's not possible to complete everything. Please prioritize the tasks by your choice and deliver the MVP
* The code should be clean and readable.
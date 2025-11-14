# GACKcon Overlay

A professional GDQ-style streaming overlay for OBS that displays real-time convention schedule information, donation progress, and panel details. Perfect for live-streaming multi-day conventions and events.

## Features

- **Bottom Bar (70px height, left side)** - Shows currently running panel and next upcoming panel with countdown timer
- **Schedule Sidebar (270px width, right side)** - Full-height visual timeline of today's schedule (11 AM - 2 AM)
- **Donation Progress** - Live donation tracking with progress bar and goal display
- **Color-Coded Floors** - Three distinct floor colors (Floor 1: Pink, Floor 2: Green, Floor 3: Blue)
- **Smart Text Sizing** - Panel names automatically shrink to fit when needed
- **Auto-Updates** - Schedule and donation data refresh automatically (30-60 second intervals)
- **Clean Design** - Transparent overlay designed to sit over video content without obstruction
- **Multi-Day Support** - Handles events spanning multiple days with proper date/time detection

## Setup in OBS

1. Add a new **Browser Source** in OBS
2. Set the URL to the local file path: `file:///C:/Users/droha/Workspace/GACKcon-Overlay/overlay.html`
3. Set dimensions to **1920x1080**
4. Check "Shutdown source when not visible" (optional, saves resources)
5. Check "Refresh browser when scene becomes active" (optional)

The overlay takes up only 80px at the bottom of the screen with a clean, professional look.

## Configuration

### Donation API

Edit `overlay.html` and update the `CONFIG` object:

```javascript
const CONFIG = {
    donationApiUrl: 'https://your-api-endpoint.com/donations',
    donationGoal: 5000,  // Change this to your goal amount
    refreshInterval: 30000, // How often to check donations (30 seconds)
    updateScheduleInterval: 60000 // How often to update schedule (1 minute)
};
```

Your donation API should return JSON like:
```json
{
  "total": 1250
}
```

### Schedule

Edit `schedule.json` to update the schedule and current panel:

```json
{
  "schedule": [
    {
      "starttime": "2025-11-14 11:00 AM",
      "duration": 30,
      "title": "Doors Open & Registration",
      "host": "Staff",
      "floor": 1
    },
    {
      "starttime": "2025-11-14 12:00 PM",
      "duration": 60,
      "title": "Opening Ceremony",
      "host": "John Doe",
      "floor": 1
    },
    {
      "starttime": "2025-11-14 5:00 PM",
      "duration": 60,
      "title": "Dinner Break / Intermission",
      "host": "Everyone",
      "floor": 1
    }
  ]
}
```

**Field descriptions:**
- `starttime`: Panel start time in format `YYYY-MM-DD HH:MM AM/PM`
  - Example: `"2025-11-14 11:00 AM"` for November 14, 2025 at 11:00 AM
- `duration`: Panel length in minutes (e.g., 30, 60, 90)
- `title`: Panel name
- `host`: Panel host name (use "TBA" if unknown)
- `floor`: Floor number (1, 2, or 3)

The overlay uses the duration to:
- Accurately detect which panel is currently running
- Show visual spacing in the schedule sidebar
- Support multi-day events spanning weeks or months

**Floor numbers:**
- `floor: 1` - Floor 1 (pink/rose color)
- `floor: 2` - Floor 2 (green color)
- `floor: 3` - Floor 3 (blue color)

**Host:** Each panel should have a `host` field for "Hosted by" display

The overlay will:
- Show only **today's schedule** (11 AM - 2 AM) in the right sidebar
- Automatically detect the current panel based on date/time
- Grey out past panels
- Highlight the current panel
- Show the next upcoming panel with time remaining in minutes
- Update every 30 seconds

**Note:** The schedule sidebar intelligently shows the current event day:
- If it's between 11 AM and midnight: shows 11 AM today through 2 AM tomorrow
- If it's between midnight and 2 AM: shows 11 AM yesterday through 2 AM today (current late-night session)
- If it's between 2 AM and 11 AM: shows 11 AM today through 2 AM tomorrow (upcoming event day)

## Customization

### Colors

Edit the CSS in `overlay.html` to change colors:
- Floor colors are defined at the top of the CSS:
  - `.floor-1` - Floor 1 (pink/rose: `#d88aa6`)
  - `.floor-2` - Floor 2 (green: `#7fba8b`)
  - `.floor-3` - Floor 3 (blue: `#7eb3d8`)
- Bottom bar background: `rgba(13, 22, 39, 0.95)`
- Donation progress: `#00ff88` to `#00d4aa` gradient
- Current panel label: `#00a8ff`
- Next up label: `#ffa500`

### Layout

The overlay consists of:
- **Bottom bar** (70px height) - spans full width with 3 equal sections:
  - Left: Current panel info
  - Center: Donation progress
  - Right: Next up panel with timer
- **Right sidebar** (320px width) - full day schedule

Adjust dimensions in the CSS by modifying `.bottom-bar` height and `.schedule-sidebar` width.

### Fonts & Sizes

All font sizes and styles are in the `<style>` section of `overlay.html`.

## Testing

Open `overlay.html` in a browser to preview before adding to OBS. The donation API will fail unless you configure a valid endpoint, but the schedule will load from `schedule.json`.

## Tips

- Use CORS-enabled API endpoints for donations
- Update `schedule.json` manually or via a script during your event
- The overlay refreshes automatically, no need to reload in OBS
- Transparent background works great over your video feed

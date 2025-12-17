# CaddieCall Design Guidelines

## Architecture Decisions

### Authentication
**No authentication required.** This is a session-based, utility app where users join temporary voice rooms using room codes. No user accounts, sign-in, or data persistence needed.

### Navigation
**Stack-only navigation** (Home → Room):
- Home Screen: Entry point with Create/Join options
- Room Screen: Modal-like full-screen experience (no back button while connected)

## Screen Specifications

### Home Screen
**Purpose:** Entry point for creating or joining a golf round voice chat.

**Layout:**
- Transparent header with app branding centered
- Top inset: `insets.top + Spacing.xl`
- Bottom inset: `insets.bottom + Spacing.xl`
- Scrollable root view (SafeAreaView)

**Content:**
- App logo/title at top third of screen
- Tagline: "Voice chat for your golf round"
- Primary action: "Create Round" button (prominent, center)
- Secondary section: Room code input (6-char max, auto-uppercase) + "Join Round" button
- Components: Large primary button, text input with button

### Room Screen
**Purpose:** Real-time voice chat interface with push-to-talk controls.

**Layout:**
- Custom header with room code (large, centered) and Leave button (top-right, text-only)
- Top inset: `headerHeight + Spacing.xl`
- Bottom inset: `insets.bottom + 120px` (space for large PTT button)
- Non-scrollable root view

**States:**
1. **Not Connected:** Nickname input + "Join Voice" button (centered)
2. **Connecting:** Loading spinner + "Connecting..." text
3. **Connected:** Participants list + voice controls

**Connected State Components:**
- Participants list (top section, scrollable if >4 people)
  - Each participant: Avatar circle + nickname + speaking indicator (animated ring)
  - Current user marked with "You" label
- Audio ducking indicator (small banner when active): "Spotify volume lowered"
- Bottom controls area (fixed):
  - Push-to-Talk button (large, center, 80x80pt minimum)
  - Open Mic toggle (left of PTT)
  - Mute Speaker toggle (right of PTT)

## Design System

### Color Palette
**Golf-inspired, minimal palette:**
- **Primary Green:** `#2D5A3D` (golf course green, for active/pressed states)
- **Light Green:** `#4A7C59` (speaking indicators, active toggles)
- **Neutral Background:** `#F8F9FA` (light mode), `#1C1C1E` (dark mode)
- **Card Background:** `#FFFFFF` (light), `#2C2C2E` (dark)
- **Text Primary:** `#1C1C1E` (light), `#F8F9FA` (dark)
- **Text Secondary:** `#6B7280`
- **Error Red:** `#DC2626` (connection errors)
- **Warning Yellow:** `#F59E0B` (ducking indicator)

### Typography
- **Headers:** SF Pro Display, 28pt Bold (room code), 20pt Semibold (screen titles)
- **Body:** SF Pro Text, 16pt Regular (nicknames, labels)
- **Buttons:** SF Pro Text, 17pt Semibold
- **Code:** SF Mono, 32pt Bold (room code display)

### Component Specifications

**Push-to-Talk Button (Critical):**
- Shape: Circle, 80x80pt
- Default: `Primary Green` background, white microphone icon
- Pressed: Scale to 0.95, `Light Green` background, add subtle outer glow
- Shadow (always visible):
  - shadowOffset: {width: 0, height: 4}
  - shadowOpacity: 0.20
  - shadowRadius: 8
- Haptic feedback on press/release
- Label below: "Push to Talk" or "Transmitting..." (when pressed)

**Toggle Buttons (Open Mic, Mute Speaker):**
- Shape: Rounded square, 56x56pt
- Default: `Card Background`, gray icon
- Active: `Primary Green` background, white icon
- Pressed: Scale to 0.95
- Icons: microphone (open mic), speaker (mute speaker)
- Labels below toggles

**Participant Cards:**
- Horizontal layout: Avatar (40x40pt circle) + Nickname (left-aligned)
- Speaking state: Animated green ring around avatar (2pt stroke, pulsing opacity 0.5→1.0)
- Background: `Card Background`, rounded 12pt
- Padding: 12pt vertical, 16pt horizontal

**Room Code Display:**
- SF Mono typeface, 32pt Bold, letter-spacing: 4pt
- Displayed as: "ABC DEF" (3-char groups with space)
- Background chip: light gray, rounded 8pt, padding 8pt horizontal

**Primary Button (Home Screen):**
- Height: 56pt, full-width (with 24pt horizontal margins)
- Background: `Primary Green`, white text
- Corner radius: 12pt
- Pressed: opacity 0.85

**Input Field (Nickname, Room Code):**
- Height: 52pt
- Background: `Card Background`
- Border: 1pt solid `#E5E7EB`, 2pt solid `Primary Green` when focused
- Corner radius: 10pt
- Placeholder color: `Text Secondary`

### Interaction Design

**Push-to-Talk Interaction:**
- Press: Immediate visual feedback (scale + color change) + haptic
- Hold: Show "Transmitting..." label + ducking indicator appears
- Release: Return to default state + ducking indicator disappears after 0.3s
- No delay between press and transmission start

**Open Mic Behavior:**
- Toggle on: PTT button changes to show continuous transmission state (pulsing subtle glow)
- PTT button disabled while open mic is active
- Label changes to "Open Mic Active"

**Connection States:**
- Use subtle skeleton loading for participant cards while connecting
- Reconnecting: Show yellow banner at top "Reconnecting..."
- Connection error: Show error banner with "Retry" button

### Accessibility Requirements
- VoiceOver labels for all interactive elements
- Push-to-Talk button: "Push to talk, double-tap and hold to transmit"
- Toggle buttons: "Open microphone, currently off" (state changes)
- Minimum touch target: 44x44pt for all interactive elements
- Dynamic Type support for all text (scale appropriately)
- High contrast mode support (increase border weights)

### Assets
**Required Generated Assets:**
- App icon: Golf flag/green silhouette on green gradient background
- Participant avatars: 4 preset golf-themed simple avatars (golf ball, golf club, flag, tee) - flat, circular, 2-color design
- Use system icons for: microphone, speaker, person.circle, xmark

**Asset Style:**
- Minimal, flat design
- No photo-realistic textures
- 2-3 colors maximum per asset
- SVG-based for scalability
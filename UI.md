 Baik, saya akan membuat **UI/UX Plan dan Navigation Structure** yang komprehensif untuk sistem PTW ini. Plan ini akan mencakup:

1. **Information Architecture** - Struktur navigasi lengkap
2. **User Flow** - Perjalanan pengguna per role
3. **Wireframe Layout** - Struktur halaman utama
4. **Design System** - Komponen UI dan styling guidelines
5. **Interaction Patterns** - Cara pengguna berinteraksi dengan sistem
6. **Responsive Strategy** - Adaptasi untuk mobile/tablet di lapangan

---

# 🎨 UI/UX PLAN & NAVIGATION SYSTEM
## PTW (Permit to Work) System

---

## 1. NAVIGATION ARCHITECTURE

### 1.1 Global Navigation Structure

```
┌─────────────────────────────────────────────────────────────┐
│                    GLOBAL NAVIGATION                        │
├─────────────────────────────────────────────────────────────┤
│  [Logo]    Dashboard    Permits    Approvals    Monitoring  │
│                                                      [User]   │
└─────────────────────────────────────────────────────────────┘

SIDEBAR NAVIGATION (Role-Based)

┌─────────────────┐
│  📊 DASHBOARD   │  ← Default landing page
├─────────────────┤
│  📝 PERMITS     │
│   ├─ New Permit │
│   ├─ My Permits │
│   ├─ Drafts     │
│   └─ All Permits│  (Admin/HSE only)
├─────────────────┤
│  ✅ APPROVALS   │
│   ├─ Pending    │
│   ├─ Approved   │
│   └─ Rejected   │
├─────────────────┤
│  👁️ MONITORING  │
│   ├─ Active Now │
│   ├─ QR Verify  │
│   └─ Stop Work  │
├─────────────────┤
│  📈 REPORTS     │  (HSE/Admin/Authorizer)
│   ├─ Analytics  │
│   ├─ Audit Log  │
│   └─ Export     │
├─────────────────┤
│  ⚙️ MASTER DATA │  (Admin only)
│   ├─ Users      │
│   ├─ Locations  │
│   ├─ Checklists │
│   └─ Permit Types│
└─────────────────┘
```

### 1.2 Role-Based Navigation Access

| Menu | Requester | Supervisor | Area Owner | HSE | Authorizer | Admin |
|------|:---------:|:----------:|:----------:|:---:|:----------:|:-----:|
| **Dashboard** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **New Permit** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **My Permits** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Approvals → Pending** | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **Approvals → History** | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **Monitoring → Active** | RO | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Monitoring → QR Verify** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Reports** | ❌ | RO | RO | ✅ | ✅ | ✅ |
| **Master Data** | ❌ | ❌ | ❌ | RO | RO | ✅ |

> **Legend:** ✅ Full Access | RO Read Only | ❌ No Access

---

## 2. USER FLOW DIAGRAMS

### 2.1 Requester Flow (Create Permit)

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  START   │────▶│ Dashboard│────▶│New Permit│────▶│Select    │
│          │     │          │     │  Entry   │     │Permit    │
└──────────┘     └──────────┘     └──────────┘     │Type      │
                                                    └────┬─────┘
                                                         │
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌───┴──────┐
│  SUBMIT  │◀────│  REVIEW  │◀────│Fill Basic│◀────│  JSEA    │
│  Permit  │     │  & Send  │     │  Info    │     │  Form    │
└────┬─────┘     └──────────┘     └──────────┘     └──────────┘
     │
     ▼
┌──────────┐
│ STATUS:  │
│SUBMITTED │
└──────────┘
     │
     ▼
┌──────────┐     ┌──────────┐     ┌──────────┐
│ Track in │────▶│Approved? │────▶│ ACTIVE   │
│ MyPermits│     │          │     │(Get QR)  │
└──────────┘     └────┬─────┘     └──────────┘
                      │
                      ▼
               ┌──────────┐
               │ REJECTED │
               │(Revise &  │
               │ Resubmit) │
               └──────────┘
```

### 2.2 Approval Flow (Multi-Stage)

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│ SUBMITTED│────▶│Supervisor│────▶│Area Owner│────▶│   HSE    │
│          │     │  Review  │     │  Review  │     │  Review  │
└──────────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
                      │                  │                │
               ┌──────┴──────┐      ┌────┴────┐      ┌────┴────┐
               │  APPROVE    │      │ APPROVE │      │ APPROVE │
               │  → Forward  │      │→Forward │      │→Forward │
               └─────────────┘      └─────────┘      └─────────┘
                      │                  │                │
                      ▼                  ▼                ▼
               ┌─────────────────────────────────────────────────┐
               │              AUTHORIZER (Final)                 │
               │  ┌────────┐    ┌────────┐    ┌────────┐       │
               │  │APPROVE │ or │ REJECT │ or │SUSPEND │       │
               │  │→ACTIVE │    │→Back   │    │→STOP   │       │
               │  └────────┘    └────────┘    └────────┘       │
               └─────────────────────────────────────────────────┘
```

### 2.3 Field Execution Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  ACTIVE  │────▶│  Print/  │────▶│Display at│
│  Permit  │     │  Show QR │     │  Site    │
└────┬─────┘     └──────────┘     └──────────┘
     │
     ▼
┌─────────────────────────────────────────────────┐
│           MONITORING & EXECUTION                │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐       │
│  │Supervisor│  │  HSE    │  │ QR Scan │       │
│  │ Checks  │  │ Patrol  │  │ Verify  │       │
│  └────┬────┘  └────┬────┘  └────┬────┘       │
│       │            │            │              │
│       ▼            ▼            ▼              │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐       │
│  │  Safe?  │  │  Safe?  │  │ Valid?  │       │
│  └────┬────┘  └────┬────┘  └────┬────┘       │
│       │            │            │              │
│   ┌───┴───┐    ┌───┴───┐    ┌───┴───┐        │
│   │YES│NO │    │YES│NO │    │YES│NO │        │
│   │↓ │ ↓ │    │↓ │ ↓ │    │↓ │ ↓ │        │
│   │Cont│Stop│   │Cont│Stop│   │OK  │Alert│        │
│   └─────────┘  └─────────┘  └─────────┘       │
└─────────────────────────────────────────────────┘
     │
     ▼
┌──────────┐     ┌──────────┐     ┌──────────┐
│ Work Done│────▶│  Close   │────▶│  CLOSED  │
│          │     │  Permit  │     │          │
└──────────┘     └──────────┘     └──────────┘
```

---

## 3. SCREEN WIREFRAMES & LAYOUT

### 3.1 Universal Layout Structure

```
┌─────────────────────────────────────────────────────────────┐
│ HEADER (Fixed, 64px height)                                 │
├─────────────────────────────────────────────────────────────┤
│ [Logo PTW]     [Breadcrumb: Home > Permits > New]      [🔔] [👤 User ▼] │
└─────────────────────────────────────────────────────────────┘

┌──────────────────┬────────────────────────────────────────┐
│ SIDEBAR          │ MAIN CONTENT AREA                        │
│ (Fixed, 240px)   │ (Scrollable, max-width 1200px)         │
│                  │                                        │
│  Dashboard       │                                        │
│  ─────────────   │                                        │
│  Permits         │     [Content varies by page]           │
│    ├ New Permit  │                                        │
│    ├ My Permits  │                                        │
│    └ Drafts      │                                        │
│  ─────────────   │                                        │
│  Approvals       │                                        │
│    ├ Pending (3) │     ←─── Main Card Container ───→      │
│    └ History     │     ┌────────────────────────┐       │
│  ─────────────   │     │ Card / Table / Form      │       │
│  Monitoring      │     │                        │       │
│  ─────────────   │     │                        │       │
│  Reports         │     └────────────────────────┘       │
│                  │                                        │
│                  │                                        │
│  ─────────────   │                                        │
│  [🌙 Dark Mode]  │                                        │
│  [⚙️ Settings]   │                                        │
│                  │                                        │
└──────────────────┴────────────────────────────────────────┘
```

### 3.2 Dashboard Layout (Role-Specific)

#### Dashboard - Requester View

```
┌─────────────────────────────────────────────────────────────┐
│  DASHBOARD                          [Date: 04 Feb 2026]     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐│
│  │ NEW PERMIT      │  │ MY PERMITS      │  │ HELP        ││
│  │ [Big Button]    │  │ [Big Button]    │  │ [Big Button]││
│  │     ➕          │  │     📋          │  │     ❓      ││
│  └─────────────────┘  └─────────────────┘  └─────────────┘│
│                                                             │
│  ┌─────────────────────────────────────────────────────────┐
│  │ QUICK STATS                           [View All →]       │
│  ├─────────────────────────────────────────────────────────┤
│  │  [🟡 2] Draft        [🟠 1] Waiting    [🟢 3] Active   │
│  └─────────────────────────────────────────────────────────┘
│                                                             │
│  ┌─────────────────────────────────────────────────────────┐
│  │ RECENT ACTIVITY (Last 5)                                │
│  ├─────────────────────────────────────────────────────────┤
│  │ • Hot Work Permit #HW-2026-0042 → [SUPERVISOR_APPROVED] │
│  │ • Electrical Work #EL-2026-0018 → [ACTIVE]           │
│  │ • Maintenance #MN-2026-0105 → [HSE_APPROVED]            │
│  └─────────────────────────────────────────────────────────┘
└─────────────────────────────────────────────────────────────┘
```

#### Dashboard - Approver View (Supervisor/Area Owner/HSE)

```
┌─────────────────────────────────────────────────────────────┐
│  DASHBOARD - SUPERVISOR                                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────────┐
│  │ ⚠️ URGENT: PERMITS REQUIRING ATTENTION                 │
│  ├─────────────────────────────────────────────────────────┤
│  │ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐          │
│  │ │Pending │ │Overdue │ │HighRisk│ │HotWork │          │
│  │ │   5    │ │   2    │ │   3    │ │   1    │          │
│  │ └────────┘ └────────┘ └────────┘ └────────┘          │
│  └─────────────────────────────────────────────────────────┘
│                                                             │
│  ┌─────────────────────────────────────────────────────────┐
│  │ APPROVAL QUEUE                                          │
│  ├──────┬──────────┬────────────┬──────────┬────────────┤
│  │SELECT│PERMIT NO.│TYPE        │REQUESTER │TIME        │
│  ├──────┼──────────┼────────────┼──────────┼────────────┤
│  │  ⭕  │HW-2026-42│Hot Work    │Ahmad F.  │2 hours ago │
│  │  ⭕  │EL-2026-18│Electrical  │Budi S.   │5 hours ago │
│  └──────┴──────────┴────────────┴──────────┴────────────┘
│  [🔘 Select All]    [✅ Approve Selected] [❌ Reject]     │
└─────────────────────────────────────────────────────────────┘
```

### 3.3 Permit Creation Form (Wizard Layout)

```
┌─────────────────────────────────────────────────────────────┐
│  NEW PERMIT - HOT WORK                          [Step 2/4] │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────────┐
│  │ PROGRESS INDICATOR                                      │
│  │  1️⃣ ─── 2️⃣ ─── 3️⃣ ─── 4️⃣                             │
│  │ Basic   JSEA   Checklist  Review                         │
│  │ ✓Done   ●Current ○Pending ○Pending                     │
│  └─────────────────────────────────────────────────────────┘
│                                                             │
│  ┌─────────────────────────────────────────────────────────┐
│  │ STEP 2: JOB SAFETY & ENVIRONMENTAL ANALYSIS (JSEA)      │
│  ├─────────────────────────────────────────────────────────┤
│  │                                                          │
│  │  Job Description: [________________________________]    │
│  │                                                          │
│  │  JSEA Steps:                                             │
│  │  ┌────┬─────────────────┬─────────────────┬────────────┐ │
│  │  │ No │ Job Step        │ Hazards         │ Controls   │ │
│  │  ├────┼─────────────────┼─────────────────┼────────────┤ │
│  │  │ 1  │ Preparasi area  │ Api, Debu      │ APD, Vent  │ │
│  │  │ 2  │ [Add Step...]   │                 │            │ │
│  │  └────┴─────────────────┴─────────────────┴────────────┘ │
│  │  [➕ Add Row]                                           │
│  │                                                          │
│  │  Risk Matrix:                                            │
│  │  [🟢 Low] [🟡 Medium] [🟠 High] [🔴 Critical]            │
│  │                                                          │
│  │  [💾 Save Draft]    [← Back]    [Next Step →]           │
│  └─────────────────────────────────────────────────────────┘
└─────────────────────────────────────────────────────────────┘
```

### 3.4 Active Permit Monitoring (Real-time Dashboard)

```
┌─────────────────────────────────────────────────────────────┐
│  MONITORING - ACTIVE PERMITS              [Auto-refresh: ON]│
├─────────────────────────────────────────────────────────────┤
│  FILTERS: [All Types ▼] [All Areas ▼] [Risk: All ▼] [🔍]    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐           │
│  │ 🔴 STOP ALL│  │ 📊 Stats   │  │ 📍 Map View│           │
│  │   WORK     │  │            │  │            │           │
│  └────────────┘  └────────────┘  └────────────┘           │
│                                                             │
│  LIVE PERMIT CARDS:                                         │
│  ┌─────────────────────────────────────────────────────────┐
│  │ 🟢 ACTIVE    HW-2026-0042    ⏱️ 2h 15m remaining       │
│  ├─────────────────────────────────────────────────────────┤
│  │  Type: Hot Work        Location: Fabrication Shop #2     │
│  │  Requester: Ahmad F.   Supervisor: Budi S.             │
│  │                                                          │
│  │  Progress: [████████░░░] 85%                           │
│  │                                                          │
│  │  [👁️ View] [🛑 Stop Work] [✅ Close] [📱 QR Code]        │
│  └─────────────────────────────────────────────────────────┘
│                                                             │
│  ┌─────────────────────────────────────────────────────────┐
│  │ 🟢 ACTIVE    EL-2026-0018    ⏱️ 4h 30m remaining       │
│  ├─────────────────────────────────────────────────────────┤
│  │  Type: Electrical Work   Location: Panel Room A          │
│  │  ⚠️  LOTO Applied: Breaker #E-42, #E-43                 │
│  │                                                          │
│  │  [👁️ View] [🛑 Stop Work] [✅ Close] [📱 QR Code]        │
│  └─────────────────────────────────────────────────────────┘
└─────────────────────────────────────────────────────────────┘
```

### 3.5 Mobile View (QR Verification - Field Use)

```
┌──────────────┐
│  PTW Mobile  │
├──────────────┤
│                                                              │
│  ┌──────────┐                                               │
│  │   📷     │  ← Camera/QR Scanner                          │
│  │  SCAN QR │                                               │
│  └──────────┘                                               │
│                                                              │
│  OR                                                          │
│                                                              │
│  Enter Permit No:                                            │
│  ┌────────────────┐                                         │
│  │ HW-2026-0042   │                                         │
│  └────────────────┘                                         │
│  [🔍 Verify]                                                 │
│                                                              │
│  ┌─────────────────────────────────────────┐                  │
│  │ PERMIT STATUS                          │                  │
│  │                                        │                  │
│  │  🟢 VALID & ACTIVE                     │                  │
│  │                                        │                  │
│  │  Hot Work Permit                       │                  │
│  │  Location: Fab Shop #2                 │                  │
│  │  Valid until: 14:00 WIB                │                  │
│  │                                        │                  │
│  │  [View Details]                        │                  │
│  │  [🚨 Report Issue]                   │                  │
│  └─────────────────────────────────────────┘                  │
│                                                              │
│  [🔄 Scan Again]  [📋 History]                               │
└──────────────┘
```

---

## 4. DESIGN SYSTEM & COMPONENT LIBRARY

### 4.1 Color Palette (Safety-First Design)

```
PRIMARY COLORS
├── Blue-600    (#2563EB) - Primary Actions, Links
├── Blue-700    (#1D4ED8) - Hover States
└── Blue-800    (#1E40AF) - Active States

SEMANTIC COLORS (Status)
├── Success-500 (#10B981) - Active, Approved, Safe
├── Warning-500 (#F59E0B) - Pending, Draft, Medium Risk
├── Danger-500  (#EF4444) - Rejected, Stop Work, High Risk
├── Info-500    (#3B82F6) - Info, In Progress
└── Gray-500    (#6B7280) - Closed, Inactive, Neutral

BACKGROUND
├── White       (#FFFFFF) - Cards, Content Areas
├── Gray-50     (#F9FAFB) - Page Background
├── Gray-100    (#F3F4F6) - Sidebar, Dividers
└── Dark-900    (#111827) - Dark Mode Background

RISK LEVEL BADGES
├── 🟢 Green-100 text-Green-800   - Low Risk
├── 🟡 Yellow-100 text-Yellow-800 - Medium Risk
├── 🟠 Orange-100 text-Orange-800 - High Risk
└── 🔴 Red-100 text-Red-800       - Critical Risk
```

### 4.2 Typography Scale

```
FONT FAMILY: Inter (Sans-serif) for UI, Roboto Mono for permit numbers

SCALE:
├── H1: 32px/40px bold    - Page Titles
├── H2: 24px/32px semibold - Section Headers
├── H3: 18px/24px medium   - Card Titles
├── Body: 14px/20px regular - Content
├── Small: 12px/16px regular - Captions, timestamps
└── Mono: 14px/20px medium - Permit Numbers (e.g., HW-2026-0042)
```

### 4.3 Component Specifications

#### Status Badge Component

```html
<!-- Active Status -->
<span class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-green-100 text-green-800">
  <span class="w-2 h-2 mr-2 rounded-full bg-green-500 animate-pulse"></span>
  ACTIVE
</span>

<!-- Pending Status -->
<span class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-yellow-100 text-yellow-800">
  <svg class="w-4 h-4 mr-1 animate-spin" fill="none" viewBox="0 0 24 24">
    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
  </svg>
  PENDING APPROVAL
</span>
```

#### Permit Card Component

```html
<div class="bg-white rounded-lg shadow-md border-l-4 border-green-500 p-4 mb-4">
  <!-- Header -->
  <div class="flex justify-between items-start mb-3">
    <div>
      <span class="text-xs font-mono text-gray-500">HW-2026-0042</span>
      <h3 class="text-lg font-semibold text-gray-900">Hot Work - Tank Repair</h3>
    </div>
    [STATUS BADGE]
  </div>
  
  <!-- Body -->
  <div class="grid grid-cols-2 gap-2 text-sm mb-4">
    <div>
      <span class="text-gray-500">Location:</span>
      <span class="text-gray-900">Tank Farm Area B</span>
    </div>
    <div>
      <span class="text-gray-500">Requester:</span>
      <span class="text-gray-900">Ahmad Fauzi</span>
    </div>
  </div>
  
  <!-- Actions -->
  <div class="flex gap-2">
    <button class="flex-1 bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">
      View Details
    </button>
    <button class="px-4 py-2 border border-gray-300 rounded hover:bg-gray-50">
      QR
    </button>
  </div>
</div>
```

#### Action Button Hierarchy

| Button Type | Style | Usage |
|-------------|-------|-------|
| **Primary** | Blue bg, white text | Main action (Submit, Approve, Save) |
| **Secondary** | White bg, blue border, blue text | Alternative action (Back, Cancel) |
| **Danger** | Red bg, white text | Destructive (Reject, Stop Work, Delete) |
| **Success** | Green bg, white text | Completion (Close Permit, Mark Safe) |
| **Ghost** | Transparent, gray text | Tertiary actions (View, Edit) |
| **Icon** | Icon + label, compact | Quick actions (Print, Download, QR) |

---

## 5. INTERACTION PATTERNS & BEHAVIORS

### 5.1 Form Interactions

```
VALIDATION PATTERNS:
├── Real-time: Validate format (email, date) on blur
├── On Submit: Validate required fields, show inline errors
├── JSEA: Cannot submit if risk matrix not filled
└── Checklist: Cannot proceed if critical items unchecked

CONFIRMATION DIALOGS:
├── Submit Permit: "Are you sure? This will start approval process."
├── Stop Work: "⚠️ STOP WORK - This will immediately halt all activities!"
├── Approve: Quick action (no dialog) for normal flow
└── Reject: Modal with reason input required
```

### 5.2 Notification System

```
TOAST NOTIFICATIONS (Auto-dismiss 5s):
├── Success: "Permit HW-2026-0042 submitted successfully" [🟢]
├── Error: "Failed to save JSEA. Please check required fields" [🔴]
├── Warning: "Permit expires in 30 minutes" [🟡]
└── Info: "New approval request received" [🔵]

BADGE NOTIFICATIONS (Persistent):
├── Sidebar: Approvals [3] ← Red badge on menu
├── Header: Bell icon with dot [🔔]
└── In-app: Pulse animation on active permits
```

### 5.3 Real-time Updates (SignalR/WebSocket)

```
AUTO-REFRESH STRATEGY:
├── Dashboard: Every 60 seconds
├── Active Monitoring: Every 30 seconds
├── Approval Queue: Every 15 seconds
└── Critical Alerts: Instant push notification

VISUAL INDICATORS:
├── Updated row: Brief yellow highlight flash
├── New item: Slide in from top with "New" badge
└── Status change: Color transition animation
```

### 5.4 Accessibility Features

```
KEYBOARD NAVIGATION:
├── Tab order: Logical flow (Top → Bottom, Left → Right)
├── Shortcuts: 
│   ├── Alt+N = New Permit
│   ├── Alt+A = Approvals
│   ├── Alt+M = Monitoring
│   └── Esc = Close modal/cancel
├── Focus states: Blue ring (2px offset) on all interactive elements
└── Skip link: "Skip to main content" for screen readers

SCREEN READER SUPPORT:
├── ARIA labels on all icons and buttons
├── Status announcements via aria-live regions
├── Table headers properly associated
└── Error messages linked to inputs via aria-describedby
```

---

## 6. MOBILE RESPONSIVE STRATEGY

### 6.1 Breakpoints

```
BREAKPOINTS:
├── Mobile: < 640px (sm)
├── Tablet: 640px - 1024px (md/lg)
├── Desktop: > 1024px (xl)
└── Large: > 1280px (2xl)

STRATEGY:
├── Mobile-first design
├── Sidebar becomes bottom nav on mobile
├── Tables become cards on mobile
├── Forms become single-column on mobile
└── QR Scanner full-screen on mobile
```

### 6.2 Mobile Adaptations

| Desktop | Mobile (< 640px) |
|---------|------------------|
| Fixed sidebar | Bottom navigation bar (4 icons) |
| Multi-column forms | Single column, collapsible sections |
| Data tables | Vertical cards with expand/collapse |
| Hover tooltips | Long-press or (i) icons |
| Right-click context | Swipe actions or ••• menu |
| Modal dialogs | Full-screen sheets |
| Side-by-side comparison | Tab switcher |

### 6.3 Field Worker Mobile Interface

```
PRIORITY FEATURES FOR MOBILE:
├── Quick QR Scan (camera-first)
├── View Active Permit (read-only, offline capable)
├── Stop Work Authority (one-tap with confirmation)
├── Photo documentation (attach to permit)
└── Voice notes (for quick reporting)

OFFLINE MODE:
├── View cached permits
├── Queue actions for sync
├── Draft photos locally
└── Sync when connection restored
```

---

## 7. PAGE-LEVEL NAVIGATION MAP

### 7.1 Complete Sitemap

```
PTW SYSTEM
│
├── 🔐 AUTH (Public)
│   ├── /login
│   ├── /forgot-password
│   └── /reset-password
│
├── 📊 DASHBOARD (Authenticated)
│   ├── /dashboard (Role-based widgets)
│   └── /notifications
│
├── 📝 PERMITS MODULE
│   ├── /permits
│   │   ├── /new (Wizard: Type → Info → JSEA → Checklist → Review)
│   │   ├── /drafts
│   │   ├── /my-permits (Requester view)
│   │   └── /all (Admin/HSE view with filters)
│   ├── /permits/{id} (Detail view)
│   │   ├── /view (Read-only)
│   │   ├── /edit (Draft only)
│   │   ├── /jsea (Edit JSEA)
│   │   ├── /checklist (Complete checklist)
│   │   └── /history (Audit trail)
│   └── /permits/{id}/close (Closing form)
│
├── ✅ APPROVALS MODULE
│   ├── /approvals
│   │   ├── /pending (Queue with bulk actions)
│   │   ├── /approved (History)
│   │   └── /rejected (History)
│   └── /approvals/{id}/review (Detailed review page)
│       ├── Approve/Reject buttons
│       ├── View JSEA & Checklist
│       └── Add comments
│
├── 👁️ MONITORING MODULE
│   ├── /monitoring
│   │   ├── /active (Real-time dashboard)
│   │   ├── /qr-verify (Scanner interface)
│   │   └── /stopped (Incident management)
│   └── /monitoring/{id}/control (Permit control panel)
│       ├── Stop Work button
│       ├── Resume button
│       └── Emergency contacts
│
├── 📈 REPORTS MODULE
│   ├── /reports
│   │   ├── /analytics (Charts & trends)
│   │   ├── /audit-log (System logs)
│   │   └── /exports (PDF/Excel generation)
│   └── /reports/permit-summary/{date-range}
│
├── ⚙️ MASTER DATA (Admin)
│   ├── /master
│   │   ├── /users (CRUD + roles)
│   │   ├── /locations (Area tree)
│   │   ├── /permit-types (Config)
│   │   ├── /checklists (Template builder)
│   │   └── /hazards (Risk library)
│   └── /settings/system (Global config)
│
└── 👤 PROFILE
    ├── /profile
    ├── /change-password
    └── /preferences (Language, notifications)
```

### 7.2 URL Patterns & Parameters

```
PATTERN EXAMPLES:
├── /permits?status=active&type=hot_work&area=fabrication
├── /approvals/pending?sort=oldest&risk=high
├── /reports/analytics?from=2026-01-01&to=2026-02-04
└── /monitoring/active?view=map (Alternative to list view)
```

---

## 8. USER ONBOARDING FLOW

```
NEW USER JOURNEY:
├── Day 1: Welcome modal + Role explanation
├── Day 2: Interactive tour (Create first draft permit)
├── Day 3: Contextual tips (JSEA helper, Risk matrix guide)
├── Day 4: Advanced features (Bulk actions, Shortcuts)
└── Ongoing: Help tooltips (?) on complex fields

HELP SYSTEM:
├── Contextual: ? icon beside every form field
├── Video: Short Loom videos for complex workflows
├── Chat: In-app support widget (bottom right)
└── Docs: Link to full user manual
```

---

## 9. ERROR HANDLING & EMPTY STATES

### 9.1 Empty States

```
SCENARIO                          VISUAL TREATMENT
─────────────────────────────────────────────────────────────
No permits created yet        📋 + "Create your first permit" CTA
No approvals pending          ✅ + "All caught up!" + relax icon
No active permits             😴 + "No work in progress"
No search results             🔍 + "Try different keywords"
No access permission          🚫 + "Contact admin for access"
Offline mode                  📡 + "Working offline, will sync..."
```

### 9.2 Error Pages

```
404 Not Found:
├── Message: "Permit not found or you don't have access"
├── Action: [Back to Dashboard] [Search Permits]
└── Visual: Broken chain link icon

500 Server Error:
├── Message: "Something went wrong. Our team has been notified."
├── Action: [Retry] [Contact Support]
└── Visual: Warning triangle with gears

Session Timeout:
├── Auto-redirect to login with message
└── Preserve form data in localStorage for recovery
```

---

## 10. IMPLEMENTATION PRIORITIES

### Phase 1: MVP (Core Flow)
```
Priority A (Must Have):
├── Login & Role-based access
├── Create Permit (Basic info + JSEA)
├── Simple Approval flow (4 steps)
├── Active/Closed status
└── Basic Dashboard

Priority B (Should Have):
├── QR Code generation
├── Safety Checklists
├── Email notifications
└── Permit detail view
```

### Phase 2: Enhanced UX
```
├── Bulk approvals
├── Advanced filtering
├── Real-time monitoring dashboard
├── Mobile-responsive field interface
└── Photo attachments
```

### Phase 3: Advanced Features
```
├── Offline mobile mode
├── Advanced analytics
├── Integration APIs
├── Multi-language support
└── AI risk suggestions
```

---

## SUMMARY: KEY DESIGN PRINCIPLES

1. **Safety First** - Dangerous actions (Stop Work) require maximum friction, important actions (Approve) require minimum friction
2. **Clarity Over Beauty** - Information hierarchy prioritizes safety-critical data
3. **Contextual Navigation** - Show only what the user needs based on role and current task
4. **Progressive Disclosure** - Complex forms (JSEA) use wizard pattern to reduce cognitive load
5. **Mobile-First for Field** - Field workers need quick, thumb-friendly interfaces
6. **Feedback Loops** - Every action has clear visual/audio confirmation
7. **Error Prevention** - Validation happens early, destructive actions require confirmation

---

# SSE Payflow - Payment Management System

## 📋 Overview

SSE PayHub is a comprehensive payment management system built for Sanskrithi School of Engineering. It provides a modern, secure, and efficient platform for managing student payments, library fines, cultural event fees, industrial visit charges, and complaint fees.

## ✨ Features

### For Students
- **Secure Authentication**: Email-based login with password reset functionality
- **Payment Dashboard**: View all pending dues in one place
- **Payment History**: Track all completed transactions with downloadable receipts
- **Digital ID Card**: Access student ID card with QR code
- **Library Management**: View borrowed books and outstanding fines
- **Mobile App**: Native iOS and Android applications
- **Offline Support**: Works seamlessly even with poor connectivity
- **Push Notifications**: Get notified about payment reminders and updates

### For Administrators
- **Role-Based Access Control**: Different admin roles for different departments
  - Master Admin: Full system access
  - Library Admin: Library book and fine management
  - Cultural Admin: Cultural event management
  - Complaint Admin: Complaint and fee management
  - IV Admin: Industrial visit management
- **Student Management**: Add, edit, delete, and manage student records
- **Payment Processing**: Process payments via QR code scanning
- **Analytics Dashboard**: View payment statistics and reports
- **Bulk Operations**: Import/export student data via CSV
- **Audit Logs**: Complete tracking of all system activities
- **Fine Adjustments**: Manage and adjust library fines

## 🏗️ Technology Stack

### Frontend
- **React 18** - Modern UI library
- **TypeScript** - Type-safe development
- **Vite** - Fast build tool and dev server
- **React Router** - Client-side routing
- **TanStack Query** - Data fetching and caching
- **Tailwind CSS** - Utility-first CSS framework
- **Shadcn UI** - High-quality UI components

### Backend
- **Supabase** - Backend as a Service
  - PostgreSQL database
  - Row Level Security (RLS)
  - Real-time subscriptions
  - Authentication
  - Edge Functions
  - File Storage

### Mobile
- **Capacitor** - Native mobile runtime
- **Android & iOS** - Native app support
- **Biometric Authentication** - Fingerprint/Face ID
- **QR Scanner** - ML Kit barcode scanning
- **Push Notifications** - Native notifications

### Payment Integration
- **Razorpay** - Payment gateway for India
- **UPI, Cards, Net Banking** - Multiple payment methods

## 🚀 Quick Start

### Prerequisites
```bash
Node.js 18+ and npm
Git
```

### Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd sse-payhub
```

2. **Install dependencies**
```bash
npm install
```

3. **Set up environment variables**
```bash
cp .env.example .env
```

Edit `.env` and add your credentials:
```env
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_PUBLISHABLE_KEY=your_supabase_anon_key
VITE_RAZORPAY_KEY_ID=your_razorpay_key_id
```

4. **Run development server**
```bash
npm run dev
```

The application will be available at `http://localhost:5173`

## 📱 Mobile App Development

### Setup
```bash
# Build the web application
npm run build

# Add platforms
npx cap add android
npx cap add ios

# Sync changes
npx cap sync

# Open in IDE
npx cap open android
npx cap open ios
```

See [MOBILE_APP_GUIDE.md](./MOBILE_APP_GUIDE.md) for detailed instructions.

## 🗄️ Database Schema

### Core Tables
- **students** - Student information and credentials
- **admins** - Administrator accounts and roles
- **user_roles** - Role-based access control
- **student_assignments** - Payment assignments (fees, fines, charges)
- **payments** - Payment transactions and status
- **library_books** - Library book inventory
- **library_transactions** - Book borrowing records
- **library_fines** - Library fine records
- **events** - Cultural events and industrial visits
- **complaints** - Student complaints and fees
- **audit_logs** - System activity tracking
- **user_sessions** - Session management

## 🏛️ System Architecture

### High-Level Architecture Diagram

<lov-presentation-mermaid>
graph TB
    subgraph Client["Client Layer"]
        Web["Web Application<br/>(React + Vite)"]
        Android["Android App<br/>(Capacitor)"]
        iOS["iOS App<br/>(Capacitor)"]
    end
    
    subgraph Gateway["API Gateway & CDN"]
        CDN["Vercel/Netlify/CloudFront<br/>SSL/TLS Termination<br/>Static Asset Caching"]
    end
    
    subgraph Backend["Backend Services"]
        Supabase["Supabase Platform"]
        
        subgraph SupabaseServices["Supabase Services"]
            Auth["Authentication<br/>JWT Tokens"]
            DB["PostgreSQL Database<br/>Row Level Security"]
            EdgeFn["Edge Functions<br/>(Deno Runtime)"]
            Realtime["Real-time Engine<br/>WebSockets"]
            Storage["File Storage<br/>S3-compatible"]
        end
    end
    
    subgraph External["External Services"]
        Razorpay["Razorpay<br/>Payment Gateway"]
        Email["Email Service<br/>SMTP/Resend"]
        SMS["SMS Gateway"]
    end
    
    Web --> CDN
    Android --> CDN
    iOS --> CDN
    
    CDN --> Auth
    CDN --> DB
    CDN --> EdgeFn
    CDN --> Realtime
    CDN --> Storage
    
    EdgeFn --> Razorpay
    EdgeFn --> Email
    EdgeFn --> SMS
    EdgeFn --> DB
    
    Auth -.-> DB
    Realtime -.-> DB
    
    style Client fill:#e1f5ff
    style Backend fill:#fff4e1
    style External fill:#ffe1e1
    style Gateway fill:#e1ffe1
</lov-presentation-mermaid>

### Complete System Architecture Flow

<lov-presentation-mermaid>
graph LR
    subgraph Users["👥 Users"]
        Student["Student"]
        Admin["Administrator"]
    end
    
    subgraph Frontend["🖥️ Frontend Application"]
        UI["React UI<br/>Components"]
        State["State Management<br/>TanStack Query"]
        Router["React Router<br/>Navigation"]
    end
    
    subgraph Auth["🔐 Authentication"]
        Login["Login Flow"]
        Session["Session Management"]
        JWT["JWT Tokens"]
    end
    
    subgraph API["⚡ Edge Functions"]
        CreateOrder["create-razorpay-order"]
        VerifyPayment["verify-razorpay-payment"]
        Webhook["razorpay-webhook"]
        UpdateFines["update-library-fines"]
    end
    
    subgraph Database["🗄️ Database Layer"]
        Students["students"]
        Admins["admins"]
        Payments["payments"]
        Books["library_books"]
        Assignments["student_assignments"]
        Roles["user_roles"]
    end
    
    subgraph RLS["🛡️ Row Level Security"]
        Policies["RLS Policies"]
        Functions["Security Functions<br/>has_role(), is_admin()"]
    end
    
    Student --> UI
    Admin --> UI
    UI --> State
    State --> Router
    
    UI --> Login
    Login --> Session
    Session --> JWT
    
    State --> API
    API --> CreateOrder
    API --> VerifyPayment
    API --> Webhook
    API --> UpdateFines
    
    CreateOrder --> Payments
    VerifyPayment --> Payments
    UpdateFines --> Books
    
    JWT --> Policies
    Policies --> Functions
    Functions --> Students
    Functions --> Admins
    Functions --> Roles
    
    Policies --> Payments
    Policies --> Books
    Policies --> Assignments
    
    style Users fill:#b3d9ff
    style Frontend fill:#d9f2d9
    style Auth fill:#ffe6b3
    style API fill:#ffb3d9
    style Database fill:#e6b3ff
    style RLS fill:#ffcccc
</lov-presentation-mermaid>

### Frontend Architecture

#### Component Hierarchy
```
src/
├── components/
│   ├── admin/                 # Admin-specific components
│   │   ├── RoleBasedDashboard.tsx
│   │   ├── StudentManagement/
│   │   ├── PaymentProcessing/
│   │   └── LibraryManagement/
│   ├── student/              # Student-specific components
│   │   ├── Dashboard.tsx
│   │   ├── PaymentFlow/
│   │   └── LibraryView/
│   ├── common/               # Reusable components
│   │   ├── ErrorBoundary.tsx
│   │   ├── LoadingSkeleton.tsx
│   │   └── OptimizedImage.tsx
│   └── ui/                   # Shadcn UI components
│       └── [all UI primitives]
├── hooks/                    # Custom React hooks
│   ├── useAuth.tsx           # Authentication logic
│   ├── usePermissions.tsx    # Role-based access control
│   ├── useOfflineSync.tsx    # Offline data management
│   └── useOptimizedQuery.tsx # Query optimization
├── lib/                      # Utility functions
│   ├── supabaseOptimizations.ts
│   ├── performance.ts
│   └── validation.ts
└── pages/                    # Route components
    ├── StudentPortal.tsx
    ├── AdminPortal.tsx
    └── [other pages]
```

#### State Management Flow
```
User Action → Component
              ↓
         React Hook (useAuth, useStudents)
              ↓
         TanStack Query (caching, optimistic updates)
              ↓
         Supabase Client
              ↓
         Backend API
```

### Backend Architecture

#### Supabase Stack
```
┌────────────────────────────────────────────────────────┐
│                    Edge Functions                       │
├────────────────────────────────────────────────────────┤
│ • create-razorpay-order    • update-library-fines     │
│ • verify-razorpay-payment  • send-notifications       │
│ • razorpay-webhook         • generate-reports         │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│              PostgreSQL Database + RLS                  │
├────────────────────────────────────────────────────────┤
│ Tables:                                                 │
│ • students         • payments         • library_books  │
│ • admins           • user_roles       • events         │
│ • student_assignments                 • complaints     │
│                                                         │
│ Security:                                               │
│ • Row Level Security (RLS) policies per table          │
│ • Service role for admin operations                    │
│ • Anon role for public operations                      │
└────────────────────────────────────────────────────────┘
```

#### Database Schema Design

<lov-presentation-mermaid>
erDiagram
    students ||--o{ student_assignments : "has"
    students ||--o{ library_books : "borrows"
    students ||--o{ library_fines : "owes"
    students ||--o{ library_transactions : "has"
    students ||--o{ borrowing_history : "has"
    students ||--o{ payments : "makes"
    students ||--o{ event_registrations : "registers"
    students ||--o{ complaints : "files"
    students ||--o{ user_sessions : "has"
    
    admins ||--o{ user_roles : "has"
    admins ||--o{ events : "creates"
    
    student_assignments ||--o{ payments : "paid_via"
    
    library_books ||--o{ library_transactions : "in"
    library_transactions ||--o{ library_fines : "generates"
    
    events ||--o{ event_registrations : "has"
    
    students {
        uuid id PK
        text name
        text roll_no UK
        text email UK
        text password_hash
        text department
        text batch
        text section
        date dob
        text student_phone
        text parent_phone
        timestamp created_at
        timestamp updated_at
    }
    
    admins {
        uuid id PK
        text name
        text email UK
        text password_hash
        date dob
        timestamp created_at
        timestamp updated_at
    }
    
    user_roles {
        uuid id PK
        uuid user_id FK
        app_role role
        timestamp created_at
    }
    
    student_assignments {
        uuid id PK
        uuid student_id FK
        text title
        text description
        numeric amount
        assignment_type type
        assignment_status status
        date due_date
        boolean paid
        timestamp created_at
        timestamp updated_at
    }
    
    payments {
        uuid id PK
        uuid student_id FK
        uuid assignment_id FK
        numeric amount
        text razorpay_order_id
        text razorpay_payment_id
        text razorpay_signature
        payment_status status
        timestamp created_at
        timestamp updated_at
    }
    
    library_books {
        uuid id PK
        text title
        text author
        text isbn
        text category
        text department
        integer copies_total
        integer copies_available
        text status
        uuid assigned_to FK
        date issued_date
        date due_date
        date return_date
        numeric fine_amount
        boolean lost_stolen
        timestamp created_at
        timestamp updated_at
    }
    
    library_transactions {
        uuid id PK
        uuid student_id FK
        uuid book_id FK
        date issue_date
        date due_date
        date return_date
        text status
        timestamp created_at
        timestamp updated_at
    }
    
    library_fines {
        uuid id PK
        uuid student_id FK
        uuid transaction_id FK
        numeric fine_amount
        boolean paid
        timestamp created_at
        timestamp updated_at
    }
    
    events {
        uuid id PK
        text title
        text description
        assignment_type event_type
        date event_date
        text location
        numeric fee
        integer max_participants
        uuid created_by FK
        timestamp created_at
        timestamp updated_at
    }
    
    event_registrations {
        uuid id PK
        uuid student_id FK
        uuid event_id FK
        boolean paid
        timestamp registered_at
    }
    
    complaints {
        uuid id PK
        uuid student_id FK
        text title
        text description
        text category
        complaint_status status
        text resolution_notes
        timestamp created_at
        timestamp updated_at
    }
</lov-presentation-mermaid>

### Security Architecture

#### Authentication Flow

<lov-presentation-mermaid>
sequenceDiagram
    participant U as User
    participant C as Client App
    participant DB as Database
    participant Auth as Auth Service
    participant Session as Session Manager
    
    U->>C: Enter credentials
    C->>DB: Call authenticate_student()<br/>or authenticate_admin()
    
    DB->>DB: Query user table
    alt User found
        DB->>DB: Verify password hash<br/>(bcrypt compare)
        alt Password correct
            DB->>Auth: Generate JWT token
            Auth-->>DB: Return token
            DB->>Session: Create session record
            Session->>Session: Set expiry time
            DB-->>C: Return user data + token
            C->>C: Store token securely<br/>(localStorage/cookie)
            C-->>U: Login successful<br/>Redirect to dashboard
        else Password incorrect
            DB-->>C: Authentication failed
            C-->>U: Invalid credentials
        end
    else User not found
        DB-->>C: User not found
        C-->>U: Invalid credentials
    end
</lov-presentation-mermaid>

#### Authorization with RLS

<lov-presentation-mermaid>
flowchart TD
    Request[API Request with JWT] --> Extract[Extract user_id<br/>from auth.uid]
    Extract --> CheckAuth{User<br/>Authenticated?}
    
    CheckAuth -->|No| Deny[Deny Access<br/>401 Unauthorized]
    CheckAuth -->|Yes| CheckRole[Query user_roles<br/>Table]
    
    CheckRole --> GetRole{Get User Role}
    
    GetRole --> MasterAdmin[Master Admin]
    GetRole --> LibraryAdmin[Library Admin]
    GetRole --> CulturalAdmin[Cultural Admin]
    GetRole --> ComplaintAdmin[Complaint Admin]
    GetRole --> IVAdmin[IV Admin]
    GetRole --> UserMgmtAdmin[User Mgmt Admin]
    
    MasterAdmin --> ApplyRLS1[Apply RLS Policy:<br/>Full Access]
    LibraryAdmin --> ApplyRLS2[Apply RLS Policy:<br/>Library Tables Only]
    CulturalAdmin --> ApplyRLS3[Apply RLS Policy:<br/>Events Tables Only]
    ComplaintAdmin --> ApplyRLS4[Apply RLS Policy:<br/>Complaints Only]
    IVAdmin --> ApplyRLS5[Apply RLS Policy:<br/>IV Tables Only]
    UserMgmtAdmin --> ApplyRLS6[Apply RLS Policy:<br/>Student CRUD Only]
    
    ApplyRLS1 --> Filter[Filter Data Based<br/>on RLS Policy]
    ApplyRLS2 --> Filter
    ApplyRLS3 --> Filter
    ApplyRLS4 --> Filter
    ApplyRLS5 --> Filter
    ApplyRLS6 --> Filter
    
    Filter --> Return[Return Filtered<br/>Data to Client]
    
    style CheckAuth fill:#FFD700
    style GetRole fill:#87CEEB
    style Filter fill:#DDA0DD
    style Deny fill:#FFB6C1
</lov-presentation-mermaid>

#### Role-Based Access Control
```
┌──────────────────┐
│   Master Admin   │ → Full system access
└──────────────────┘
         ↓
┌──────────────────┐
│  Library Admin   │ → Library operations only
├──────────────────┤
│ Cultural Admin   │ → Event management only
├──────────────────┤
│ Complaint Admin  │ → Complaint handling only
├──────────────────┤
│    IV Admin      │ → Industrial visits only
├──────────────────┤
│User Mgmt Admin   │ → Student CRUD only
└──────────────────┘
```

### Payment Processing Architecture

<lov-presentation-mermaid>
sequenceDiagram
    participant S as Student
    participant UI as React UI
    participant EF1 as Edge Function<br/>create-order
    participant DB as Database
    participant RZP as Razorpay Gateway
    participant EF2 as Edge Function<br/>verify-payment
    participant EF3 as Edge Function<br/>webhook
    
    S->>UI: Select pending dues
    UI->>UI: Calculate total amount
    S->>UI: Click "Pay Now"
    
    UI->>EF1: Create order request
    EF1->>DB: Store order details
    EF1->>RZP: Create Razorpay order
    RZP-->>EF1: Return order_id
    EF1-->>UI: Return order details
    
    UI->>RZP: Open payment gateway
    S->>RZP: Enter payment details
    RZP->>RZP: Process payment
    RZP-->>UI: Payment response
    
    UI->>EF2: Verify payment signature
    EF2->>EF2: Validate signature
    EF2->>DB: Update payment status
    EF2->>DB: Mark assignments as paid
    EF2-->>UI: Payment confirmed
    
    RZP->>EF3: Webhook notification
    EF3->>EF3: Verify webhook signature
    EF3->>DB: Update payment status
    EF3->>DB: Log transaction
    EF3-->>RZP: 200 OK
    
    UI->>S: Show receipt
    UI->>S: Send email notification
</lov-presentation-mermaid>

### Mobile App Architecture

<lov-presentation-mermaid>
graph TB
    subgraph MobileApp["📱 Mobile Application"]
        subgraph WebLayer["Web Application Layer"]
            ReactApp["React Application<br/>(Same codebase as web)<br/>TypeScript + Vite"]
        end
        
        subgraph CapacitorBridge["Capacitor Bridge Layer"]
            Camera["@capacitor/camera<br/>QR Scanning"]
            Biometric["@aparajita/biometric-auth<br/>Fingerprint/Face ID"]
            Push["@capacitor/push-notifications<br/>FCM/APNs"]
            Network["@capacitor/network<br/>Connectivity Status"]
            Storage["IndexedDB/LocalStorage<br/>Offline Data"]
            Geolocation["@capacitor/geolocation<br/>Location Services"]
            App["@capacitor/app<br/>Lifecycle Events"]
        end
        
        subgraph NativeLayer["Native Platform Layer"]
            AndroidAPIs["Android APIs<br/>• CameraX<br/>• BiometricPrompt<br/>• WorkManager<br/>• Room Database"]
            iOSAPIs["iOS APIs<br/>• AVFoundation<br/>• LocalAuthentication<br/>• UserNotifications<br/>• Core Data"]
        end
    end
    
    ReactApp <--> Camera
    ReactApp <--> Biometric
    ReactApp <--> Push
    ReactApp <--> Network
    ReactApp <--> Storage
    ReactApp <--> Geolocation
    ReactApp <--> App
    
    Camera --> AndroidAPIs
    Camera --> iOSAPIs
    
    Biometric --> AndroidAPIs
    Biometric --> iOSAPIs
    
    Push --> AndroidAPIs
    Push --> iOSAPIs
    
    Network --> AndroidAPIs
    Network --> iOSAPIs
    
    Storage --> AndroidAPIs
    Storage --> iOSAPIs
    
    Geolocation --> AndroidAPIs
    Geolocation --> iOSAPIs
    
    App --> AndroidAPIs
    App --> iOSAPIs
    
    style WebLayer fill:#e1f5ff
    style CapacitorBridge fill:#fff4e1
    style NativeLayer fill:#ffe1f0
</lov-presentation-mermaid>

### Deploym
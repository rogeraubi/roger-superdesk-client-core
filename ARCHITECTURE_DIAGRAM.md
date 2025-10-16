# Superdesk Client Core - Architecture Diagram

## System Overview

**Superdesk** is a news content management system (CMS) client application built with **AngularJS 1.6**, **React**, and **TypeScript**.

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SUPERDESK CLIENT APPLICATION                         │
│                              (Version 2.5.3)                                 │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
                    ▼                 ▼                 ▼
        ┌─────────────────┐  ┌──────────────┐  ┌──────────────┐
        │  Angular 1.6    │  │    React     │  │  TypeScript  │
        │   Framework     │  │  Components  │  │   + Lodash   │
        └─────────────────┘  └──────────────┘  └──────────────┘
```

---

## Application Layers

```
┌───────────────────────────────────────────────────────────────────────────┐
│                           PRESENTATION LAYER                               │
├───────────────────────────────────────────────────────────────────────────┤
│  Angular Templates │ React Components │ Directives │ Controllers          │
│  - Views           │ - UI Components  │ - UI Logic │ - Route Control     │
│  - Modals          │ - Extension UI   │            │                     │
└───────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                          APPLICATION LAYER                                 │
├───────────────────────────────────────────────────────────────────────────┤
│  Feature Modules (apps/*)                                                 │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │
│  │  Authoring   │ │  Monitoring  │ │   Archive    │ │   Workspace  │   │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │
│  │    Ingest    │ │   Publishing │ │    Search    │ │     Users    │   │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │
│  │    Desks     │ │  Templates   │ │   Contacts   │ │   Settings   │   │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │
└───────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                            CORE SERVICES LAYER                             │
├───────────────────────────────────────────────────────────────────────────┤
│  Core Modules (core/*)                                                    │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │
│  │     API      │ │     Auth     │ │  Notification│ │   Activity   │   │
│  │   Service    │ │   Service    │ │   Service    │ │   Service    │   │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │
│  │   Editor3    │ │   Upload     │ │   Datetime   │ │    Menu      │   │
│  │   Service    │ │   Service    │ │   Service    │ │   Service    │   │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐   │
│  │  Privileges  │ │  Spellcheck  │ │  Elastic     │ │   Filters    │   │
│  │   Service    │ │   Service    │ │  Service     │ │   Service    │   │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘   │
└───────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                        DATA & COMMUNICATION LAYER                          │
├───────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────────┐  ┌──────────────────────┐                      │
│  │    HTTP/REST API     │  │   WebSocket Proxy    │                      │
│  │  - CRUD Operations   │  │  - Real-time Events  │                      │
│  │  - Resource Queries  │  │  - Live Updates      │                      │
│  │  - Authentication    │  │  - Notifications     │                      │
│  └──────────────────────┘  └──────────────────────┘                      │
└───────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                          BACKEND SERVICES                                  │
├───────────────────────────────────────────────────────────────────────────┤
│  Superdesk REST API Server  │  WebSocket Server  │  ElasticSearch        │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## Core Module Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           CORE MODULES                                   │
└─────────────────────────────────────────────────────────────────────────┘

┌──────────────────────┐
│   core/api           │  API Service & HTTP Communication
│ ─────────────────────│
│ • api-service.ts     │  Main API provider
│ • url-resolver.ts    │  Backend URL resolution
│ • allowed.ts         │  Permissions check
│ • request.ts         │  HTTP request wrapper
│ • timeout.ts         │  Request timeout handling
└──────────────────────┘

┌──────────────────────┐
│   core/auth          │  Authentication & Authorization
│ ─────────────────────│
│ • auth-service.ts    │  Login/logout logic
│ • session.ts         │  Session management
│ • basic-auth.ts      │  Basic authentication
│ • keycloak.ts        │  OAuth/OIDC support
│ • interceptor.ts     │  Auth token injection
└──────────────────────┘

┌──────────────────────┐
│  core/notification   │  Real-time Communication
│ ─────────────────────│
│ • notification.ts    │  WebSocket proxy
│ • reload-service.ts  │  Live reload handler
│ • notify.ts          │  User notifications
└──────────────────────┘

┌──────────────────────┐
│  core/activity       │  Routing & Activities
│ ─────────────────────│
│ • activity.ts        │  Activity registration
│ • routes.ts          │  Route management
└──────────────────────┘

┌──────────────────────┐
│  core/editor3        │  Rich Text Editor
│ ─────────────────────│
│ • Draft.js based     │  React editor component
│ • Plugins support    │  Extensible editor
└──────────────────────┘

┌──────────────────────┐
│  core/ui             │  UI Components
│ ─────────────────────│
│ • Forms              │  Form components
│ • Lists              │  List views
│ • Modals             │  Dialog system
└──────────────────────┘
```

---

## Application Modules (Feature Layer)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        APPLICATION MODULES                               │
└─────────────────────────────────────────────────────────────────────────┘

┌──────────────────────┐   ┌──────────────────────┐   ┌──────────────────────┐
│   apps/authoring     │   │  apps/monitoring     │   │   apps/archive       │
│ ─────────────────────│   │ ─────────────────────│   │ ─────────────────────│
│ Content editing      │   │ Desk monitoring      │   │ Article archive      │
│ • Article editor     │   │ • Stage views        │   │ • Published items    │
│ • Metadata           │   │ • List/Swimlane      │   │ • Item history       │
│ • Versioning         │   │ • Live updates       │   │ • Search/filter      │
│ • Autosave           │   │ • Item actions       │   │ • Bulk operations    │
│ • Locking            │   │ • Drag & drop        │   │ • Export             │
│ • Validation         │   │ • Filtering          │   └──────────────────────┘
└──────────────────────┘   └──────────────────────┘

┌──────────────────────┐   ┌──────────────────────┐   ┌──────────────────────┐
│    apps/ingest       │   │   apps/publish       │   │   apps/search        │
│ ─────────────────────│   │ ─────────────────────│   │ ─────────────────────│
│ Content ingestion    │   │ Publishing workflow  │   │ Content search       │
│ • Feed providers     │   │ • Publish actions    │   │ • Advanced filters   │
│ • Routing rules      │   │ • Destinations       │   │ • Saved searches     │
│ • Auto-import        │   │ • Corrections        │   │ • Full-text search   │
│ • Provider auth      │   │ • Scheduled pub      │   │ • ElasticSearch      │
└──────────────────────┘   └──────────────────────┘   └──────────────────────┘

┌──────────────────────┐   ┌──────────────────────┐   ┌──────────────────────┐
│    apps/desks        │   │   apps/workspace     │   │    apps/users        │
│ ─────────────────────│   │ ─────────────────────│   │ ─────────────────────│
│ Desk management      │   │ Workspace views      │   │ User management      │
│ • Desk creation      │   │ • Personal space     │   │ • Roles/permissions  │
│ • Stages             │   │ • Custom workspaces  │   │ • User profiles      │
│ • Desk members       │   │ • Dashboard          │   │ • Activity tracking  │
│ • Templates          │   │ • Navigation         │   │ • Import/export      │
└──────────────────────┘   └──────────────────────┘   └──────────────────────┘

┌──────────────────────┐   ┌──────────────────────┐   ┌──────────────────────┐
│   apps/templates     │   │  apps/vocabularies   │   │   apps/contacts      │
│ ─────────────────────│   │ ─────────────────────│   │ ─────────────────────│
│ Content templates    │   │ Controlled vocab     │   │ Contact management   │
│ • Template creation  │   │ • Categories         │   │ • Organizations      │
│ • Template types     │   │ • Subject codes      │   │ • Persons            │
│ • Scheduled          │   │ • Custom fields      │   │ • Contact cards      │
└──────────────────────┘   └──────────────────────┘   └──────────────────────┘

┌──────────────────────┐   ┌──────────────────────┐
│   apps/settings      │   │  apps/content-api    │
│ ─────────────────────│   │ ─────────────────────│
│ System settings      │   │ Public content API   │
│ • Configuration      │   │ • API management     │
│ • Preferences        │   │ • Subscribers        │
└──────────────────────┘   └──────────────────────┘
```

---

## Data Flow Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                            DATA FLOW                                     │
└─────────────────────────────────────────────────────────────────────────┘

    USER INTERACTION
           │
           ▼
    ┌─────────────┐
    │  Angular    │
    │ Controller  │◄────────┐
    │   / React   │         │
    │  Component  │         │
    └─────────────┘         │
           │                │
           ▼                │
    ┌─────────────┐         │
    │   Service   │         │
    │   Layer     │         │
    └─────────────┘         │
           │                │
           ▼                │
    ┌─────────────┐         │
    │  API Layer  │         │
    │  (HTTP/WS)  │         │
    └─────────────┘         │
           │                │
           ▼                │
    ┌─────────────┐         │
    │  Backend    │         │
    │   Server    │         │
    └─────────────┘         │
           │                │
           ▼                │
    ┌─────────────┐         │
    │  Database/  │         │
    │ ElasticSearch│        │
    └─────────────┘         │
           │                │
           └────────────────┘
         (Updates via
         WebSocket)


REAL-TIME UPDATES (WebSocket)
────────────────────────────

    Backend Event
         │
         ▼
    WebSocket Server
         │
         ▼
    WebSocket Proxy
         │
         ▼
    $rootScope.$broadcast
         │
         ▼
    Component Updates
         │
         ▼
    UI Refresh
```

---

## Authentication Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      AUTHENTICATION FLOW                                 │
└─────────────────────────────────────────────────────────────────────────┘

1. LOGIN REQUEST
   ┌──────────┐
   │  Login   │
   │   Form   │
   └────┬─────┘
        │
        ▼
   ┌──────────────┐
   │ auth.login() │
   └──────┬───────┘
          │
          ▼
   ┌────────────────┐
   │  authAdapter   │
   │ .authenticate()│
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │  POST /auth_db │
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │ Receive Token  │
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │ session.start()│
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │  Set Headers   │
   │ Authorization: │
   │     Token      │
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │  Redirect to   │
   │   Workspace    │
   └────────────────┘


2. AUTHENTICATED REQUESTS
   ┌──────────────┐
   │  HTTP Request│
   └──────┬───────┘
          │
          ▼
   ┌────────────────┐
   │  Interceptor   │
   │  adds Token    │
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │  Backend API   │
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │  401 Response? │
   └──────┬─────────┘
          │
          ├─ YES ──►  Expire Session
          │            Redirect to Login
          │
          └─  NO ──►  Return Response


3. SESSION EXPIRY
   ┌──────────────┐
   │  401 Error   │
   └──────┬───────┘
          │
          ▼
   ┌────────────────┐
   │ session.expire()│
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │  Clear Token   │
   └──────┬─────────┘
          │
          ▼
   ┌────────────────┐
   │  Redirect to   │
   │  Login Page    │
   └────────────────┘
```

---

## WebSocket Communication

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    WEBSOCKET COMMUNICATION                               │
└─────────────────────────────────────────────────────────────────────────┘

    Backend Server
         │
         ▼
    WebSocket Server
    (ws://server/ws)
         │
         ▼
    ┌─────────────────┐
    │ WebSocketProxy  │  (core/notification)
    └────────┬────────┘
             │
             ├──► Connection Management
             │    • Auto-reconnect
             │    • Heartbeat
             │    • Status tracking
             │
             ├──► Event Types:
             │    ┌──────────────────────┐
             │    │ • content:update     │
             │    │ • resource:created   │
             │    │ • resource:updated   │
             │    │ • resource:deleted   │
             │    │ • item:lock          │
             │    │ • item:unlock        │
             │    │ • item:spike         │
             │    │ • item:unspike       │
             │    │ • item:highlights    │
             │    │ • desk:mention       │
             │    │ • item:duplicate     │
             │    └──────────────────────┘
             │
             ▼
    $rootScope.$broadcast
             │
             ▼
    ┌────────────────────────┐
    │ Event Listeners        │
    │ (Various Controllers/  │
    │  Services/Components)  │
    └────────────────────────┘
             │
             ▼
    ┌────────────────────────┐
    │ UI Updates             │
    │ • Refresh lists        │
    │ • Show notifications   │
    │ • Update item status   │
    │ • Lock indicators      │
    └────────────────────────┘
```

---

## Extension System

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         EXTENSION SYSTEM                                 │
└─────────────────────────────────────────────────────────────────────────┘

    ┌────────────────────────┐
    │  Extension Interface   │
    │   (superdesk-api)      │
    └──────────┬─────────────┘
               │
               ▼
    ┌────────────────────────┐
    │  Extension Loader      │
    │ registerExtensions()   │
    └──────────┬─────────────┘
               │
               ├──► Custom Widgets
               │    • Authoring widgets
               │    • Dashboard widgets
               │
               ├──► Custom UI Components
               │    • Configurable components
               │    • React components
               │
               ├──► Custom Actions
               │    • Article actions
               │    • Bulk actions
               │
               ├──► Custom Fields
               │    • Field types
               │    • Validators
               │
               └──► API Extensions
                    • Endpoints
                    • Data transformers
```

---

## Build & Bundle System

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      BUILD SYSTEM (Webpack)                              │
└─────────────────────────────────────────────────────────────────────────┘

    superdesk.config.js
         │
         ▼
    ┌─────────────────┐
    │ webpack.config  │
    └────────┬────────┘
             │
             ├──► Entry Point: scripts/index.ts
             │
             ├──► Module Resolution:
             │    • TypeScript (.ts, .tsx)
             │    • JavaScript (.js, .jsx)
             │    • SCSS/SASS
             │    • Templates
             │
             ├──► Loaders:
             │    • ts-loader (TypeScript)
             │    • babel-loader (JS)
             │    • sass-loader (Styles)
             │    • ExtractTextPlugin (CSS)
             │
             ├──► Plugins:
             │    • ProvidePlugin (jQuery, moment, etc.)
             │    • DefinePlugin (__SUPERDESK_CONFIG__)
             │    • ExtractTextPlugin
             │
             └──► Output:
                  • dist/app.bundle.js
                  • dist/app.bundle.css
                  • dist/[chunks].bundle.js


    Grunt Tasks:
    ┌──────────────────────┐
    │ • ngtemplates        │  Generate template cache
    │ • build              │  Production build
    │ • server             │  Dev server
    │ • watch              │  File watching
    │ • test               │  Run tests
    └──────────────────────┘
```

---

## Key Technologies & Dependencies

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      TECHNOLOGY STACK                                    │
└─────────────────────────────────────────────────────────────────────────┘

FRONTEND FRAMEWORKS
├─ AngularJS 1.6.9         Main application framework
├─ React 16.8.23           Modern UI components
├─ TypeScript              Type-safe development
└─ Lodash                  Utility functions

UI LIBRARIES
├─ Superdesk UI Framework  Custom UI components
├─ PrimeReact             React component library
├─ Draft.js               Rich text editor (Editor3)
├─ Medium Editor          Alternative editor
└─ jQuery/jQuery UI       Legacy UI support

DATA & STATE
├─ Redux                   State management (React)
├─ Angular Resources       REST resource abstraction
└─ RxJS                    Reactive programming

COMMUNICATION
├─ Angular $http          HTTP client
├─ WebSocket              Real-time communication
└─ Fetch API              Modern HTTP requests

TESTING
├─ Karma                   Test runner
├─ Jasmine                 Test framework
├─ Protractor              E2E testing
└─ Enzyme                  React component testing

BUILD TOOLS
├─ Webpack                 Module bundler
├─ Grunt                   Task runner
├─ Babel                   JavaScript transpiler
└─ SASS                    CSS preprocessor

UTILITIES
├─ Moment.js              Date/time handling
├─ Angular-gettext        i18n/l10n
├─ Gridster               Dashboard layouts
└─ Owl Carousel           Carousels
```

---

## Configuration System

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        CONFIGURATION                                     │
└─────────────────────────────────────────────────────────────────────────┘

superdesk.config.js
    │
    ├──► Server Configuration
    │    • server.url           REST API endpoint
    │    • server.ws            WebSocket endpoint
    │
    ├──► Features
    │    • features.editor3     Enable Editor3
    │    • features.swimlane    Swimlane view
    │    • features.savedSearch Saved searches
    │    • features.preview     Print preview
    │
    ├──► Services
    │    • iframely.key         Embed service
    │    • google.key           Google services
    │    • raven.dsn            Error tracking
    │    • analytics.ga         Google Analytics
    │
    ├──► UI Preferences
    │    • view.dateformat      Date format
    │    • view.timeformat      Time format
    │    • ui.italicAbstract    Styling options
    │
    ├──► Editor Settings
    │    • editor.toolbar       Toolbar config
    │    • editor.embeds        Embed support
    │
    ├──► List Configuration
    │    • list.priority        Priority display
    │    • list.firstLine       Main line items
    │    • list.secondLine      Secondary items
    │
    └──► Workspace
         • workspace.content     Content view
         • workspace.ingest      Ingest view
         • defaultRoute          Default route
```

---

## File Structure

```
superdesk-client-core/
│
├── scripts/                    Source code
│   ├── index.ts               Entry point
│   ├── vendor.ts              Vendor imports
│   ├── appConfig.ts           App configuration
│   │
│   ├── core/                  Core services & utilities
│   │   ├── api/               API communication
│   │   ├── auth/              Authentication
│   │   ├── editor3/           Rich text editor
│   │   ├── notification/      WebSocket & notifications
│   │   ├── ui/                UI components
│   │   ├── services/          Shared services
│   │   └── ...
│   │
│   ├── apps/                  Feature modules
│   │   ├── authoring/         Content editing
│   │   ├── monitoring/        Desk monitoring
│   │   ├── archive/           Archive management
│   │   ├── ingest/            Content ingestion
│   │   ├── publish/           Publishing workflow
│   │   ├── search/            Search functionality
│   │   ├── desks/             Desk management
│   │   ├── workspace/         Workspace views
│   │   ├── users/             User management
│   │   └── ...
│   │
│   └── extensions/            Extension system
│
├── styles/                    Stylesheets
│   └── sass/                  SASS files
│
├── templates/                 Angular templates
│
├── e2e/                       End-to-end tests
│   ├── client/                Client E2E tests
│   └── server/                Test server
│
├── build-tools/               Build utilities
│
├── tasks/                     Grunt tasks
│
├── po/                        Translations
│
├── dist/                      Build output
│
├── webpack.config.js          Webpack configuration
├── Gruntfile.js              Grunt configuration
├── superdesk.config.js       App configuration
├── package.json              Dependencies
└── tsconfig.json             TypeScript config
```

---

## Summary

**Superdesk Client Core** is a sophisticated news content management system with:

- **Hybrid Architecture**: AngularJS 1.6 + React + TypeScript
- **Modular Design**: Core services + Feature modules + Extensions
- **Real-time Communication**: WebSocket for live updates
- **Rich Editing**: Multiple editor options (Editor3 with Draft.js)
- **Extensible**: Plugin/extension system for customization
- **Multi-language**: i18n support with angular-gettext
- **Enterprise Features**: Role-based access, workflows, publishing, archiving

**Key Patterns**:
- Service-oriented architecture
- Dependency injection (Angular)
- Event-driven communication (WebSocket + $rootScope)
- RESTful API communication
- Component-based UI (React + Angular directives)
- Configuration-driven features

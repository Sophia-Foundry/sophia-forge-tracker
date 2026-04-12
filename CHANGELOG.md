# Changelog

## 0.8.3 — 2026-04-12

### Added
- String tables plugin fully migrated — entries, translations, bulk linking, key generation, filtering, and pagination
- Data tables plugin fully migrated — column management, mapping workflows, and entity linking
- All plugins now migrated to electron — centralized registration file removed as it's no longer needed
- New e2e tests for plugin registration, plugin manifests, and core backend functionality

### Changed
- Category templates plugin refactored to complete partial migration
- Electron upgraded from v35 to v41
- Expanded and fixed e2e test coverage across all plugins

### Removed
- Legacy command registry and field status hooks
- Standalone string table entry picker component

## 0.8.2 — 2026-04-11

### Added
- **UE5 Sync Layer:** Complete rewrite of the engine sync backend, replacing 4 broken sync commands with correct UE5 plugin communication across 7 phases:
  - **Room Export Pipeline:** Level creation, wall/floor generation with door hole subtraction, door/utility/mesh/blueprint spawning, material queries, and export status — with coordinate mapping, tag resolution, dimension fallbacks, and real-time progress events
  - **String Table Sync:** Bidirectional push/pull with source text resolution from linked entities, conflict detection, namespace tracking, and bridge record management
  - **Data Table Sync:** Push/pull with cell resolution (FText references, asset paths, structs), column type mapping, pre-push summary, column sync preview, and UE5 data table scanning
  - **Blueprint Sync:** Pull property schema from UE5, push dirty changes with property diff computation, variable management, pending removals, and category curation workflow
  - **Asset Registration:** UE5 "Send to Sophia Forge" now creates/updates asset records with recursive dependency processing, thumbnail storage, parent material linking, and string table auto-linking
  - **Asset Push Pipeline:** Push files from SF to UE5 with prepare/execute/cancel lifecycle, batch push support, import presets, overwrite confirmation, and progress tracking
  - **Level Actor Sync:** Query UE5 levels, compute actor diffs with tag matching and transform tolerances, track added/removed/moved actors, register actors as SF assets
  - **UE5 Content Browser Browsing:** Folder tree listing, asset listing with per-type metadata, and thumbnail fetching
  - **Utility Schema Storage:** Schemas received from the UE5 plugin on connect are stored and used to enrich the property editor
- **Hidden Assets:** Toggle visibility on UE5 assets with hide/unhide actions in list page row menus and a "Show hidden" filter toggle — hidden assets display at reduced opacity with an eye icon
- **Push-to-UE5 Section:** Injected on all exportable entity detail pages (textures, meshes, illustrations, audio, characters) — shows connection status, push button, and linked UE5 asset management
- **Level Utility Import from Level:** Query utility actors from the currently open UE5 level and import them with full property snapshots
- **Inline Struct Property Editor:** Editable struct fields for level utility properties — supports numeric, boolean, and nested struct fields instead of showing "Struct (0 fields)"
- **Level Utility Schema Enrichment:** Property editor shows enum dropdowns, display names, tooltips, and override flags from schemas without requiring re-import of the utility
- **UE5 Config Re-push:** Settings changes on the Engine Config page are automatically pushed to the connected UE5 plugin

### Fixed
- **UE5 sync commands:** Room export, string table, data table, and blueprint sync now actually communicate with the UE5 plugin (previously sent commands that didn't exist)
- **Asset registration from UE5:** "Send to Sophia Forge" in UE5's content browser now works — assets appear in SF with thumbnails, metadata, and dependency links
- **Real-time sync progress:** Export and sync progress events now appear correctly in the frontend log
- **Room export status:** No longer returns 404 when checking export status for a room
- **Asset list pages:** Engine path column now shows actual paths instead of dashes, dates show correctly instead of "Invalid Date"
- **Asset detail pages:** Clicking an asset in the list now opens the detail page instead of returning "not found"
- **Blueprint detail page:** Categories, properties, and variables load correctly instead of being intercepted by the wrong route
- **Translation keys:** Fixed missing or broken translations across the Engine Config page, import dialogs, blueprint detail, and asset push section
- **Level utility creation:** Create button now enabled when UE5 is connected (was always disabled). Actor class names resolve correctly when spawning utilities
- **Asset icon crashes:** Unknown asset types no longer crash the thumbnail component
- **Engine messages:** Commands from the UE5 plugin with no registered handler now show warnings instead of failing silently

### Changed
- **UE5 asset and level utility detail pages:** Redesigned to match the standard detail page layout — flat sections with separators, no back buttons, tab-based navigation, large thumbnail display, tab title shows entity name
- **UE5 category assignment editor:** Flat section layout with standard header pattern replacing the card wrapper
- **Push-to-UE5 section styling:** Flat section layout with push button next to link button on the UE5 Assets header row
- **UE5 list pages:** Left-aligned table headers, thumbnail column on blueprints page, custom row action menus with hide/unhide and delete
- **Level utilities list page:** Import from Level button added, create button respects actual schema availability
- **Tooltip component:** Self-contained provider per tooltip — removed redundant wrappers
- **Room preview canvas:** Responds to sibling layout changes instead of only window resize
- **Room export defaults:** "Cut door openings into walls" checked by default, all spawn operations ensure the correct level is open
- **Internal refactoring:** i18n namespace corrections, route deduplication, asset type format standardization, engine handler cleanup, dead code removal

### Removed
- **Legacy sync service:** Replaced by dedicated per-feature sync services
- **Redundant tooltip provider wrappers:** No longer needed after tooltip component change

## 0.8.1 — 2026-04-09

### Added
- Inline entity creation from kanban boards — Dashboard Board and Progress Kanban columns now have "Add Item" buttons that let you pick an entity type, fill in a name and parent, and create it directly into the target status column
- `inlineCreatable` registration flag on 7 entity types (act, beat, cutscene, game system, level, room, OST track)
- Backend snapshots module for synchronous constellation and chronicle generation from SQLite cache
- Project-tracking plugin backend — entity-status functionality extracted from core into a self-registering plugin with its own routes, config, PluginSection registrations, and EventSystem integration
- Frontend project-tracking API layer and React Query hooks (`useProgress`, `useEntityStatusList`, `useEntityStatusReorder`)
- Story canvas alignment and distribution tools — snap and space selected acts horizontally or vertically
- Entity untracking with configurable confirmation dialog (`confirmUntrack` config option)
- Child entity hydration (`hydrateChildEntityType`) scans parent directories for nested entities and indexes them in the SQLite cache
- Room SQLite cache indexing on create/update/delete for immediate entity resolution
- Entity resolver now returns description field, used by ItemCardModal
- Parent-aware entity creation in GenericEntityLinker (Room→Level, Beat→Act, Act→auto-position on canvas)
- Category template config toggles (`enableItemCategories`, `enableSystemCategories`)
- Cutscene list type filter
- Extensive i18n additions across all 11 locales — entity untrack labels, add-item dialogs, story alignment tools, Claude Code toasts, MCP status, UE5 blueprint sync, and many previously hardcoded English strings

### Fixed
- DragOverlay rendering in all kanban views — portaled to `document.body` to fix visual clipping in scrollable containers
- BugCardModal data loss on close — debounced field saves are now flushed before the modal closes
- Room entity resolution — newly created rooms are immediately resolvable without full rehydration
- Entity status API routing consistency — all calls now use project-scoped paths

### Changed
- Board service rewrite — items derived from bugs + entity statuses instead of `board-order.json`; reorder uses `(boardItemId, targetStatus, newIndex)`
- Dashboard layout — Briefing tab removed (3 tabs: Board, Constellation, Chronicle); CSS Grid header with centered larger tabs
- Chronicle entry redesign — single-row layout with inline colored badges for entity type and action
- Constellation canvas entity type keys normalized to kebab-case with 8 new entity type colors
- PluginSections `after-status` moved below description/content fields across 15+ detail pages
- Level settings drawer and room properties panel visual refresh (larger labels, standardized spacing)
- ItemCardModal — description reads/saves actual entity description; `projectLabels` prop made optional
- ProgressDashboard refactored to use `useProgress` React Query hook
- ProgressFilters simplified (status chips removed, implicit via kanban columns)
- Board and ProgressKanban subscribe to entity change events with debounced live refresh

### Removed
- `core/entity-status/` module (4 files, relocated to `plugins/project-tracking/`)
- Core tools entity status entries (`set_entity_status`, `get_entity_status`, `get_progress`) — moved to plugin
- Hardcoded project-tracking registrations from `plugin-registrations.ts` and `plugin-registry.module.ts`
- Dashboard Briefing tab

## 0.8.0 — 2026-04-09

### Added
- Game system constitution rules and project DNA settings
- UI polish across plugins

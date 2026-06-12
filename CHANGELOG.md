# Changelog

## 0.8.10 — 2026-06-12

### Added
- **Toolbox Mode.** Creating a new project now opens with a choice between **Toolbox mode** — a stripped-down setup that hides the studio name, logo, Project DNA, and Game Details — and the full **Project Workstation**. You can switch modes at any time from the project's General Info settings.
- **Updates to the state machine editor.** States can now be marked Implemented, Planned, or Drift, each shown with its own icon on the state node; transitions can be made bidirectional, drawn with an arrowhead at each end; and the state nodes get refreshed sizing and spacing.
- **Reorder game systems.** In the Systems tree you can now move a system up or down within its group, nest it under another system as a subsystem, or move it back to the top level.
- **MCP server authentication options.** Each project's MCP Server settings now let you choose whether AI clients must present an auth token to connect, and whether the token and port stay stable across launches or are regenerated each session.
- **Customize MCP port.** You can now customize the port the MCP server uses — set a default base port for new projects in App Settings, and override it per project.

### Fixed
- **Automatic recovery from a damaged local cache.** If a project's local cache becomes corrupted, the app now detects it, rebuilds the cache from your project files, and retries the request — so you see your data instead of an error.
- **Plugin settings no longer leak into standalone sections.** A leftover from earlier code caused some plugin settings to show up in separate standalone sections; each plugin's settings now live only inside its own card on the project Plugins settings page.
- Fixed a label collision in the room preview where the floor surface and the floor level showed the same label.
- Clarified the help text for inheriting a room's default trims.

### Changed
- Reviewed and expanded translations across all 11 supported languages, including visual-effects, status-filter, and several dialog and banner strings.
- Reworded the MCP server authentication and base-port setting descriptions for clarity.
- Bug-tracker items shown on the dashboard board now carry a "Bug" tag.

## 0.8.9 — 2026-05-15

### Added
- Cutscenes: link characters, items, and other entities as their own lanes on a cutscene's timeline, then create events directly on the entity lane. Lanes can be reordered by drag and removed inline; each lane has a "+" button that spawns an event at the next free slot, and the timeline edge-pans automatically while dragging near the container edges.
- Dialogues: speakers can now be linked to entities (typically characters, but anything linked to the dialogue from the metadata panel) via a picker, in addition to the existing freeform speaker label.
- Category templates on Cutscenes, Dialogues, Lore Codex, Meshes, and Textures — Categories tab on each list page and a category filter in the popover. New toggles in plugin settings to enable or disable category templates for floors, rooms, spritesheets, cutscenes, dialogues, Lore Codex, meshes, and textures.
- Spritesheets: slicing-mode filter in the list filter popover.
- Top bar: Documentation entry in the Help menu.
- Settings: plugin cards collapse to title + tags by default, expand on click to show description and per-plugin configuration.

### Changed
- Cutscenes — event types collapse to Camera, Action, Transition, Custom, and Entity. The standalone dialogue / sfx / ost / flag / item / narration / character / voice over / file event types are gone — those events are now created directly on an entity lane and resolve to the linked entity automatically.
- Cutscenes — the metadata panel is now an inline section on the editor page rather than a separate side sheet.
- Top bar — the "About" menu is now "Help" (still hosts the About entry, plus the new Documentation entry).
- Settings — "Directions" and "Notes" plugins are grouped under the Infrastructure category (previously Project).
- MCP server — every project now has its own bearer token (written to `.sophia/mcp.json` and `.mcp.json`), the HTTP server only answers requests with a localhost host header, CORS is narrowed, and the OAuth discovery paths return a clear pointer to the bearer-token mechanism instead of an opaque "Not Found".

### Fixed
- Room editor — Fixed a bug that blocked snapping a door precisely to Left / Center / Right of its wall, with a clear "No room on this wall for that position" message when it doesn't fit.
- Room editor — Fixed a bug that blocked filleting a corner into a curved wall with an adjustable radius.
- Room editor — Fixed a bug that blocked drag-erasing multiple walls at once with a rectangle.
- Room editor — Fixed a bug where mirror-door pairs didn't render as one continuous wall-spanning shape, and where pairing required two manual steps; the action is now one click and bidirectionally links both ends.
- Room editor — Fixed a bug that blocked configuring thresholds (bridge plates at doorways), skirting transitions (flat / ramped / beveled material-change lines), raised platforms, and inlays in the floor architecture editor.
- UE5 export — Fixed a bug where tweaking a single trim (e.g., chair rail) rebuilt the whole wall; incremental sync now only respawns the actors whose specs actually changed.
- UE5 export — Fixed a bug where surface treatments and trim bands missed end caps and divider walls with both sides exposed; they now emit on every exposed face with mitred extrusion ends and full pattern coverage.
- UE5 export — Fixed a bug that blocked exporting a room's free-standing and wall-attached columns.
- UE5 export — Fixed a bug that blocked configuring the World Outliner folder separately from the placement folder; subfolders for Trims, Surfaces, Windows, Thresholds, Skirting, Platforms, Inlays, and Columns now nest under their parents (plus an optional "Add to level gen" root prefix).
- UE5 export — Fixed a bug where choosing no material for a wall or floor overwrote the existing actor's material with the project default; the existing material is now left alone.
- UE5 asset registration — Fixed a bug where child textures pulled in alongside a material import cluttered the asset list; they're now hidden on creation (pre-existing textures are left alone).
- Status bar — Fixed the portal-arrow visibility toggle showing up on every page; it now lives in the floor canvas's own status bar and only appears while a floor is open.
- Palette window — Fixed the secondary palette window flashing the bare dark defaults before mounting; it now inherits the active theme on open.
- Multiple bugfixes to the Room editor, Cutscenes, and Dialogues plugins.

### Removed
- The old cutscene "Resolve reference" popover and "Unresolved events" section. Events resolve to their entity automatically by virtue of sitting on an entity lane.
- Column orders other than Square pier and Round shaft are temporarily hidden (Doric, Tuscan, Ionic, Composite, Corinthian, Engaged, Asian post, Cluster, Solomonic) and will return when their geometry is finalized.

## 0.8.8 — 2026-05-06

### Added
- VFX plugin: a new top-level catalogue for visual effects, organized into groups with tags, status filters, and per-effect detail pages. VFX entries don't hold images themselves — link Concept Art entries to attach reference imagery and they show up in the preview rail automatically.
- Story endings as first-class entities in the story plugin. Endings appear in their own container on the story canvas and have a summary, free-form unlock conditions, a show-on-canvas toggle, and gating via flag-set / flag-unset relationships. Beats can trigger endings, and endings can require or be required by other endings.
- Directions plugin: per-plugin design-direction notes accessed from a "Direction" button in the toolbar of any list page (rooms, OST, items, characters, …). The modal opens scoped to that plugin; markdown is supported, and notes attach to the direction itself. Directions are also surfaced to AI agents so AI work agrees with the user's intent.
- 3D room and wall preview modals on the room editor — orbit-camera 3D scenes for visualizing room geometry, wall profiles, surface treatments, and trim bands without round-tripping through Unreal.
- Surface treatments and trim bands editors for rooms and walls, with per-room defaults and per-wall overrides.
- Floor architecture editor for cross-floor concerns (default trims, default surface treatments) shared across the floors of a project.
- Glass walls and arched-shape doors in the room editor.
- Columns, windows, and post-it notes as floor-canvas entities — drawn directly on the floor canvas alongside walls and doors, with their own properties panels.
- Wall cascade tools: split, merge, and an "engulf" modal when one wall absorbs another, with lineage tracked across the change.
- Status-bar toggle for portal-arrow visibility on the floor canvas — flip between always-visible and mouseover-only without leaving the canvas.
- Rich update-available modal showing version, what's new, and Update now / Update on next launch / Skip this version / Remind me later actions, plus background-download toasts and a Restart-and-install prompt when the update is ready.
- Backend health banner that surfaces when the backend disconnects, with Reconnecting / Stopped-responding states and Restart app / Quit actions.
- Project integrity modal: when a project's project file is missing settings or unreadable, the app explains the issue and offers Fix and continue (writes safe defaults) or Close project.
- "Merge geometry into single mesh per wall" option in the UE5 room export dialog — bakes walls + trim bands + surface treatments into one actor per wall for fewer draw calls.
- Categories tab on the floors, ambience, and rooms list pages.
- Project-settings toggles for ambience, OST, SFX-group, VFX-group, voice-actor, and state-machine category templates.
- Native clipboard read/write bridge for renderer features that need real OS clipboard access.

### Changed
- Color picker now opens with the RGBA tab active by default (order: RGBA, hex, LCH).
- Outline and ghost buttons share a unified accent-bordered transparent style across the app.
- Constellation overhaul: procedural twinkling starfield background, softer halos on nodes, tooltip card now stays mounted through its exit animation, and additional entity-type colors for VFX, VFX groups, ambience, spritesheets, state machines, and fonts. Snapshots no longer include non-trackable entity types.

### Fixed
- Wall edits surface clearer error messages when an update, split, or merge can't complete.
- Door placement and validation rewritten end-to-end: arched and standard doors are correctly clamped to their host wall, portal partners stay in sync across floors, and orphaned partner references are cleaned up when a wall is split, merged, or engulfed.
- Wall corner geometry is more reliable at acute and obtuse junctions — interior normals, fillets, and surface-treatment seams line up where adjacent walls meet.
- Wall extents are preserved correctly when a wall is reshaped or its lineage changes, so trim bands and surface treatments no longer drift off the wall after a split or merge.

## 0.8.7 — 2026-04-24

### Added
- New Levels plugin with Tiled map integration, per-level image gallery, active tileset selector, and spritesheet-based rendering
- New Spritesheets plugin for managing and editing sprite animations
- New State Machines plugin with a list page and a node-based state machine editor
- Welcome modal on first launch with language and theme selection plus onboarding info
- Multi-step Create Project wizard covering name and location, general info, project DNA, game details, and plugin selection, with a project logo uploader and inline per-plugin help while picking plugins
- Plugin help dialog accessible from plugin sidebars, showing description, audience, and how-to-use guidance
- Login dialog and Account settings section with editable first name, last name, and alias fields, email display, and sign-out
- Dedicated Theme settings section
- Top bar can now optionally display the signed-in user's name or alias
- Category filter bar with OR/AND query operators on the newly added plugins' list pages
- Configurable per-project asset folder location
- Pluggable project settings sections, with Project Tracking exposing effort weights, status completion percentages, and a gamejam mode toggle
- App now monitors its own memory usage in the background for diagnostics

### Changed
- Level Designer renamed and reworked into Room Editor — canvas-based floor designer with door placement, minimap overlay, and per-room detail pages; what used to be called "Levels" in that workflow is now "Floors", and rooms live inside floors
- Settings page reorganized around Account and Theme sections
- Repo-wide internal refactoring, code formatting pass, test suite reorganization, and lots of bug fixes

### Removed
- Claude Code integration inside the app

## 0.8.6 — 2026-04-17

### Added
- **Theme system.** Ten built-in themes replace the hardcoded dark emerald look: Midnight Blue, Amethyst, Golden Hour, Neutral Gray, Light Emerald, Blue-Orange (dark), Blue-Orange (light), High Contrast (dark), and High Contrast (light). The two Blue-Orange themes are designed for color-vision accessibility (deuteranopia, protanopia, tritanopia). Light mode is now a first-class option. All colors cross-fade smoothly when switching themes. A new Appearance section in App Settings lets you pick your theme; project-level "system theme" palettes also appear in the list with an option to override them.
- **Color Palettes plugin.** Create and manage named color palettes with free-form or system-theme types. System-theme palettes define fixed named slots that map to the app's UI surfaces. Regular palettes support adding, removing, and reordering colors. Any palette can be opened in a standalone floating reference window that persists its size and position across sessions.
- **Audio Ambience plugin.** Manage ambient audio clips with support for looping/non-looping filtering, audio file uploads (MP3, WAV, OGG, WebM), and per-project file storage. Integrates with entity linking and production tracking.
- **Fonts plugin.** Manage font families with multiple variants (weight, style). Upload TTF, OTF, WOFF, or WOFF2 files per variant, preview them inline, and reorder or remove individual variants.
- **Notes plugin.** Attach notes to any entity across the app. Notes support markdown body, tags, color coding, and pinning. A compact Notes section appears on every entity's detail page showing a preview of the pinned or latest note.
- **Portal doors in the Level Designer.** Doors can now link across two adjacent rooms as portals. When Room A and Room B share a wall, placing a door near the boundary can pair it with the matching wall on the other side. The properties panel shows a Portal section for linking, viewing the linked room, and unlinking. An auto-link setting makes this happen automatically when doors are placed near eligible walls.
- **Game Jam mode for Project Tracking.** When enabled in project settings, progress calculation treats every tracked item as equal weight (ignoring effort estimates) and items with unknown effort no longer appear in the "needs attention" list.

### Changed
- **Concept Art gallery** now supports filtering by illustration stage (Rough, Sketch, Line Art, Color, Final) with a multi-select popover filter alongside the search bar, replacing the previous single-value tab filter.
- **Cutscene list** rebuilt as a sortable table with columns for name, description, status, event count, and last updated — with a multi-select status filter popover.
- **Project Tracking progress** now reads effort weights and status completion percentages from project settings instead of using fixed defaults.
- **Sidebar** no longer wraps plugin sections in an extra collapsible group — sections sit directly at the top level.
- Every plugin across the app now uses theme tokens instead of hardcoded colors, so all UI consistently follows the active theme.

### Removed
- **Code Explorer** — the file browser and Monaco editor sidebar panel have been removed, along with the Monaco and file-icon dependencies.
- Internal dead code cleanup across the app.

## 0.8.5 — 2026-04-16

### Added
- Template export and import. You can now bundle selected entities into a reusable ZIP and merge them back into another project. The export dialog lets you pick which entities to include, name the template, and toggle extras per entity type — for example, include relationships between the selected characters, include flags alongside acts, and choose whether to embed all, only referenced, or no category templates. Co-owned children travel automatically (recipes follow their items, beats follow their acts). The import dialog detects per-entity conflicts and lets you resolve each one individually or apply the same choice to all: overwrite the local copy, keep the local copy, or import as a fresh copy with a new ID. You can also pick an ID strategy up front (preserve original IDs or generate new ones). After import, a warning appears if any imported data belongs to plugins that are currently disabled.
- Drag-to-resize sidebar width (persists across sessions), with a Reset Sidebar Width action in the sidebar menu.
- Ellipsis menu on sidebar sections with entries for Plugin Settings, Disable Plugin (with a confirmation dialog), Help, Collapse All / Expand All, Collapse All Others, Collapse All Sub-sections, and Manage Plugins.

### Fixed
- External edits inside a project's `.sophia` folder are again picked up as live updates — the dotfile ignore filter was incorrectly excluding the folder itself.
- With multiple projects open, entity changes in one project no longer refresh data shown for another project.
- Rename validation messages in the file tree are now localized instead of hardcoded English.
- The category templates filter bar in list views now only lists templates scoped to the current entity type.
- Live data updates reconnect automatically when the backend restarts on a new port.

## 0.8.4 — 2026-04-12

**First public alpha — the first ever release of Sophia Forge.**

### Added
- Auto-update preferences (Settings → Editor): toggle automatic checks and set a custom interval. Changes apply without restart — installed versions will receive new releases automatically from here on.
- Plugins can declare system requirements — missing dependencies are surfaced in Settings → Plugins with the toggle disabled and an "Unavailable — required system dependency not detected" notice.
- Plugins can ship as opt-in; new projects start with the standard set and optional plugins must be turned on explicitly.
- New app icon set across Windows, macOS, and Linux with full-resolution artwork.
- Multi-language Windows installer.
- About dialog now displays the real installed app version.
- Third-party licenses bundled (EN/PT-BR).
- Friendlier startup screen: "Getting the anvil ready…" message with a larger title and live boot step underneath.

### Changed
- Only a single instance of Sophia Forge can run at a time; launching while it's already open focuses the existing window instead of starting a second copy.

### Fixed
- Packaged builds now locate the backend, preload script, and renderer assets reliably, fixing startup failures in installed versions.
- Backend now runs as an Electron utility process, improving stability and clean shutdown.

### Internal
- Large refactor of the end-to-end test suite and expanded unit test coverage across renderer components and plugin APIs.

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

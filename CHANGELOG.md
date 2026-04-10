# Changelog

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

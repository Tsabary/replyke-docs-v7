# Docs v7 — Landing Page Data

Accumulated product observations from agents reading source code during documentation phases. Raw material for a future landing page agent. Not a design brief.

---

## Auth & Identity

- Supports email/password authentication with JWT access tokens and refresh tokens
- OAuth support for Google, GitHub, and other configurable providers
- Users can link multiple OAuth providers to a single account (multi-identity)
- External/custom auth mode: developers can bring their own JWT issuer and Replyke verifies tokens
- SDK handles full token lifecycle automatically (refresh, storage, account switching)
- Multi-account support: users can be signed into multiple accounts simultaneously and switch between them
- See: `server/src/v7/controllers/auth/`, `server/src/v7/controllers/oauth/`, `monorepo/packages/core/src/hooks/auth/`

## Chat

- Real-time 1:1 and group conversations powered by Socket.io
- Message reactions, threading (reply-to), optimistic UI updates via client ID deduplication
- Typing indicators, unread counts per conversation, mark-as-read
- Each Space can have an integrated chat conversation (created on demand)
- Full moderation: admins can moderate chat messages and resolve chat reports within a space
- See: `server/src/v7/controllers/chat/`, `monorepo/packages/core/src/hooks/chat/`

## Spaces

- Hierarchical community containers — spaces can nest up to 10 levels deep
- Per-space permission model: reading and posting independently configurable (`anyone`, `members`, `admins`)
- Approval workflow: spaces can require admin/moderator approval before new members become active
- Three membership roles: admin, moderator, member — with cascading governance to child spaces
- Built-in moderation: approve/remove entities, comments, and chat messages; handle member reports
- Community rules: ordered, admin-managed, publicly readable list of community guidelines
- Space mentions: `#space-slug` autocomplete for referencing spaces in content
- Optional slug-based addressing (Unicode slugs supported)
- Digest/newsletter webhook: periodic content summaries delivered to a configurable URL on a schedule
- Semantic search support: space names/descriptions indexed for AI-powered discovery
- See: `server/src/v7/controllers/spaces/`, `monorepo/packages/core/src/hooks/spaces/`, `server/src/models/core/Space.ts`

## Entities

- The "entity" is a generic content unit — a blog post, product listing, forum thread, photo, or any item your app centers around
- `foreignId` pattern: link Replyke entities to existing items in your own system by ID — no data duplication required
- `createIfNotFound`: social infrastructure auto-creates on first access (great for "entity per URL" patterns)
- Short IDs auto-generated on every entity — ready-made for shareable/embeddable URLs
- Draft/publish workflow built in: create as `isDraft: true`, publish when ready
- File and image uploads supported at entity creation time (multipart)
- `infuseData` callback in `useEntityList`: merge Replyke entity lists with data from your own backend at render time, with per-foreignId caching — no server-side join needed
- See: `monorepo/packages/core/src/hooks/entities/`, `monorepo/packages/core/src/hooks/entity-lists/`

## Entity Feed Filtering

- Entity lists support a rich filter system: keywords (include/exclude), title/content substring match, attachments presence, geographic radius, metadata key-value matching
- Metadata filters support `includes`, `includesAny`, `doesNotInclude`, `exists`, `doesNotExist` — allowing arbitrary faceted queries against developer-defined fields
- Sort by `"hot"`, `"top"`, `"new"`, `"controversial"`, or any metadata field (numeric, text, boolean, timestamp)
- `followedOnly` filter: show only content from users the current user follows
- See: `monorepo/packages/core/src/interfaces/entity-filters/`

## Reactions

- 8 reaction types on both entities and comments: `upvote`, `downvote`, `like`, `love`, `wow`, `sad`, `angry`, `funny`
- Each type has a defined reputation impact on the author (upvote +1, love +2, downvote -1, etc.)
- `useReactionToggle` handles optimistic updates, toggle-to-remove, and state revert on error — one hook covers the full reaction UX
- See: `monorepo/packages/core/src/hooks/reactions/`, `monorepo/packages/core/src/interfaces/models/Reaction.ts`

## Comments

- CommentSectionProvider manages a full comment tree: paginated loading, optimistic placeholder on submit, Reddit-style soft delete (content hidden, thread preserved)
- GIF attachment support built into the comment schema
- `autoReaction` on comment create: optionally react to your own comment immediately after posting (e.g. auto-upvote)
- Deep-link to a specific comment via `highlightedCommentId` — fetches the comment and its parent, injects into tree
- `@user` and `#space` mention autocomplete configurable via trigger characters
- See: `monorepo/packages/core/src/hooks/comments/`

## User Data Model

- `secureMetadata`: same as `metadata` but excluded from all other users' views — useful for storing private user attributes alongside public ones without a separate API call
- `authMethods` on `AuthUser`: tells the client which auth methods the user has set up (e.g. `["password", "google"]`)
- See: `monorepo/packages/core/src/interfaces/models/User.ts`

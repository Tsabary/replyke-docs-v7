# Docs v7 — Structure Plan

## What This Document Is

This is the complete information architecture for the v7 docs site. Every page that will exist is listed here, with its file path, description, classification, and (where applicable) the corresponding v6 reference file.

Agents in Phase 2 (scaffolding) and Phases 3–4 (writing) work from this document as their blueprint.

---

## Navigation Design

### Tab Structure

The site has **6 tabs**:

| Tab | Purpose |
|-----|---------|
| **Overview** | Product orientation. Conceptual guides that apply to everything: what Replyke is, integration paths, auth model, webhooks, semantic search/AI, MCP. Non-feature-specific. |
| **SDK Reference** | Integration guides. Explains *how to add features to your app* using the React/React Native SDK. Covers context providers, high-level hooks, and common patterns. Assumes the developer wants to understand a feature end-to-end. |
| **Hooks Reference** | Complete one-per-hook technical reference. Every hook, all parameters, all return values. Use this when you need to know exactly what a hook accepts and returns. |
| **API Reference** | Complete one-per-endpoint REST reference. Every endpoint, all request/response fields. For developers calling the API directly (no SDK). |
| **Data Models** | TypeScript interfaces developers receive. Documents the public-facing data shapes from `@replyke/core/interfaces`. |
| **Components** | CLI component system (`@replyke/cli`). Shadcn-style installable UI components. |

### SDK Reference vs Hooks Reference — The Key Distinction

This was a source of confusion in v6. The rule:

- **SDK Reference** answers "how do I add [feature] to my app?" It explains the integration pattern — which provider to wrap, which hook to use, common flows. Pages may cover multiple hooks together in context.
- **Hooks Reference** answers "what exactly does `useX` accept and return?" One page per hook. Mechanical reference.

A developer building a comment section reads SDK > Comments for the overall approach, then refers to Hooks Reference for precise parameter shapes.

### What is NOT in the docs

- **Lists** — legacy feature, remains supported in the API, but not documented in v7 docs. New developers are not directed here.
- **Node SDK (`@replyke/node`)** — out of date, not documented.
- **JS SDK (`@replyke/js`)** — out of date, not documented.

---

## Tab 1: Overview

### Group: Getting Started

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `index.mdx` | Introduction to Replyke — what it provides, who it's for, what problems it solves. Quick-start card grid linking to SDK getting started, API reference, Components, and key features. | UPDATED | `docs/index.mdx` |
| `integration-options.mdx` | The different ways to integrate Replyke: React/React Native SDK (recommended), raw REST API, building with the SDK vs building custom. Helps developers choose the right path for their project. | UPDATED | `docs/integration-options.mdx` |
| `authentication.mdx` | Conceptual overview of all three auth modes: built-in email/password, external auth via signed JWT, and OAuth. Explains the token model (access + refresh tokens), how to keep secrets server-side, and when to use each mode. Links to the SDK and API auth sections for implementation. | UPDATED | `docs/authentication.mdx` |
| `webhooks.mdx` | Replyke's webhook system: when Replyke calls your server (app notification delivery for push notification bridging, space digest/newsletter delivery). Payload structure, HMAC signature validation, webhook security best practices. | NEW | — |
| `semantic-search-ai.mdx` | v7 automatic content embedding — entities, comments, messages, users, and spaces are embedded automatically when enabled. What this enables: semantic search across content, users, and spaces; the AI ask endpoint that synthesizes answers from your content. How to enable embeddings per-project. Links to SDK Search and API Search sections. | NEW | — |
| `mcp-server.mdx` | MCP server integration for AI tooling (Cursor, Claude Code, etc.). | CARRY-OVER | `docs/mcp-server.mdx` |

---

## Tab 2: SDK Reference

### Group: Setup

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/getting-started.mdx` | Installing `@replyke/react-js` or `@replyke/react-native`. Wrapping the app with `ReplykeProvider` (projectId). Making the first SDK call. The minimal setup to get Replyke working. | UPDATED | `docs/sdk/getting-started.mdx` |
| `sdk/redux-integration.mdx` | For apps that already have a Redux store: using `ReplykeIntegrationProvider` instead of `ReplykeProvider`. How to configure `replykeReducers`, `replykeApiReducer`, and `replykeMiddleware` in the app's own store. When to use this vs the default provider. | NEW | — |

### Group: Authentication

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/authentication/overview.mdx` | What the auth SDK provides: auth state via `useAuth`, sign-up/in/out, account state, the `signedToken` prop for external auth. How auth state flows through the app and auto-refreshes. | UPDATED | `docs/sdk/authentication/overview.mdx` |
| `sdk/authentication/built-in.mdx` | Built-in email/password auth: implementing sign-up, sign-in, sign-out, and password reset flows using SDK hooks. Common patterns like persisting session and handling auth errors. | UPDATED | `docs/sdk/authentication/built-in.mdx` |
| `sdk/authentication/external.mdx` | External auth integration: generating a signed JWT on your backend, passing it to `ReplykeProvider` via `signedToken`, how Replyke verifies or creates the user. Security: why the signing secret must stay server-side. | UPDATED | `docs/sdk/authentication/external.mdx` |
| `sdk/authentication/oauth.mdx` | OAuth provider integration (Google, GitHub, etc.): configuring providers in the dashboard, initiating OAuth with `useOAuthSignIn` (React web), handling the callback, linking additional providers to an existing account. PKCE explained. | NEW | — |
| `sdk/authentication/multi-account.mdx` | Managing multiple user accounts in a single app instance: `useAccounts` for the account list, `useSwitchAccount` to change the active user, `useAddAccount` to link a new account, `useRemoveAccount` to unlink, `useSignOutAll` to clear everything. | NEW | — |

### Group: Current User

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/current-user/overview.mdx` | The `useUser` and `useUserActions` hooks — exclusively for the authenticated user's own state. Reading profile data, updating name/bio/username/birthdate, uploading avatar and banner images. This section is only about the logged-in user; for fetching other users' profiles see User Profiles. | UPDATED | `docs/sdk/users/use-user-hook.mdx` |

### Group: User Profiles

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/user-profiles/overview.mdx` | Hooks for working with other users' public profiles across the project: fetching by ID, foreign ID, or username with `useFetchUser` and variants. Username availability with `useCheckUsernameAvailability`. Mention suggestions and @mention autocomplete with `useUserMentions`. These hooks are not scoped to the current user. | NEW | — |

### Group: Entities

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/entities/overview.mdx` | What entities are: the core content unit (posts, articles, pages) around which comments, votes, reactions, and views attach. The `EntityProvider` pattern — wrapping a piece of content to give its subtree full entity state and actions. How votes, reactions, and view increments work. | UPDATED | `docs/sdk/entities/overview.mdx` |
| `sdk/entities/provider-and-hook.mdx` | Setting up `EntityProvider` by entity ID, foreign ID, short ID, or a full entity object passed directly. Using `useEntity` to access all entity data and trigger actions (vote, react, increment views, update, delete) from context anywhere in the subtree. | UPDATED | `docs/sdk/entities/provider-and-hook.mdx` |
| `sdk/entities/drafts-and-publishing.mdx` | Creating draft entities (`isDraft: true` on create), listing the current user's drafts with `useFetchDrafts`, publishing a draft with `usePublishDraft`. Use cases: scheduled publishing, content review workflows. | NEW | — |

### Group: Entity Lists

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/entity-lists/overview.mdx` | What entity lists are and when to use `useEntityList`: paginated, filtered, sorted collections of entities for feeds, search results, profile pages. The list state shape and how it connects to `EntityProvider`. | UPDATED | `docs/sdk/entity-lists/overview.mdx` |
| `sdk/entity-lists/fetch-entities.mdx` | Fetching, paginating, sorting, and refreshing entity lists. `useEntityListActions` for refresh and load-more. Configuring sort order (newest, hottest, most-viewed, score). | UPDATED | `docs/sdk/entity-lists/fetch-entities.mdx` |
| `sdk/entity-lists/infuse-data.mdx` | Using `useInfusedData` to merge the server-fetched entity list with local state overrides — useful for optimistic updates or enriching entities with local data before rendering. | CARRY-OVER | `docs/sdk/entity-lists/infuse-data.mdx` |
| `sdk/entity-lists/filters.mdx` | Complete filter reference for entity lists: title, content, keywords, attachments, location (geo-radius), metadata, spaceId, sourceId, draft status, userId. How to combine filters. | UPDATED | `docs/sdk/entity-lists/filters/` (multiple pages consolidated) |

### Group: Comments

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/comments/overview.mdx` | What the comments system provides: threaded comments and replies on entities, soft-delete behavior (content cleared, structure preserved), reactions, mentions, GIF attachments. How to build a comment integration using `CommentSectionProvider` and `useCommentSection` — without assuming the CLI component is used. The CLI component is mentioned at the end as an optional ready-made implementation that uses the same hooks. | UPDATED | — |
| `sdk/comments/comment-section.mdx` | `CommentSectionProvider` + `useCommentSection` in depth: the full comment section state (comments, pagination, sort), creating top-level comments and replies, deleting, reactions on comments, mention autocomplete. | UPDATED | — |

### Group: Reactions

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/reactions/overview.mdx` | The reaction system: 8 reaction types (upvote, downvote, like, love, wow, sad, angry, funny), how reactions apply to entities and comments separately, `reactionCounts` on entity/comment objects, `userReaction` for the current user's reaction, reputation impact per type. Using `useReactionToggle` as the primary integration pattern. | NEW | — |

### Group: Spaces

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/spaces/overview.mdx` | What Spaces are: hierarchical community spaces with their own membership, permissions, moderation, rules, digest, and chat. Key concepts: slug (URL-friendly identifier), parentSpaceId/depth (hierarchy), reading/posting permissions, `requireJoinApproval`. | NEW | — |
| `sdk/spaces/provider-and-hook.mdx` | `SpaceProvider` + `useSpace`: accessing space data, membership status, full `SpaceMemberPermissions` (canPost, canModerate, canRead, isAdmin, isModerator), and space actions from context. | NEW | — |
| `sdk/spaces/membership.mdx` | Joining a space (and the approval workflow when `requireJoinApproval` is true), leaving, listing members by role, `useSpacePermissions` for runtime permission checks, managing roles (admin/moderator/member), banning and unbanning users. | NEW | — |
| `sdk/spaces/moderation.mdx` | Moderating content within a space: approving/removing entities and comments (`useModerateSpaceEntity`, `useModerateSpaceComment`), moderating chat messages, handling reports from `useFetchModeratedReports`. | NEW | — |
| `sdk/spaces/rules.mdx` | Community rules: creating ordered rules with `useCreateRule`, editing with `useUpdateRule`, drag-and-drop reorder with `useReorderRules`, listing with `useFetchManyRules`. | NEW | — |
| `sdk/spaces/digest-newsletter.mdx` | Space digest/newsletter: what it is (periodic summary sent to a webhook), configuring schedule and timezone, setting the webhook URL, `useFetchDigestConfig`, `useUpdateDigestConfig`. References the Webhooks Overview page for HMAC validation. | NEW | — |
| `sdk/spaces/space-lists.mdx` | Fetching paginated lists of spaces with `useSpaceList`: filtering, sorting, pagination. `useSpaceListActions` for refresh and load-more. Use cases: space discovery pages, "your spaces" sidebar. | NEW | — |

### Group: Chat

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/chat/overview.mdx` | What Replyke Chat provides: real-time 1:1 and group messaging with reactions, message threads, read receipts, typing indicators, file attachments, and moderation. Architecture: socket.io connection managed by `ChatProvider`, per-conversation state in `ConversationProvider`. | NEW | — |
| `sdk/chat/setup.mdx` | Placing `ChatProvider` in the component tree (must be inside `ReplykeProvider`, outside any conversation UI). How the socket connection is established and maintained. `ConversationProvider` for a single conversation view. | NEW | — |
| `sdk/chat/conversations.mdx` | Creating direct (1:1) conversations with `useCreateDirectConversation`, creating group conversations, listing conversations with `useConversations` (includes `ConversationPreview` with unread count and last message). `ConversationProvider` for managing single-conversation state. | NEW | — |
| `sdk/chat/messages.mdx` | Sending messages with `useSendMessage` (optimistic updates, `clientId` for deduplication), editing with `useEditMessage`, deleting with `useDeleteMessage`. File attachments in messages. Message data shape. | NEW | — |
| `sdk/chat/real-time.mdx` | Real-time features: typing indicators with `useTypingIndicator`, total unread count with `useTotalUnreadCount`, per-conversation unread count, marking conversations as read with `useMarkConversationAsRead`. | NEW | — |
| `sdk/chat/threads.mdx` | Message threads: `MessageThreadProvider` (must be inside `ConversationProvider`), `useMessageThread` for loading thread replies, replying to a message, `threadReplyCount` on the parent message. | NEW | — |

### Group: Collections

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/collections/overview.mdx` | What Collections are: a personal hierarchical bookmark system per user. Each user has a root collection and can create sub-collections. Entities are saved into collections. Key concepts: `isRoot`, `parentId`, `entityCount`. How collections differ from Lists (legacy). | NEW | — |
| `sdk/collections/managing-collections.mdx` | `useCollections` for navigating the collection hierarchy, `useCollectionsActions` for creating/renaming/deleting collections and adding/removing saved entities. `useIsEntitySaved` to check if an entity is already saved. | NEW | — |

### Group: Relationships

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/relationships/overview.mdx` | Follows (unidirectional — one user follows another) vs Connections (bidirectional — mutual confirmation required). When to use each. How they interact with user profiles and feeds. | CARRY-OVER | `docs/sdk/relationships/overview.mdx` |
| `sdk/relationships/use-follow-manager.mdx` | `useFollowManager`: manages follow state for a user pair — current follow status, the follow action, and the unfollow action — in a single hook. Ideal for follow buttons on profile cards. | CARRY-OVER | `docs/sdk/relationships/use-follow-manager.mdx` |
| `sdk/relationships/use-connection-manager.mdx` | `useConnectionManager`: manages connection state for a user pair — current connection status (none/pending-sent/pending-received/accepted), and request/accept/decline/remove actions. Ideal for connection buttons. | CARRY-OVER | `docs/sdk/relationships/use-connection-manager.mdx` |

### Group: App Notifications

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/app-notifications/overview.mdx` | What app notifications are: in-app notification records triggered by system events (new comment, new follower, etc.). The notification type system, how they're triggered automatically. References the Components tab for the ready-made notification UI component. | UPDATED | `docs/sdk/app-notifications/overview.mdx` |
| `sdk/app-notifications/hook.mdx` | Using the notifications hook: fetching paginated notifications, getting the unread count, marking single or all notifications as read. Building notification badge UI. | UPDATED | `docs/sdk/app-notifications/hook.mdx` |
| `sdk/app-notifications/notification-templates.mdx` | All notification type strings and the metadata shape for each type. How to map notification types to display messages and icons in a custom notification UI. | UPDATED | `docs/sdk/app-notifications/notification-templates.mdx` |
| `sdk/app-notifications/webhook-integration.mdx` | Bridging app notifications to push notifications (FCM, APNs, web push): Replyke calls your server webhook when a notification is created, you relay it to the device. Links to the Webhooks Overview page for HMAC validation and payload format. | UPDATED | `docs/sdk/app-notifications/webhook-integration.mdx` |

### Group: Search & AI

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/search/overview.mdx` | Semantic search capabilities enabled by v7 auto-embedding: content search (entities/comments/messages), user search, space search, and the AI ask endpoint. Prerequisites: semantic embeddings must be enabled in the project dashboard. | NEW | — |
| `sdk/search/semantic-search.mdx` | Using `useSearchContent` (search over entities, comments, and messages by natural language), `useSearchUsers` (find users by profile description), `useSearchSpaces` (find spaces by description). Query patterns, scoping results to a specific space or conversation. | NEW | — |
| `sdk/search/ask.mdx` | `useAskContent` — ask a natural language question, receive an answer synthesized from the embedded content in your project. How it works internally (retrieval + generation), response shape, scoping to a space or conversation. | NEW | — |

### Group: Storage

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/storage/overview.mdx` | Uploading files and images with `useUploadFile` (generic files, no processing) and `useUploadImage` (images — processed into thumbnail/small/medium/large variants). How files attach to content: entities, comments, messages, users (avatar/banner), and spaces (avatar/banner). The `File` and `FileImage` types. | NEW | — |

### Group: Moderation

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `sdk/moderation/overview.mdx` | How content moderation works in Replyke: moderation status fields on entities, comments, and messages (`approved`/`removed`/null), user-level reporting with `useCreateReport`, space-level moderation (moderators manage content within their space), platform-level moderation (project admins). | UPDATED | `docs/sdk/moderation.mdx` |

---

## Tab 3: Hooks Reference

One page per hook. Format: purpose, parameters, return values, usage notes.

### Group: Auth Hooks

| File | Hook | Class |
|------|------|-------|
| `hooks/auth/use-auth.mdx` | `useAuth` | UPDATED |
| `hooks/auth/use-accounts.mdx` | `useAccounts` | NEW |
| `hooks/auth/use-add-account.mdx` | `useAddAccount` | NEW |
| `hooks/auth/use-remove-account.mdx` | `useRemoveAccount` | NEW |
| `hooks/auth/use-switch-account.mdx` | `useSwitchAccount` | NEW |
| `hooks/auth/use-sign-out-all.mdx` | `useSignOutAll` | NEW |
| `hooks/auth/use-oauth-identities.mdx` | `useOAuthIdentities` | NEW |
| `hooks/auth/use-request-password-reset.mdx` | `useRequestPasswordReset` | NEW |

### Group: Chat — Conversations

| File | Hook | Class |
|------|------|-------|
| `hooks/chat/conversations/use-conversations.mdx` | `useConversations` | NEW |
| `hooks/chat/conversations/use-conversation.mdx` | `useConversation` | NEW |
| `hooks/chat/conversations/use-create-direct-conversation.mdx` | `useCreateDirectConversation` | NEW |
| `hooks/chat/conversations/use-conversation-members.mdx` | `useConversationMembers` | NEW |
| `hooks/chat/conversations/use-space-conversation.mdx` | `useSpaceConversation` | NEW |

### Group: Chat — Messages

| File | Hook | Class |
|------|------|-------|
| `hooks/chat/messages/use-chat-messages.mdx` | `useChatMessages` | NEW |
| `hooks/chat/messages/use-send-message.mdx` | `useSendMessage` | NEW |
| `hooks/chat/messages/use-edit-message.mdx` | `useEditMessage` | NEW |
| `hooks/chat/messages/use-delete-message.mdx` | `useDeleteMessage` | NEW |
| `hooks/chat/messages/use-toggle-reaction.mdx` | `useToggleReaction` | NEW |
| `hooks/chat/messages/use-message-thread.mdx` | `useMessageThread` | NEW |

### Group: Chat — Core

| File | Hook | Class |
|------|------|-------|
| `hooks/chat/use-chat-socket.mdx` | `useChatSocket` | NEW |
| `hooks/chat/use-mark-conversation-as-read.mdx` | `useMarkConversationAsRead` | NEW |
| `hooks/chat/use-report-message.mdx` | `useReportMessage` | NEW |
| `hooks/chat/use-total-unread-count.mdx` | `useTotalUnreadCount` | NEW |
| `hooks/chat/use-typing-indicator.mdx` | `useTypingIndicator` | NEW |
| `hooks/chat/use-unread-conversation-count.mdx` | `useUnreadConversationCount` | NEW |

### Group: Collections

| File | Hook | Class |
|------|------|-------|
| `hooks/collections/use-collections.mdx` | `useCollections` | NEW |
| `hooks/collections/use-collections-actions.mdx` | `useCollectionsActions` | NEW |
| `hooks/collections/use-collection-entities-wrapper.mdx` | `useCollectionEntitiesWrapper` | NEW |
| `hooks/collections/use-is-entity-saved.mdx` | `useIsEntitySaved` | NEW |

### Group: Comments

| File | Hook | Class |
|------|------|-------|
| `hooks/comments/use-comment-section.mdx` | `useCommentSection` | UPDATED |
| `hooks/comments/use-entity-comments.mdx` | `useEntityComments` | UPDATED |
| `hooks/comments/use-replies.mdx` | `useReplies` | UPDATED |
| `hooks/comments/use-create-comment.mdx` | `useCreateComment` | UPDATED |
| `hooks/comments/use-update-comment.mdx` | `useUpdateComment` | UPDATED |
| `hooks/comments/use-delete-comment.mdx` | `useDeleteComment` | UPDATED |
| `hooks/comments/use-fetch-comment.mdx` | `useFetchComment` | CARRY-OVER |
| `hooks/comments/use-fetch-comment-by-foreign-id.mdx` | `useFetchCommentByForeignId` | CARRY-OVER |
| `hooks/comments/use-fetch-many-comments.mdx` | `useFetchManyComments` | UPDATED |

### Group: Crypto

| File | Hook | Class |
|------|------|-------|
| `hooks/crypto/use-sign-testing-jwt.mdx` | `useSignTestingJwt` | CARRY-OVER |

### Group: Entities

| File | Hook | Class |
|------|------|-------|
| `hooks/entities/use-entity.mdx` | `useEntity` | UPDATED |
| `hooks/entities/use-fetch-entity.mdx` | `useFetchEntity` | UPDATED |
| `hooks/entities/use-fetch-entity-by-foreign-id.mdx` | `useFetchEntityByForeignId` | CARRY-OVER |
| `hooks/entities/use-fetch-entity-by-short-id.mdx` | `useFetchEntityByShortId` | CARRY-OVER |
| `hooks/entities/use-fetch-many-entities.mdx` | `useFetchManyEntities` | UPDATED |
| `hooks/entities/use-create-entity.mdx` | `useCreateEntity` | UPDATED |
| `hooks/entities/use-update-entity.mdx` | `useUpdateEntity` | UPDATED |
| `hooks/entities/use-delete-entity.mdx` | `useDeleteEntity` | CARRY-OVER |
| `hooks/entities/use-fetch-drafts.mdx` | `useFetchDrafts` | NEW |
| `hooks/entities/use-publish-draft.mdx` | `usePublishDraft` | NEW |
| `hooks/entities/use-increment-entity-views.mdx` | `useIncrementEntityViews` | CARRY-OVER |

### Group: Entity Lists

| File | Hook | Class |
|------|------|-------|
| `hooks/entity-lists/use-entity-list.mdx` | `useEntityList` | UPDATED |
| `hooks/entity-lists/use-entity-list-actions.mdx` | `useEntityListActions` | UPDATED |
| `hooks/entity-lists/use-infused-data.mdx` | `useInfusedData` | CARRY-OVER |

### Group: Reactions

| File | Hook | Class |
|------|------|-------|
| `hooks/reactions/use-add-reaction.mdx` | `useAddReaction` | NEW |
| `hooks/reactions/use-remove-reaction.mdx` | `useRemoveReaction` | NEW |
| `hooks/reactions/use-reaction-toggle.mdx` | `useReactionToggle` | NEW |
| `hooks/reactions/use-fetch-entity-reactions.mdx` | `useFetchEntityReactions` | NEW |
| `hooks/reactions/use-fetch-comment-reactions.mdx` | `useFetchCommentReactions` | NEW |

### Group: Follows

| File | Hook | Class |
|------|------|-------|
| `hooks/follows/use-follow-manager.mdx` | `useFollowManager` | CARRY-OVER |
| `hooks/follows/use-follow-user.mdx` | `useFollowUser` | CARRY-OVER |
| `hooks/follows/use-unfollow-by-follow-id.mdx` | `useUnfollowByFollowId` | CARRY-OVER |
| `hooks/follows/use-unfollow-user-by-user-id.mdx` | `useUnfollowUserByUserId` | CARRY-OVER |
| `hooks/follows/use-fetch-follow-status.mdx` | `useFetchFollowStatus` | CARRY-OVER |
| `hooks/follows/use-fetch-followers.mdx` | `useFetchFollowers` | CARRY-OVER |
| `hooks/follows/use-fetch-followers-by-user-id.mdx` | `useFetchFollowersByUserId` | CARRY-OVER |
| `hooks/follows/use-fetch-followers-count.mdx` | `useFetchFollowersCount` | CARRY-OVER |
| `hooks/follows/use-fetch-followers-count-by-user-id.mdx` | `useFetchFollowersCountByUserId` | CARRY-OVER |
| `hooks/follows/use-fetch-following.mdx` | `useFetchFollowing` | CARRY-OVER |
| `hooks/follows/use-fetch-following-by-user-id.mdx` | `useFetchFollowingByUserId` | CARRY-OVER |
| `hooks/follows/use-fetch-following-count.mdx` | `useFetchFollowingCount` | CARRY-OVER |
| `hooks/follows/use-fetch-following-count-by-user-id.mdx` | `useFetchFollowingCountByUserId` | CARRY-OVER |

### Group: Connections

| File | Hook | Class |
|------|------|-------|
| `hooks/connections/use-connection-manager.mdx` | `useConnectionManager` | CARRY-OVER |
| `hooks/connections/use-request-connection.mdx` | `useRequestConnection` | CARRY-OVER |
| `hooks/connections/use-accept-connection.mdx` | `useAcceptConnection` | CARRY-OVER |
| `hooks/connections/use-decline-connection.mdx` | `useDeclineConnection` | CARRY-OVER |
| `hooks/connections/use-remove-connection.mdx` | `useRemoveConnection` | CARRY-OVER |
| `hooks/connections/use-remove-connection-by-user-id.mdx` | `useRemoveConnectionByUserId` | CARRY-OVER |
| `hooks/connections/use-fetch-connections.mdx` | `useFetchConnections` | CARRY-OVER |
| `hooks/connections/use-fetch-connections-by-user-id.mdx` | `useFetchConnectionsByUserId` | CARRY-OVER |
| `hooks/connections/use-fetch-connections-count.mdx` | `useFetchConnectionsCount` | CARRY-OVER |
| `hooks/connections/use-fetch-connections-count-by-user-id.mdx` | `useFetchConnectionsCountByUserId` | CARRY-OVER |
| `hooks/connections/use-fetch-connection-status.mdx` | `useFetchConnectionStatus` | CARRY-OVER |
| `hooks/connections/use-fetch-sent-pending-connections.mdx` | `useFetchSentPendingConnections` | CARRY-OVER |
| `hooks/connections/use-fetch-received-pending-connections.mdx` | `useFetchReceivedPendingConnections` | CARRY-OVER |

### Group: App Notifications

| File | Hook | Class |
|------|------|-------|
| `hooks/app-notifications/use-app-notifications.mdx` | `useAppNotifications` | UPDATED |

### Group: Reports

| File | Hook | Class |
|------|------|-------|
| `hooks/reports/use-create-report.mdx` | `useCreateReport` | UPDATED |
| `hooks/reports/use-fetch-moderated-reports.mdx` | `useFetchModeratedReports` | NEW |
| `hooks/reports/use-handle-space-entity-report.mdx` | `useHandleSpaceEntityReport` | NEW |
| `hooks/reports/use-handle-space-comment-report.mdx` | `useHandleSpaceCommentReport` | NEW |

### Group: Search

| File | Hook | Class |
|------|------|-------|
| `hooks/search/use-search-content.mdx` | `useSearchContent` | NEW |
| `hooks/search/use-search-users.mdx` | `useSearchUsers` | NEW |
| `hooks/search/use-search-spaces.mdx` | `useSearchSpaces` | NEW |
| `hooks/search/use-ask-content.mdx` | `useAskContent` | NEW |

### Group: Spaces

| File | Hook | Class |
|------|------|-------|
| `hooks/spaces/use-space.mdx` | `useSpace` | NEW |
| `hooks/spaces/use-fetch-space.mdx` | `useFetchSpace` | NEW |
| `hooks/spaces/use-fetch-space-by-short-id.mdx` | `useFetchSpaceByShortId` | NEW |
| `hooks/spaces/use-fetch-space-by-slug.mdx` | `useFetchSpaceBySlug` | NEW |
| `hooks/spaces/use-fetch-many-spaces.mdx` | `useFetchManySpaces` | NEW |
| `hooks/spaces/use-fetch-user-spaces.mdx` | `useFetchUserSpaces` | NEW |
| `hooks/spaces/use-fetch-space-children.mdx` | `useFetchSpaceChildren` | NEW |
| `hooks/spaces/use-fetch-space-breadcrumb.mdx` | `useFetchSpaceBreadcrumb` | NEW |
| `hooks/spaces/use-fetch-space-members.mdx` | `useFetchSpaceMembers` | NEW |
| `hooks/spaces/use-fetch-space-team.mdx` | `useFetchSpaceTeam` | NEW |
| `hooks/spaces/use-check-my-membership.mdx` | `useCheckMyMembership` | NEW |
| `hooks/spaces/use-check-slug-availability.mdx` | `useCheckSlugAvailability` | NEW |
| `hooks/spaces/use-create-space.mdx` | `useCreateSpace` | NEW |
| `hooks/spaces/use-update-space.mdx` | `useUpdateSpace` | NEW |
| `hooks/spaces/use-delete-space.mdx` | `useDeleteSpace` | NEW |
| `hooks/spaces/use-join-space.mdx` | `useJoinSpace` | NEW |
| `hooks/spaces/use-leave-space.mdx` | `useLeaveSpace` | NEW |
| `hooks/spaces/use-approve-member.mdx` | `useApproveMember` | NEW |
| `hooks/spaces/use-decline-member.mdx` | `useDeclineMember` | NEW |
| `hooks/spaces/use-remove-member.mdx` | `useRemoveMember` | NEW |
| `hooks/spaces/use-update-member-role.mdx` | `useUpdateMemberRole` | NEW |
| `hooks/spaces/use-unban-member.mdx` | `useUnbanMember` | NEW |
| `hooks/spaces/use-moderate-space-entity.mdx` | `useModerateSpaceEntity` | NEW |
| `hooks/spaces/use-moderate-space-comment.mdx` | `useModerateSpaceComment` | NEW |
| `hooks/spaces/use-space-mentions.mdx` | `useSpaceMentions` | NEW |
| `hooks/spaces/use-space-permissions.mdx` | `useSpacePermissions` | NEW |
| `hooks/spaces/use-fetch-digest-config.mdx` | `useFetchDigestConfig` | NEW |
| `hooks/spaces/use-update-digest-config.mdx` | `useUpdateDigestConfig` | NEW |

### Group: Spaces — Rules

| File | Hook | Class |
|------|------|-------|
| `hooks/spaces/rules/use-fetch-many-rules.mdx` | `useFetchManyRules` | NEW |
| `hooks/spaces/rules/use-fetch-rule.mdx` | `useFetchRule` | NEW |
| `hooks/spaces/rules/use-create-rule.mdx` | `useCreateRule` | NEW |
| `hooks/spaces/rules/use-update-rule.mdx` | `useUpdateRule` | NEW |
| `hooks/spaces/rules/use-delete-rule.mdx` | `useDeleteRule` | NEW |
| `hooks/spaces/rules/use-reorder-rules.mdx` | `useReorderRules` | NEW |

### Group: Space Lists

| File | Hook | Class |
|------|------|-------|
| `hooks/space-lists/use-space-list.mdx` | `useSpaceList` | NEW |
| `hooks/space-lists/use-space-list-actions.mdx` | `useSpaceListActions` | NEW |

### Group: Storage

| File | Hook | Class |
|------|------|-------|
| `hooks/storage/use-upload-file.mdx` | `useUploadFile` | NEW |
| `hooks/storage/use-upload-image.mdx` | `useUploadImage` | NEW |

### Group: Current User Hooks

| File | Hook | Class |
|------|------|-------|
| `hooks/user/use-user.mdx` | `useUser` — the logged-in user's full state | UPDATED |
| `hooks/user/use-user-actions.mdx` | `useUserActions` — update profile, avatar, banner | UPDATED |

### Group: User Profile Hooks

| File | Hook | Class |
|------|------|-------|
| `hooks/users/use-fetch-user.mdx` | `useFetchUser` | CARRY-OVER |
| `hooks/users/use-fetch-user-by-foreign-id.mdx` | `useFetchUserByForeignId` | CARRY-OVER |
| `hooks/users/use-fetch-user-by-username.mdx` | `useFetchUserByUsername` | NEW |
| `hooks/users/use-fetch-user-suggestions.mdx` | `useFetchUserSuggestions` | CARRY-OVER |
| `hooks/users/use-check-username-availability.mdx` | `useCheckUsernameAvailability` | CARRY-OVER |
| `hooks/users/use-user-mentions.mdx` | `useUserMentions` | UPDATED |

---

## Tab 4: API Reference

### Group: Getting Started

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `api-reference/getting-started.mdx` | Base URL structure (`/:projectId/api/v7/`), projectId scoping, request/response format, pagination, error codes. | UPDATED | `docs/api-reference/getting-started.mdx` |
| `api-reference/authentication.mdx` | Authenticating requests: Bearer token in the Authorization header, access vs refresh tokens, token lifecycle. | UPDATED | `docs/api-reference/authentication.mdx` |

### Group: Auth Endpoints

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/auth/sign-up.mdx` | `POST /auth/sign-up` | UPDATED | `docs/api-reference/auth/sign-up.mdx` |
| `api-reference/auth/sign-in.mdx` | `POST /auth/sign-in` | UPDATED | `docs/api-reference/auth/sign-in.mdx` |
| `api-reference/auth/sign-out.mdx` | `POST /auth/sign-out` | CARRY-OVER | `docs/api-reference/auth/sign-out.mdx` |
| `api-reference/auth/change-password.mdx` | `POST /auth/change-password` | CARRY-OVER | `docs/api-reference/auth/change-password.mdx` |
| `api-reference/auth/request-access-token.mdx` | `POST /auth/request-access-token` | CARRY-OVER | `docs/api-reference/auth/request-new-access-token.mdx` |
| `api-reference/auth/verify-external-user.mdx` | `POST /auth/verify-external-user` | UPDATED | `docs/api-reference/auth/verify-external-user.mdx` |
| `api-reference/auth/request-password-reset.mdx` | `POST /auth/request-password-reset` | NEW | — |
| `api-reference/auth/reset-password.mdx` | `POST /auth/reset-password` | NEW | — |

### Group: OAuth Endpoints

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/oauth/authorize.mdx` | `POST /oauth/authorize` — initiate OAuth flow, returns redirect URL | NEW |
| `api-reference/oauth/link.mdx` | `POST /oauth/link` — link a new provider to the current user | NEW |
| `api-reference/oauth/list-identities.mdx` | `GET /oauth/identities` — list linked OAuth identities | NEW |
| `api-reference/oauth/unlink-identity.mdx` | `DELETE /oauth/identities/:identityId` — unlink an identity | NEW |

### Group: User Endpoints

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/users/fetch-user.mdx` | `GET /users/:userId` | CARRY-OVER | `docs/api-reference/users/fetch-user.mdx` |
| `api-reference/users/fetch-user-by-foreign-id.mdx` | `GET /users/by-foreign-id` | CARRY-OVER | `docs/api-reference/users/fetch-user-by-foreign-id.mdx` |
| `api-reference/users/fetch-user-by-username.mdx` | `GET /users/by-username` | NEW | — |
| `api-reference/users/fetch-user-suggestions.mdx` | `GET /users/suggestions` | CARRY-OVER | `docs/api-reference/users/fetch-user-suggestions.mdx` |
| `api-reference/users/check-username-availability.mdx` | `GET /users/check-username` | CARRY-OVER | `docs/api-reference/users/check-username-availability.mdx` |
| `api-reference/users/update-user.mdx` | `PATCH /users/:userId` — supports avatar/banner upload | UPDATED | `docs/api-reference/users/update-user.mdx` |

### Group: User Follow Operations

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/users/follows/follow-user.mdx` | `POST /users/:userId/follow` | CARRY-OVER |
| `api-reference/users/follows/get-follow-status.mdx` | `GET /users/:userId/follow` | CARRY-OVER |
| `api-reference/users/follows/get-followers.mdx` | `GET /users/:userId/followers` | CARRY-OVER |
| `api-reference/users/follows/get-followers-count.mdx` | `GET /users/:userId/followers-count` | CARRY-OVER |
| `api-reference/users/follows/get-following.mdx` | `GET /users/:userId/following` | CARRY-OVER |
| `api-reference/users/follows/get-following-count.mdx` | `GET /users/:userId/following-count` | CARRY-OVER |
| `api-reference/users/follows/unfollow-user.mdx` | `DELETE /users/:userId/follow` | CARRY-OVER |

### Group: User Connection Operations

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/users/connections/request-connection.mdx` | `POST /users/:userId/connection` | CARRY-OVER |
| `api-reference/users/connections/get-connection-status.mdx` | `GET /users/:userId/connection` | CARRY-OVER |
| `api-reference/users/connections/get-connections.mdx` | `GET /users/:userId/connections` | CARRY-OVER |
| `api-reference/users/connections/get-connections-count.mdx` | `GET /users/:userId/connections-count` | CARRY-OVER |
| `api-reference/users/connections/remove-connection.mdx` | `DELETE /users/:userId/connection` | CARRY-OVER |

### Group: Follow Endpoints

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/follows/fetch-following.mdx` | `GET /follows/following` | CARRY-OVER |
| `api-reference/follows/fetch-followers.mdx` | `GET /follows/followers` | CARRY-OVER |
| `api-reference/follows/fetch-following-count.mdx` | `GET /follows/following-count` | CARRY-OVER |
| `api-reference/follows/fetch-followers-count.mdx` | `GET /follows/followers-count` | CARRY-OVER |
| `api-reference/follows/delete-follow.mdx` | `DELETE /follows/:followId` | CARRY-OVER |

### Group: Connection Endpoints

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/connections/fetch-connections.mdx` | `GET /connections` | CARRY-OVER |
| `api-reference/connections/fetch-connections-count.mdx` | `GET /connections/count` | CARRY-OVER |
| `api-reference/connections/fetch-sent-pending.mdx` | `GET /connections/pending/sent` | CARRY-OVER |
| `api-reference/connections/fetch-received-pending.mdx` | `GET /connections/pending/received` | CARRY-OVER |
| `api-reference/connections/accept-connection.mdx` | `PATCH /connections/:connectionId/accept` | CARRY-OVER |
| `api-reference/connections/decline-connection.mdx` | `PATCH /connections/:connectionId/decline` | CARRY-OVER |
| `api-reference/connections/remove-connection.mdx` | `DELETE /connections/:connectionId` | CARRY-OVER |

### Group: Entity Endpoints

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/entities/create-entity.mdx` | `POST /entities` — supports multipart file upload | UPDATED | `docs/api-reference/entities/create-entity.mdx` |
| `api-reference/entities/fetch-entity.mdx` | `GET /entities/:entityId` | UPDATED | `docs/api-reference/entities/fetch-entity.mdx` |
| `api-reference/entities/fetch-entity-by-foreign-id.mdx` | `GET /entities/by-foreign-id` | CARRY-OVER | `docs/api-reference/entities/fetch-entity-by-foreign-id.mdx` |
| `api-reference/entities/fetch-entity-by-short-id.mdx` | `GET /entities/by-short-id` | CARRY-OVER | `docs/api-reference/entities/fetch-entity-by-short-id.mdx` |
| `api-reference/entities/fetch-many-entities.mdx` | `GET /entities` — paginated with rich filtering | UPDATED | — |
| `api-reference/entities/fetch-drafts.mdx` | `GET /entities/drafts` | NEW | — |
| `api-reference/entities/publish-entity.mdx` | `PATCH /entities/:entityId/publish` | NEW | — |
| `api-reference/entities/fetch-top-comment.mdx` | `GET /entities/:entityId/top-comment` | NEW | — |
| `api-reference/entities/update-entity.mdx` | `PATCH /entities/:entityId` | UPDATED | `docs/api-reference/entities/update-entity.mdx` |
| `api-reference/entities/delete-entity.mdx` | `DELETE /entities/:entityId` | CARRY-OVER | `docs/api-reference/entities/delete-entity.mdx` |
| `api-reference/entities/upvote-entity.mdx` | `PATCH /entities/:entityId/upvote` | CARRY-OVER | `docs/api-reference/entities/upvote-entity.mdx` |
| `api-reference/entities/downvote-entity.mdx` | `PATCH /entities/:entityId/downvote` | CARRY-OVER | `docs/api-reference/entities/downvote-entity.mdx` |
| `api-reference/entities/remove-entity-upvote.mdx` | `PATCH /entities/:entityId/remove-upvote` | CARRY-OVER | `docs/api-reference/entities/remove-entity-upvote.mdx` |
| `api-reference/entities/remove-entity-downvote.mdx` | `PATCH /entities/:entityId/remove-downvote` | CARRY-OVER | `docs/api-reference/entities/remove-entity-downvote.mdx` |
| `api-reference/entities/add-reaction.mdx` | `POST /entities/:entityId/reactions` | NEW | — |
| `api-reference/entities/remove-reaction.mdx` | `DELETE /entities/:entityId/reactions` | NEW | — |
| `api-reference/entities/fetch-reactions.mdx` | `GET /entities/:entityId/reactions` | NEW | — |
| `api-reference/entities/increment-views.mdx` | `PATCH /entities/:entityId/increment-views` | CARRY-OVER | `docs/api-reference/entities/increment-entity-views.mdx` |

### Group: Comment Endpoints

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/comments/create-comment.mdx` | `POST /comments` | UPDATED | `docs/api-reference/comments/create-comment.mdx` |
| `api-reference/comments/fetch-comment.mdx` | `GET /comments/:commentId` | CARRY-OVER | `docs/api-reference/comments/fetch-comment.mdx` |
| `api-reference/comments/fetch-comment-by-foreign-id.mdx` | `GET /comments/by-foreign-id` | CARRY-OVER | `docs/api-reference/comments/fetch-comment-by-foreign-id.mdx` |
| `api-reference/comments/fetch-many-comments.mdx` | `GET /comments` | UPDATED | `docs/api-reference/comments/fetch-many-comments.mdx` |
| `api-reference/comments/update-comment.mdx` | `PATCH /comments/:commentId` | CARRY-OVER | `docs/api-reference/comments/update-comment.mdx` |
| `api-reference/comments/delete-comment.mdx` | `DELETE /comments/:commentId` | CARRY-OVER | `docs/api-reference/comments/delete-comment.mdx` |
| `api-reference/comments/upvote-comment.mdx` | `PATCH /comments/:commentId/upvote` | CARRY-OVER | `docs/api-reference/comments/upvote-comment.mdx` |
| `api-reference/comments/downvote-comment.mdx` | `PATCH /comments/:commentId/downvote` | CARRY-OVER | `docs/api-reference/comments/downvote-comment.mdx` |
| `api-reference/comments/remove-comment-upvote.mdx` | `PATCH /comments/:commentId/remove-upvote` | CARRY-OVER | `docs/api-reference/comments/remove-comment-upvote.mdx` |
| `api-reference/comments/remove-comment-downvote.mdx` | `PATCH /comments/:commentId/remove-downvote` | CARRY-OVER | `docs/api-reference/comments/remove-comment-downvote.mdx` |
| `api-reference/comments/add-reaction.mdx` | `POST /comments/:commentId/reactions` | NEW | — |
| `api-reference/comments/remove-reaction.mdx` | `DELETE /comments/:commentId/reactions` | NEW | — |
| `api-reference/comments/fetch-reactions.mdx` | `GET /comments/:commentId/reactions` | NEW | — |

### Group: Chat — Conversations

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/chat/conversations/list-conversations.mdx` | `GET /chat/conversations` | NEW |
| `api-reference/chat/conversations/create-group.mdx` | `POST /chat/conversations` | NEW |
| `api-reference/chat/conversations/create-direct.mdx` | `POST /chat/conversations/direct` | NEW |
| `api-reference/chat/conversations/unread-count.mdx` | `GET /chat/conversations/unread-count` | NEW |
| `api-reference/chat/conversations/fetch-conversation.mdx` | `GET /chat/conversations/:conversationId` | NEW |
| `api-reference/chat/conversations/update-conversation.mdx` | `PATCH /chat/conversations/:conversationId` | NEW |
| `api-reference/chat/conversations/delete-conversation.mdx` | `DELETE /chat/conversations/:conversationId` | NEW |

### Group: Chat — Members

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/chat/members/list-members.mdx` | `GET /chat/conversations/:id/members` | NEW |
| `api-reference/chat/members/add-member.mdx` | `POST /chat/conversations/:id/members` | NEW |
| `api-reference/chat/members/remove-member.mdx` | `DELETE /chat/conversations/:id/members/:userId` | NEW |
| `api-reference/chat/members/leave-conversation.mdx` | `DELETE /chat/conversations/:id/leave` | NEW |
| `api-reference/chat/members/update-member-role.mdx` | `PATCH /chat/conversations/:id/members/:userId/role` | NEW |

### Group: Chat — Messages

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/chat/messages/list-messages.mdx` | `GET /chat/conversations/:id/messages` | NEW |
| `api-reference/chat/messages/send-message.mdx` | `POST /chat/conversations/:id/messages` — supports file upload | NEW |
| `api-reference/chat/messages/fetch-message.mdx` | `GET /chat/conversations/:id/messages/:messageId` | NEW |
| `api-reference/chat/messages/edit-message.mdx` | `PATCH /chat/conversations/:id/messages/:messageId` | NEW |
| `api-reference/chat/messages/delete-message.mdx` | `DELETE /chat/conversations/:id/messages/:messageId` | NEW |
| `api-reference/chat/messages/toggle-reaction.mdx` | `POST /chat/.../messages/:messageId/reactions` | NEW |
| `api-reference/chat/messages/fetch-reaction-users.mdx` | `GET /chat/.../messages/:messageId/reactions` | NEW |
| `api-reference/chat/messages/mark-as-read.mdx` | `POST /chat/conversations/:id/read` | NEW |
| `api-reference/chat/messages/report-message.mdx` | `POST /chat/.../messages/:messageId/report` | NEW |

### Group: Collection Endpoints

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/collections/fetch-root.mdx` | `GET /collections/root` | NEW |
| `api-reference/collections/is-entity-saved.mdx` | `GET /collections/is-entity-saved` | NEW |
| `api-reference/collections/create-sub-collection.mdx` | `POST /collections/:collectionId/sub-collections` | NEW |
| `api-reference/collections/fetch-sub-collections.mdx` | `GET /collections/:collectionId/sub-collections` | NEW |
| `api-reference/collections/fetch-entities.mdx` | `GET /collections/:collectionId/entities` | NEW |
| `api-reference/collections/add-entity.mdx` | `POST /collections/:collectionId/entities` | NEW |
| `api-reference/collections/remove-entity.mdx` | `DELETE /collections/:collectionId/entities/:entityId` | NEW |
| `api-reference/collections/update-collection.mdx` | `PATCH /collections/:collectionId` | NEW |
| `api-reference/collections/delete-collection.mdx` | `DELETE /collections/:collectionId` | NEW |

### Group: App Notification Endpoints

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/app-notifications/fetch-notifications.mdx` | `GET /app-notifications` | UPDATED | `docs/api-reference/app-notifications/fetch-notifications.mdx` |
| `api-reference/app-notifications/count-unread.mdx` | `GET /app-notifications/count` | UPDATED | `docs/api-reference/app-notifications/count-unread-notification.mdx` |
| `api-reference/app-notifications/mark-as-read.mdx` | `PATCH /app-notifications/:id/mark-as-read` | UPDATED | `docs/api-reference/app-notifications/mark-notification-as-read.mdx` |
| `api-reference/app-notifications/mark-all-as-read.mdx` | `PATCH /app-notifications/mark-all-as-read` | NEW | — |

### Group: Report Endpoints

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/reports/create-report.mdx` | `POST /reports` | UPDATED | `docs/api-reference/moderation/create-report.mdx` |
| `api-reference/reports/fetch-moderated-reports.mdx` | `GET /reports/moderated` | NEW | — |

### Group: Search Endpoints

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/search/search-content.mdx` | `POST /search/content` — semantic search over entities, comments, messages | NEW |
| `api-reference/search/search-users.mdx` | `POST /search/users` | NEW |
| `api-reference/search/search-spaces.mdx` | `POST /search/spaces` | NEW |
| `api-reference/search/ask.mdx` | `POST /search/ask` — AI-powered Q&A over content | NEW |

### Group: Space Endpoints

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/spaces/create-space.mdx` | `POST /spaces` | NEW |
| `api-reference/spaces/fetch-spaces.mdx` | `GET /spaces` | NEW |
| `api-reference/spaces/fetch-space.mdx` | `GET /spaces/:spaceId` | NEW |
| `api-reference/spaces/fetch-space-by-short-id.mdx` | `GET /spaces/by-short-id` | NEW |
| `api-reference/spaces/fetch-space-by-slug.mdx` | `GET /spaces/by-slug` | NEW |
| `api-reference/spaces/fetch-user-spaces.mdx` | `GET /spaces/user-spaces` | NEW |
| `api-reference/spaces/check-slug.mdx` | `GET /spaces/check-slug` | NEW |
| `api-reference/spaces/update-space.mdx` | `PATCH /spaces/:spaceId` | NEW |
| `api-reference/spaces/delete-space.mdx` | `DELETE /spaces/:spaceId` | NEW |
| `api-reference/spaces/fetch-children.mdx` | `GET /spaces/:spaceId/children` | NEW |
| `api-reference/spaces/fetch-breadcrumb.mdx` | `GET /spaces/:spaceId/breadcrumb` | NEW |
| `api-reference/spaces/digest/fetch-digest-config.mdx` | `GET /spaces/:spaceId/digest-config` | NEW |
| `api-reference/spaces/digest/update-digest-config.mdx` | `PATCH /spaces/:spaceId/digest-config` | NEW |

### Group: Space — Membership

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/spaces/membership/join-space.mdx` | `POST /spaces/:spaceId/join` | NEW |
| `api-reference/spaces/membership/leave-space.mdx` | `DELETE /spaces/:spaceId/leave` | NEW |
| `api-reference/spaces/membership/check-my-membership.mdx` | `GET /spaces/:spaceId/membership/me` | NEW |
| `api-reference/spaces/membership/list-members.mdx` | `GET /spaces/:spaceId/members` | NEW |
| `api-reference/spaces/membership/list-team.mdx` | `GET /spaces/:spaceId/team` | NEW |
| `api-reference/spaces/membership/approve-member.mdx` | `PATCH /spaces/:spaceId/members/:memberId/approve` | NEW |
| `api-reference/spaces/membership/decline-member.mdx` | `PATCH /spaces/:spaceId/members/:memberId/decline` | NEW |
| `api-reference/spaces/membership/ban-member.mdx` | `PATCH /spaces/:spaceId/members/:memberId/ban` | NEW |
| `api-reference/spaces/membership/unban-member.mdx` | `PATCH /spaces/:spaceId/members/:memberId/unban` | NEW |
| `api-reference/spaces/membership/update-member-role.mdx` | `PATCH /spaces/:spaceId/members/:memberId/role` | NEW |

### Group: Space — Rules

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/spaces/rules/list-rules.mdx` | `GET /spaces/:spaceId/rules` | NEW |
| `api-reference/spaces/rules/fetch-rule.mdx` | `GET /spaces/:spaceId/rules/:ruleId` | NEW |
| `api-reference/spaces/rules/create-rule.mdx` | `POST /spaces/:spaceId/rules` | NEW |
| `api-reference/spaces/rules/update-rule.mdx` | `PATCH /spaces/:spaceId/rules/:ruleId` | NEW |
| `api-reference/spaces/rules/delete-rule.mdx` | `DELETE /spaces/:spaceId/rules/:ruleId` | NEW |
| `api-reference/spaces/rules/reorder-rules.mdx` | `PATCH /spaces/:spaceId/rules/reorder` | NEW |

### Group: Space — Moderation

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/spaces/moderation/moderate-entity.mdx` | `PATCH /spaces/:spaceId/entities/:entityId/moderation` | NEW |
| `api-reference/spaces/moderation/moderate-comment.mdx` | `PATCH /spaces/:spaceId/comments/:commentId/moderation` | NEW |
| `api-reference/spaces/moderation/moderate-message.mdx` | `PATCH /spaces/:spaceId/chat/messages/:messageId/moderation` | NEW |
| `api-reference/spaces/moderation/handle-entity-report.mdx` | `PATCH /spaces/:spaceId/reports/entity/:reportId` | NEW |
| `api-reference/spaces/moderation/handle-comment-report.mdx` | `PATCH /spaces/:spaceId/reports/comment/:reportId` | NEW |
| `api-reference/spaces/moderation/handle-message-report.mdx` | `PATCH /spaces/:spaceId/chat/reports/:reportId` | NEW |

### Group: Space — Chat

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/spaces/chat/get-space-conversation.mdx` | `GET /spaces/:spaceId/conversation` — get or create the space's chat conversation | NEW |

### Group: Storage Endpoints

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/storage/upload-image.mdx` | `POST /storage/images` — upload + process; creates thumbnail/small/medium/large variants | NEW | — |
| `api-reference/storage/upload-file.mdx` | `POST /storage` — upload a generic file (no processing) | UPDATED | `docs/api-reference/storage/upload-file.mdx` |
| `api-reference/storage/fetch-file.mdx` | `GET /storage/:fileId` — fetch file metadata | NEW | — |
| `api-reference/storage/delete-file.mdx` | `DELETE /storage/:fileId` | NEW | — |

### Group: Utils

| File | Endpoint | Class |
|------|----------|-------|
| `api-reference/utils/get-metadata.mdx` | `GET /utils/get-metadata` — fetch Open Graph metadata for a URL (for link previews) | NEW |

### Group: Crypto

| File | Endpoint | Class | v6 ref |
|------|----------|-------|--------|
| `api-reference/crypto/sign-testing-jwt.mdx` | `GET /crypto/sign-testing-jwt` — generate a signed testing JWT (development only) | CARRY-OVER | `docs/api-reference/crypto/sign-testing-jwt.mdx` |

---

## Tab 5: Data Models

Documents the TypeScript interfaces developers receive (from `monorepo/packages/core/src/interfaces/`), not the raw Sequelize schema. These are the public-facing data shapes returned by hooks and API calls.

| File | Description | Class | v6 ref |
|------|-------------|-------|--------|
| `data-models/user.mdx` | `User`, `AuthUser`, `UserFull` — all fields, field visibility by context, include options (`files`), the `authMethods` array. | UPDATED | `docs/data-models/user.mdx` |
| `data-models/entity.mdx` | `Entity` — all fields, `reactionCounts`, `TopComment`, include options (`space`, `user`, `topComment`, `saved`, `files`). | UPDATED | `docs/data-models/entity.mdx` |
| `data-models/comment.mdx` | `Comment` — all fields, soft-delete behavior (`userDeletedAt`), `GifData`, include options (`user`, `entity`, `space`, `parent`). | UPDATED | `docs/data-models/comment.mdx` |
| `data-models/space.mdx` | `Space`, `SpacePreview`, `SpaceDetailed`, `SpaceMemberPermissions`, `CheckMyMembershipResponse`, `DigestConfig`. | NEW | — |
| `data-models/space-member.mdx` | `SpaceMember`, `SpaceMemberWithUser` — roles, statuses. | NEW | — |
| `data-models/conversation.mdx` | `Conversation`, `ConversationPreview` — `unreadCount`, truncated `lastMessage`. | NEW | — |
| `data-models/conversation-member.mdx` | `ConversationMember` — role, `lastReadAt`, `mutedUntil`, `isActive`. | NEW | — |
| `data-models/chat-message.mdx` | `ChatMessage` — `clientId` for optimistic deduplication, `quotedMessage`, `threadReplyCount`, `userReactions`. | NEW | — |
| `data-models/collection.mdx` | `Collection` — `parentId`, `isRoot`, `entityCount`. | NEW | — |
| `data-models/rule.mdx` | `Rule` — `title`, `description`, `order`. | NEW | — |
| `data-models/reaction.mdx` | `Reaction`, `ReactionCounts`, `ReactionType` — 8 reaction types with reputation impact per type. | NEW | — |
| `data-models/file.mdx` | `File`, `FileImage`, `FileImageVariant` — full file and image variant shapes, proxied URLs. | NEW | — |
| `data-models/follow.mdx` | `Follow` — `followerId`, `followedId`. | CARRY-OVER | `docs/data-models/follow.mdx` |
| `data-models/connection.mdx` | `Connection` — `requesterId`, `receiverId`, status. | CARRY-OVER | `docs/data-models/connection.mdx` |
| `data-models/app-notification.mdx` | `AppNotification` — `type` string, `action`, `metadata` shape. | UPDATED | `docs/data-models/app-notification.mdx` |

---

## Tab 6: Components

CLI component distribution system (`@replyke/cli`). Shadcn-style installable, editable source code. All pages carry over from v6 with light cleanup.

| File | Class | v6 ref |
|------|-------|--------|
| `components/overview.mdx` | CARRY-OVER | `docs/components/overview.mdx` |
| `components/installation/cli-setup.mdx` | CARRY-OVER | `docs/components/installation/cli-setup.mdx` |
| `components/installation/configuration.mdx` | CARRY-OVER | `docs/components/installation/configuration.mdx` |
| `components/installation/quick-start.mdx` | CARRY-OVER | `docs/components/installation/quick-start.mdx` |
| `components/components/overview.mdx` | CARRY-OVER | `docs/components/components/overview.mdx` |
| `components/components/threaded/overview.mdx` | CARRY-OVER | `docs/components/components/threaded/overview.mdx` |
| `components/components/threaded/installation.mdx` | CARRY-OVER | `docs/components/components/threaded/installation.mdx` |
| `components/components/threaded/props-api.mdx` | CARRY-OVER | `docs/components/components/threaded/props-api.mdx` |
| `components/components/threaded/file-structure.mdx` | CARRY-OVER | `docs/components/components/threaded/file-structure.mdx` |
| `components/components/threaded/features.mdx` | CARRY-OVER | `docs/components/components/threaded/features.mdx` |
| `components/components/social/overview.mdx` | CARRY-OVER | `docs/components/components/social/overview.mdx` |
| `components/components/social/installation.mdx` | CARRY-OVER | `docs/components/components/social/installation.mdx` |
| `components/components/social/props-api.mdx` | CARRY-OVER | `docs/components/components/social/props-api.mdx` |
| `components/components/social/file-structure.mdx` | CARRY-OVER | `docs/components/components/social/file-structure.mdx` |
| `components/components/social/features.mdx` | CARRY-OVER | `docs/components/components/social/features.mdx` |
| `components/components/notifications/overview.mdx` | CARRY-OVER | `docs/components/components/notifications/overview.mdx` |
| `components/components/notifications/installation.mdx` | CARRY-OVER | `docs/components/components/notifications/installation.mdx` |
| `components/components/notifications/props-api.mdx` | CARRY-OVER | `docs/components/components/notifications/props-api.mdx` |
| `components/components/notifications/customization.mdx` | CARRY-OVER | `docs/components/components/notifications/customization.mdx` |
| `components/components/notifications/integration-examples.mdx` | CARRY-OVER | `docs/components/components/notifications/integration-examples.mdx` |
| `components/customization/overview.mdx` | CARRY-OVER | `docs/components/customization/overview.mdx` |
| `components/customization/colors-theming.mdx` | CARRY-OVER | `docs/components/customization/colors-theming.mdx` |
| `components/customization/layout-structure.mdx` | CARRY-OVER | `docs/components/customization/layout-structure.mdx` |
| `components/customization/styling-variants.mdx` | CARRY-OVER | `docs/components/customization/styling-variants.mdx` |
| `components/customization/adding-features.mdx` | CARRY-OVER | `docs/components/customization/adding-features.mdx` |
| `components/customization/advanced.mdx` | CARRY-OVER | `docs/components/customization/advanced.mdx` |

---

## Page Count Summary

| Tab | Pages | NEW | UPDATED | CARRY-OVER |
|-----|-------|-----|---------|------------|
| Overview | 6 | 2 | 3 | 1 |
| SDK Reference | 44 | 25 | 11 | 8 |
| Hooks Reference | 100 | 68 | 12 | 20 |
| API Reference | 104 | 66 | 18 | 20 |
| Data Models | 15 | 9 | 4 | 2 |
| Components | 26 | 0 | 0 | 26 |
| **Total** | **~295** | **~170** | **~48** | **~77** |

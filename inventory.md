# Docs v7 — Code Inventory

## Inventory Progress

### server/src/models/core/
- [x] AppNotification
- [x] ChatMessage
- [x] Collection
- [x] CollectionEntity
- [x] Comment
- [x] Connection
- [x] ContentEmbedding
- [x] Conversation
- [x] ConversationMember
- [x] Entity
- [x] File
- [x] Follow
- [x] Image
- [x] MessageReaction
- [x] OAuthProviderConfig
- [x] OAuthState
- [x] Reaction
- [x] Report
- [x] Rule
- [x] Space
- [x] SpaceBan
- [x] SpaceEmbedding
- [x] SpaceMember
- [x] Suspension
- [x] Token
- [x] User
- [x] UserEmbedding
- [x] UserIdentity
- [x] UserReport

### server/src/v7/routers/
- [x] app-notifications
- [x] auth
- [x] chat
- [x] collections
- [x] comments
- [x] connections
- [x] entities
- [x] follows
- [x] oauth
- [x] reports
- [x] search
- [x] spaces
- [x] storage
- [x] users
- [x] utils

### monorepo/packages/core/src/hooks/
- [x] hooks/app-notifications/
- [x] hooks/auth/
- [x] hooks/chat/conversations/
- [x] hooks/chat/messages/
- [x] hooks/chat/ (root-level files)
- [x] hooks/collections/
- [x] hooks/comments/
- [x] hooks/crypto/
- [x] hooks/entities/
- [x] hooks/entity-lists/
- [x] hooks/projects/
- [x] hooks/reactions/
- [x] hooks/relationships/connections/
- [x] hooks/relationships/follows/
- [x] hooks/reports/
- [x] hooks/search/
- [x] hooks/space-lists/
- [x] hooks/spaces/
- [x] hooks/spaces/rules/
- [x] hooks/storage/
- [x] hooks/user/
- [x] hooks/users/
- [x] hooks/utils/

### monorepo/packages/core/src/context/
- [x] chat-context
- [x] comment-section-context
- [x] conversation-context
- [x] entity-context
- [x] message-thread-context
- [x] replyke-context
- [x] replyke-integration-context
- [x] replyke-store-context
- [x] space-context

### monorepo/packages/core/src/interfaces/
- [x] models/AppNotification
- [x] models/Collection
- [x] models/Comment
- [x] models/Connection
- [x] models/Entity
- [x] models/File
- [x] models/Follow
- [x] models/ChatMessage
- [x] models/Conversation
- [x] models/ConversationMember
- [x] models/Image
- [x] models/Mention
- [x] models/Project
- [x] models/Reaction
- [x] models/Rule
- [x] models/Space
- [x] models/SpaceMember
- [x] models/User
- [x] entity-filters/
- [x] top-level (PaginatedResponse, EntityCommentsTree, IAccountStorage, etc.)

---

## Feature Areas

| Feature | Sub-areas | Status |
|---------|-----------|--------|
| **Auth** | Email/password sign-up/in/out, password reset, token refresh, external user verification | Existing + updated |
| **OAuth** | Provider-based sign-in, account linking, identity management | NEW in v7 |
| **Users** | Profile fetch/update, username check, suggestions, avatar/banner upload | Existing + updated |
| **Entities** | Create/read/update/delete, drafts, publish, votes, reactions, views, file uploads | Existing + updated |
| **Comments** | Create/read/update/delete, nested replies, votes, reactions, GIFs, mentions | Existing + updated |
| **Spaces** | Hierarchical community spaces, membership, roles, moderation, rules, digest/newsletter, chat | NEW in v7 |
| **Chat** | Real-time 1:1 and group conversations, messages, reactions, threads, read state, moderation | NEW in v7 |
| **Collections** | User-owned bookmarking/folder system for saved entities | NEW in v7 |
| **Lists** | Simple ordered entity lists (pre-dating Collections) | Existing |
| **Follows** | Unidirectional follow relationships between users | Existing |
| **Connections** | Bidirectional connection requests (friend-style) | Existing |
| **Reports** | User reporting of content (entities, comments, messages) + moderation workflows | Existing + updated |
| **App Notifications** | In-app notification system for users | NEW in v7 |
| **Storage** | File and image upload with processing | NEW in v7 |
| **Search** | Semantic search for content, users, spaces + AI-powered ask endpoint | NEW in v7 |
| **Reactions** | Emoji reactions on entities and comments (like, love, wow, sad, angry, funny) | Existing + updated |

---

## Data Models

### User
**Purpose**: A user account scoped to a project.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Scopes user to a project |
| role | ENUM | `admin`, `moderator`, `visitor` |
| foreignId | STRING | Optional external system ID |
| hash | TEXT | Password hash (null for OAuth-only) |
| salt | STRING | Password salt |
| email | STRING | Unique per project |
| name | STRING | Display name |
| username | STRING | Unique per project |
| avatar | TEXT | Avatar URL (legacy; use avatarFileId) |
| avatarFileId | UUID | FK → Files |
| bannerFileId | UUID | FK → Files |
| bio | STRING(300) | Profile bio |
| birthdate | DATE | Optional |
| location | GEOGRAPHY(POINT) | PostGIS geo-point |
| isVerified | BOOLEAN | Verified status |
| isActive | BOOLEAN | Account active status |
| lastActive | DATE | Last activity timestamp |
| passwordResetToken | STRING(64) | Temp token for password reset |
| passwordResetExpiry | DATE | Expiry of reset token |
| reputation | INTEGER | Reputation score |
| metadata | JSONB | Public custom data (max 512 KB) |
| secureMetadata | JSONB | Private custom data (max 512 KB) |
| createdAt / updatedAt | DATE | Timestamps |

---

### Entity
**Purpose**: A piece of content (post, article, page) — the core unit around which comments, votes, and reactions are attached.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | Author (FK → Users, SET NULL on delete) |
| spaceId | UUID | Optional space assignment (FK → Spaces) |
| shortId | STRING | Short human-readable ID (auto-generated from UUID) |
| foreignId | STRING | External system reference (unique per project) |
| sourceId | STRING | Feed grouping identifier |
| title | TEXT | Max 300 chars |
| content | TEXT | Max 100,000 chars |
| attachments | JSONB[] | Array of attachment objects |
| mentions | JSONB[] | Array of user mentions |
| upvotes | UUID[] | Array of user IDs who upvoted |
| downvotes | UUID[] | Array of user IDs who downvoted |
| reactionCounts | JSONB | Counts per reaction type |
| sharesCount | INTEGER | Share count |
| views | INTEGER | View count |
| location | GEOGRAPHY(POINT) | Geo-location |
| keywords | TEXT[] | Tags/keywords |
| score | FLOAT | Algorithmic relevance score |
| scoreUpdatedAt | DATE | Last score recalculation |
| metadata | JSONB | Custom data (max 1 MB) |
| isDraft | BOOLEAN | Draft state (null or false = published) |
| moderationStatus | ENUM | `approved`, `removed`, or null |
| moderatedAt | DATE | When moderation action was taken |
| moderatedById | UUID | Who moderated (polymorphic) |
| moderatedByType | ENUM | `client` or `user` |
| moderationReason | TEXT | Reason for moderation |
| createdAt / updatedAt / deletedAt | DATE | Timestamps (soft delete) |

---

### Comment
**Purpose**: A comment or reply on an entity.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| entityId | UUID | Parent entity (FK → Entities) |
| spaceId | UUID | Space the entity belongs to |
| parentId | UUID | Parent comment (for nested replies) |
| userId | UUID | Author (FK → Users, SET NULL on delete) |
| foreignId | STRING | External reference |
| content | TEXT | Comment text |
| gif | JSONB | GIF attachment object |
| mentions | JSONB[] | User mentions |
| upvotes | UUID[] | Upvoter user IDs |
| downvotes | UUID[] | Downvoter user IDs |
| reactionCounts | JSONB | Per-reaction counts |
| referencedCommentId | UUID | Quoted/referenced comment |
| attachments | JSONB[] | File attachments |
| metadata | JSONB | Custom data |
| parentDeletedAt | DATE | When parent was deleted |
| userDeletedAt | DATE | When user soft-deleted this comment |
| moderationStatus | ENUM | `approved`, `removed`, or null |
| moderatedAt / moderatedById / moderatedByType / moderationReason | — | Moderation fields |
| createdAt / updatedAt / deletedAt | DATE | Timestamps (soft delete) |

---

### Conversation
**Purpose**: A chat conversation (direct, group, or space-tied).
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| type | ENUM | `direct`, `group`, `space` |
| name | STRING | Display name (group/space only) |
| description | TEXT | Optional description |
| avatarFileId | UUID | FK → Files |
| spaceId | UUID | Space conversation is linked to (space type only) |
| directKey | STRING | Deterministic key for DM deduplication |
| createdById | UUID | Creator user ID |
| lastMessageAt | DATE | Timestamp of last message |
| postingPermission | ENUM | `members` or `admins` |
| metadata | JSONB | Custom data |
| createdAt / updatedAt | DATE | Timestamps |

---

### ChatMessage
**Purpose**: A message in a conversation.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| conversationId | UUID | FK → Conversations |
| spaceId | UUID | Space the conversation belongs to |
| userId | UUID | Author (FK → Users, SET NULL on delete) |
| content | TEXT | Message text |
| gif | JSONB | GIF attachment |
| mentions | JSONB[] | User mentions |
| metadata | JSONB | Custom data |
| parentMessageId | UUID | Parent for thread replies |
| quotedMessageId | UUID | Quoted message reference |
| threadReplyCount | INTEGER | Count of thread replies |
| editedAt | DATE | Last edit timestamp |
| userDeletedAt | DATE | User soft-delete timestamp |
| moderationStatus | ENUM | `approved`, `removed`, or null |
| moderatedAt / moderatedById / moderatedByType / moderationReason | — | Moderation fields |
| createdAt / updatedAt | DATE | Timestamps |

---

### ConversationMember
**Purpose**: Membership record for a user in a conversation.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| conversationId | UUID | FK → Conversations |
| userId | UUID | FK → Users |
| role | ENUM | `admin`, `member`, or null |
| lastReadAt | DATE | Last read timestamp (for unread tracking) |
| mutedUntil | DATE | Mute expiry |
| isActive | BOOLEAN | Whether user is active in conversation |
| leftAt | DATE | When user left |
| createdAt / updatedAt | DATE | Timestamps |

---

### MessageReaction
**Purpose**: An emoji reaction on a chat message.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| conversationId | UUID | FK → Conversations |
| messageId | UUID | FK → ChatMessages |
| userId | UUID | FK → Users |
| emoji | STRING | The emoji character/shortcode |
| createdAt | DATE | Timestamp |

---

### Space
**Purpose**: A community space (subreddit/forum-style), supports hierarchy, membership, moderation, and digest.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | Creator |
| shortId | STRING | Short human-readable ID |
| name | STRING | Display name |
| slug | STRING | URL slug (unique per project) |
| description | TEXT | Description |
| avatarFileId | UUID | FK → Files |
| bannerFileId | UUID | FK → Files |
| readingPermission | ENUM | `anyone`, `members` |
| postingPermission | ENUM | `anyone`, `members`, `admins` |
| requireJoinApproval | BOOLEAN | Whether membership requires approval |
| metadata | JSONB | Custom data |
| parentSpaceId | UUID | Parent space (for hierarchy) |
| depth | INTEGER | Nesting depth |
| digestEnabled | BOOLEAN | Whether digest/newsletter is enabled |
| digestWebhookUrl | STRING | Webhook URL for digest delivery |
| digestWebhookSecret | STRING | Webhook HMAC secret |
| digestScheduleHour | INTEGER | Hour of day to send digest |
| digestTimezone | STRING | Digest timezone |
| createdAt / updatedAt | DATE | Timestamps |

---

### SpaceMember
**Purpose**: Membership record for a user in a space.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| spaceId | UUID | FK → Spaces |
| userId | UUID | FK → Users |
| role | ENUM | `admin`, `moderator`, `member` |
| status | ENUM | `pending`, `active`, `banned`, `rejected` |
| joinedAt | DATE | When member joined |
| createdAt | DATE | Timestamp |

---

### SpaceBan
**Purpose**: A ban record for a user from a space.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| spaceId | UUID | FK → Spaces |
| userId | UUID | Banned user |
| bannedBy | UUID | Moderator who issued ban |
| reason | STRING | Ban reason |
| reportId | UUID | Associated report (optional) |
| createdAt / updatedAt | DATE | Timestamps |

---

### Rule
**Purpose**: A community rule for a space.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| spaceId | UUID | FK → Spaces |
| title | STRING | Rule title |
| description | TEXT | Optional description |
| order | INTEGER | Display order |
| lastApprovedBy | UUID | Who last approved the rule |
| createdAt / updatedAt | DATE | Timestamps |

---

### Collection
**Purpose**: A user's personal bookmark folder for saved entities (hierarchical).
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | Owner |
| parentId | UUID | Parent collection (null = root) |
| name | STRING | Collection name |
| isRoot | BOOLEAN | Whether this is the root collection |
| entityCount | INTEGER | Cached count of saved entities |
| createdAt / updatedAt | DATE | Timestamps |

---

### CollectionEntity
**Purpose**: Junction table linking an entity to a collection.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | Owner |
| collectionId | UUID | FK → Collections |
| entityId | UUID | FK → Entities |
| createdAt / updatedAt | DATE | Timestamps |

---

### OAuthProviderConfig
**Purpose**: OAuth provider configuration (Google, GitHub, etc.) per project.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| provider | STRING | Provider name (e.g. `google`, `github`) |
| clientId | STRING | OAuth app client ID |
| clientSecretEncrypted | JSONB | Encrypted client secret |
| redirectUri | STRING | Registered redirect URI |
| allowedRedirectUris | STRING[] | Whitelisted post-auth redirect URIs |
| scopes | STRING[] | Requested OAuth scopes |
| providerMetadata | JSONB | Provider-specific config |
| isEnabled | BOOLEAN | Whether this provider is active |
| createdAt / updatedAt | DATE | Timestamps |

---

### OAuthState
**Purpose**: Temporary PKCE/state storage for an in-flight OAuth flow.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| provider | STRING | Provider name |
| redirectAfterAuth | STRING | Where to redirect after auth |
| codeVerifier | STRING | PKCE code verifier |
| linkToUserId | UUID | If linking to existing user |
| expiresAt | DATE | State expiry |
| createdAt | DATE | Timestamp |

---

### UserIdentity
**Purpose**: A linked OAuth identity for a user (one user can have multiple identities).
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | FK → Users |
| provider | STRING | Provider name |
| providerAccountId | STRING | ID from the provider |
| email | STRING | Email from provider |
| name | STRING | Name from provider |
| avatar | STRING | Avatar URL from provider |
| isVerified | BOOLEAN | Provider-verified email |
| rawProfile | JSONB | Full raw profile from provider |
| createdAt / updatedAt | DATE | Timestamps |

---

### AppNotification
**Purpose**: An in-app notification for a user.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | Recipient (FK → Users) |
| type | STRING | Notification type |
| isRead | BOOLEAN | Read status |
| action | STRING | Action identifier |
| metadata | JSONB | Contextual data |
| createdAt / updatedAt | DATE | Timestamps |

---

### Report
**Purpose**: A user report on a piece of content (entity, comment, or message).
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| spaceId | UUID | Space the content belongs to |
| targetId | UUID | ID of reported content (polymorphic) |
| targetType | ENUM | `comment`, `entity`, `message` |
| reporterCount | INTEGER | Number of users who reported |
| status | ENUM | `pending`, `reviewed`, etc. |
| actionTaken | STRING | Moderation action taken |
| createdAt / updatedAt | DATE | Timestamps |

---

### UserReport
**Purpose**: Individual report submission linking a user to a Report aggregate.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| reportId | UUID | FK → Reports |
| userId | UUID | Reporting user |
| reason | STRING | Report reason |
| details | TEXT | Optional details |
| createdAt | DATE | Timestamp |

---

### Reaction
**Purpose**: An emoji reaction by a user on an entity or comment.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| targetType | ENUM | `entity` or `comment` |
| targetId | UUID | Target entity or comment ID |
| userId | UUID | Reacting user |
| reactionType | ENUM | `like`, `love`, `wow`, `sad`, `angry`, `funny` |
| createdAt / updatedAt | DATE | Timestamps |

---

### Follow
**Purpose**: A unidirectional follow relationship between two users.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| followerId | UUID | User who follows |
| followedId | UUID | User being followed |
| createdAt | DATE | Timestamp |

---

### Connection
**Purpose**: A bidirectional connection request between two users.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| requesterId | UUID | User who sent the request |
| receiverId | UUID | User who received the request |
| status | ENUM | `pending`, `accepted`, `declined` |
| createdAt | DATE | Timestamp |

---

### File
**Purpose**: A stored file (image, video, document, etc.) attached to content.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | Uploader |
| entityId | UUID | Associated entity (optional) |
| commentId | UUID | Associated comment (optional) |
| spaceId | UUID | Associated space (optional) |
| chatMessageId | UUID | Associated chat message (optional) |
| type | ENUM | `image`, `video`, `document`, `other` |
| originalPath | STRING | Storage path |
| originalSize | INTEGER | File size in bytes |
| originalMimeType | STRING | MIME type |
| position | INTEGER | Display position |
| metadata | JSONB | Custom metadata |
| createdAt / updatedAt | DATE | Timestamps |

---

### Image
**Purpose**: Processed image variants associated with a File.
| Field | Type | Notes |
|-------|------|-------|
| fileId | UUID | FK → Files (primary key) |
| originalWidth / originalHeight | INTEGER | Original dimensions |
| variants | JSONB | Map of named variants (thumbnail, medium, etc.) |
| processingStatus | ENUM | `completed`, `failed` |
| processingError | STRING | Error message if failed |
| format | STRING | Output format |
| quality | INTEGER | Compression quality |
| exifStripped | BOOLEAN | Whether EXIF data was removed |
| createdAt / updatedAt | DATE | Timestamps |

---

### Token
**Purpose**: Refresh token record for a user session.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | FK → Users |
| refreshToken | STRING | The refresh token value |
| familyId | UUID | Token rotation family (for reuse detection) |
| revokedAt | DATE | Revocation timestamp |
| createdAt / updatedAt | DATE | Timestamps |

---

### Suspension
**Purpose**: A platform-level account suspension for a user.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| userId | UUID | Suspended user |
| reason | STRING | Suspension reason |
| startDate | DATE | When suspension begins |
| endDate | DATE | When suspension ends (null = permanent) |
| createdBy | UUID | Admin who issued suspension |
| createdAt / updatedAt | DATE | Timestamps |

---

### ContentEmbedding
**Purpose**: Vector embedding for semantic search over entities, comments, and messages.
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| projectId | UUID | Project scope |
| spaceId | UUID | Optional space scope |
| conversationId | UUID | Optional conversation scope |
| userId | UUID | Optional user scope |
| sourceType | ENUM | `entity`, `comment`, `message` |
| sourceId | UUID | ID of the source content |
| chunkIndex | INTEGER | Chunk index for long content |
| content | TEXT | The chunk text |
| embedding | FLOAT[] | Vector embedding |
| embeddingModel | STRING | Model used |
| status | ENUM | `pending`, `embedded`, `failed` |
| retryCount | INTEGER | Retry counter |
| lastAttemptAt | DATE | Last embedding attempt |
| createdAt / updatedAt | DATE | Timestamps |

---

### SpaceEmbedding
**Purpose**: Vector embedding for a space's description (for semantic space search).
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| spaceId | UUID | FK → Spaces |
| projectId | UUID | Project scope |
| embeddingText | TEXT | Text used for embedding |
| embedding | FLOAT[] | Vector embedding |
| embeddingModel | STRING | Model used |
| status | ENUM | `pending`, `embedded`, `failed` |
| retryCount / lastAttemptAt | — | Retry tracking |
| createdAt / updatedAt | DATE | Timestamps |

---

### UserEmbedding
**Purpose**: Vector embedding for a user's profile (for semantic user search).
| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| userId | UUID | FK → Users |
| projectId | UUID | Project scope |
| profileText | TEXT | Text used for embedding |
| embedding | FLOAT[] | Vector embedding |
| embeddingModel | STRING | Model used |
| status | ENUM | `pending`, `embedded`, `failed` |
| retryCount / lastAttemptAt | — | Retry tracking |
| createdAt / updatedAt | DATE | Timestamps |

---

## API Endpoints

All routes are prefixed with `/:projectId/v7/`.

### App Notifications (`/app-notifications`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/app-notifications` | Required | Fetch paginated notifications for the logged-in user |
| GET | `/app-notifications/count` | Required | Count unread notifications |
| PATCH | `/app-notifications/:notificationId/mark-as-read` | Required | Mark a single notification as read |
| PATCH | `/app-notifications/mark-all-as-read` | Required | Mark all notifications as read |

---

### Auth (`/auth`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/auth/sign-up` | None | Create a new user with email/password |
| POST | `/auth/sign-in` | None | Sign in with email/password, returns tokens |
| POST | `/auth/sign-out` | None | Revoke refresh token |
| POST | `/auth/change-password` | None | Change password (requires current password) |
| POST | `/auth/request-access-token` | None | Get new access token using refresh token |
| POST | `/auth/verify-external-user` | None | Verify/create a user from external auth system (signed JWT) |
| POST | `/auth/request-password-reset` | None | Send password reset email |
| POST | `/auth/reset-password` | None | Reset password with token |
| GET | `/auth/reset-password` | None | Render password reset page (server-side HTML) |

---

### OAuth (`/oauth`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/oauth/authorize` | None | Initiate OAuth flow (sign-in or sign-up); returns redirect URL |
| POST | `/oauth/link` | Required | Initiate OAuth to link a new provider to current user |
| GET | `/oauth/identities` | Required | List all OAuth identities linked to current user |
| DELETE | `/oauth/identities/:identityId` | Required | Unlink an OAuth identity from the current user |

Note: The OAuth callback is handled at `GET /v7/oauth/callback` (separate route, not project-scoped).

---

### Chat (`/chat/conversations`)
#### Conversations
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/chat/conversations` | Required | List the caller's conversations (paginated) |
| POST | `/chat/conversations` | Required | Create a group conversation |
| POST | `/chat/conversations/direct` | Required | Get or create a direct (1:1) conversation |
| GET | `/chat/conversations/unread-count` | Required | Get total unread messages + unread conversation count |
| GET | `/chat/conversations/:conversationId` | Required | Fetch a single conversation |
| PATCH | `/chat/conversations/:conversationId` | Required | Update conversation metadata (name, description, etc.) |
| DELETE | `/chat/conversations/:conversationId` | Required | Delete a conversation |

#### Members
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/chat/conversations/:conversationId/members` | Required | List members of a conversation |
| POST | `/chat/conversations/:conversationId/members` | Required | Add a member (group admin only) |
| DELETE | `/chat/conversations/:conversationId/members/:userId` | Required | Remove a member (group admin only) |
| DELETE | `/chat/conversations/:conversationId/leave` | Required | Leave a conversation |
| PATCH | `/chat/conversations/:conversationId/members/:userId/role` | Required | Change a member's role (admin only) |

#### Messages
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/chat/conversations/:conversationId/messages` | Required | List messages (paginated, supports file attachments) |
| POST | `/chat/conversations/:conversationId/messages` | Required | Send a message (supports file uploads via multipart) |
| GET | `/chat/conversations/:conversationId/messages/:messageId` | Required | Fetch a single message |
| PATCH | `/chat/conversations/:conversationId/messages/:messageId` | Required | Edit a message |
| DELETE | `/chat/conversations/:conversationId/messages/:messageId` | Required | Delete or moderate a message |

#### Reactions
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/chat/conversations/:conversationId/messages/:messageId/reactions` | Required | Toggle a reaction on a message |
| GET | `/chat/conversations/:conversationId/messages/:messageId/reactions` | Required | List users who reacted with a specific emoji |

#### Read State
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/chat/conversations/:conversationId/read` | Required | Mark messages as read up to a given timestamp |

#### Reports
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/chat/conversations/:conversationId/messages/:messageId/report` | Required | Report a message |

---

### Collections (`/collections`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/collections/root` | Required | Fetch the user's root collection |
| GET | `/collections/is-entity-saved` | Required | Check if an entity is saved in any of the user's collections |
| POST | `/collections/:collectionId/sub-collections` | Required | Create a sub-collection |
| GET | `/collections/:collectionId/sub-collections` | Required | Fetch sub-collections of a collection |
| GET | `/collections/:collectionId/entities` | Required | Fetch paginated entities in a collection |
| POST | `/collections/:collectionId/entities` | Required | Add an entity to a collection |
| DELETE | `/collections/:collectionId/entities/:entityId` | Required | Remove an entity from a collection |
| PATCH | `/collections/:collectionId` | Required | Update a collection (name) |
| DELETE | `/collections/:collectionId` | Required | Delete a collection |

---

### Comments (`/comments`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/comments` | Required | Create a comment or reply |
| GET | `/comments` | Optional | Fetch paginated comments (filterable by entity, parent, etc.) |
| GET | `/comments/by-foreign-id` | Optional | Fetch a comment by foreign ID |
| GET | `/comments/:commentId` | Optional | Fetch a single comment |
| PATCH | `/comments/:commentId` | Required | Update comment content |
| PATCH | `/comments/:commentId/upvote` | Required | Upvote a comment |
| PATCH | `/comments/:commentId/remove-upvote` | Required | Remove upvote from comment |
| PATCH | `/comments/:commentId/downvote` | Required | Downvote a comment |
| PATCH | `/comments/:commentId/remove-downvote` | Required | Remove downvote from comment |
| POST | `/comments/:commentId/reactions` | Required | Add or update a reaction on a comment |
| DELETE | `/comments/:commentId/reactions` | Required | Remove a reaction from a comment |
| GET | `/comments/:commentId/reactions` | None | Fetch reactions for a comment |
| GET | `/comments/:commentId/reactions/me` | Required | Get current user's reaction on a comment |
| DELETE | `/comments/:commentId` | Required | Delete a comment and its replies |

---

### Connections (`/connections`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/connections` | Required | Fetch established connections |
| GET | `/connections/count` | Required | Count connections |
| GET | `/connections/pending/sent` | Required | Fetch sent pending connection requests |
| GET | `/connections/pending/received` | Required | Fetch received pending connection requests |
| PATCH | `/connections/:connectionId/accept` | Required | Accept a connection request |
| PATCH | `/connections/:connectionId/decline` | Required | Decline a connection request |
| DELETE | `/connections/:connectionId` | Required | Remove/withdraw a connection |

---

### Entities (`/entities`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/entities` | Optional | Create an entity (supports file uploads) |
| GET | `/entities` | Optional | Fetch paginated entities (rich filtering) |
| GET | `/entities/by-foreign-id` | Optional | Fetch entity by foreign ID (or create blank) |
| GET | `/entities/by-short-id` | Optional | Fetch entity by short ID |
| GET | `/entities/drafts` | Required | Fetch the current user's draft entities |
| GET | `/entities/:entityId` | Optional | Fetch a single entity |
| GET | `/entities/:entityId/top-comment` | None | Fetch the top-scoring comment on an entity |
| PATCH | `/entities/:entityId/publish` | Required | Publish a draft entity |
| PATCH | `/entities/:entityId/upvote` | Required | Upvote an entity |
| PATCH | `/entities/:entityId/remove-upvote` | Required | Remove upvote from entity |
| PATCH | `/entities/:entityId/downvote` | Required | Downvote an entity |
| PATCH | `/entities/:entityId/remove-downvote` | Required | Remove downvote from entity |
| POST | `/entities/:entityId/reactions` | Required | Add or update a reaction on an entity |
| DELETE | `/entities/:entityId/reactions` | Required | Remove a reaction from an entity |
| GET | `/entities/:entityId/reactions` | None | Fetch reactions on an entity |
| GET | `/entities/:entityId/reactions/me` | Required | Get current user's reaction on an entity |
| PATCH | `/entities/:entityId/increment-views` | None | Increment the view count |
| PATCH | `/entities/:entityId` | Required | Update an entity |
| DELETE | `/entities/:entityId` | Required | Delete an entity |

---

### Follows (`/follows`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/follows/following` | Required | Get accounts the logged-in user follows |
| GET | `/follows/followers` | Required | Get accounts following the logged-in user |
| GET | `/follows/following-count` | Required | Count accounts the user follows |
| GET | `/follows/followers-count` | Required | Count accounts following the user |
| DELETE | `/follows/:followId` | Required | Unfollow by follow ID |

Also exposed under Users (see below):
| POST | `/users/:userId/follow` | Required | Follow a user |
| GET | `/users/:userId/follow` | Required | Get follow status with a user |
| GET | `/users/:userId/followers` | None | Get followers of a user |
| GET | `/users/:userId/followers-count` | None | Count followers of a user |
| GET | `/users/:userId/following` | None | Get accounts a user follows |
| GET | `/users/:userId/following-count` | None | Count accounts a user follows |
| DELETE | `/users/:userId/follow` | Required | Unfollow a user |

---

### Reports (`/reports`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/reports` | Required | Create a report on content |
| GET | `/reports/moderated` | Required | Fetch reports from spaces the user moderates |

---

### Search (`/search`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/search/content` | Optional | Semantic search over entities, comments, messages |
| POST | `/search/users` | Optional | Semantic search over users |
| POST | `/search/spaces` | Optional | Semantic search over spaces |
| POST | `/search/ask` | Optional | AI-powered Q&A over content (calls embedding + LLM) |

---

### Spaces (`/spaces`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/spaces` | Required | Create a new space |
| GET | `/spaces` | Optional | Fetch spaces with filtering and pagination |
| GET | `/spaces/check-slug` | None | Check if a slug is available |
| GET | `/spaces/user-spaces` | Required | Fetch spaces the current user is a member of |
| GET | `/spaces/by-short-id` | Optional | Fetch a space by short ID |
| GET | `/spaces/by-slug` | Optional | Fetch a space by slug |
| GET | `/spaces/:spaceId` | Optional | Fetch a space by ID |
| PATCH | `/spaces/:spaceId` | Required (admin) | Update space metadata |
| DELETE | `/spaces/:spaceId` | Required | Delete a space |
| GET | `/spaces/:spaceId/digest-config` | Required (admin) | Fetch digest/newsletter config |
| PATCH | `/spaces/:spaceId/digest-config` | Required (admin) | Update digest/newsletter config |
| POST | `/spaces/:spaceId/join` | Required | Join a space |
| GET | `/spaces/:spaceId/members` | Optional | List space members |
| GET | `/spaces/:spaceId/team` | Optional | List space admins and moderators |
| GET | `/spaces/:spaceId/membership/me` | Optional | Check current user's membership status |
| PATCH | `/spaces/:spaceId/members/:memberId/role` | Required (admin) | Update a member's role |
| PATCH | `/spaces/:spaceId/members/:memberId/approve` | Required (mod) | Approve a pending membership |
| PATCH | `/spaces/:spaceId/members/:memberId/decline` | Required (mod) | Decline a pending membership |
| PATCH | `/spaces/:spaceId/members/:memberId/ban` | Required (mod) | Ban a member |
| PATCH | `/spaces/:spaceId/members/:memberId/unban` | Required (mod) | Unban a member |
| DELETE | `/spaces/:spaceId/leave` | Required (member) | Leave a space |
| GET | `/spaces/:spaceId/children` | None | Fetch child spaces |
| GET | `/spaces/:spaceId/breadcrumb` | None | Fetch ancestor breadcrumb |
| GET | `/spaces/:spaceId/rules` | None | List space rules |
| GET | `/spaces/:spaceId/rules/:ruleId` | None | Fetch a single rule |
| POST | `/spaces/:spaceId/rules` | Required (admin) | Create a rule |
| PATCH | `/spaces/:spaceId/rules/reorder` | Required (admin) | Reorder rules |
| PATCH | `/spaces/:spaceId/rules/:ruleId` | Required (admin) | Update a rule |
| DELETE | `/spaces/:spaceId/rules/:ruleId` | Required (admin) | Delete a rule |
| PATCH | `/spaces/:spaceId/reports/entity/:reportId` | Required (mod) | Handle an entity report |
| PATCH | `/spaces/:spaceId/reports/comment/:reportId` | Required (mod) | Handle a comment report |
| PATCH | `/spaces/:spaceId/entities/:entityId/moderation` | Required (mod) | Moderate an entity (approve/remove) |
| PATCH | `/spaces/:spaceId/comments/:commentId/moderation` | Required (mod) | Moderate a comment (approve/remove) |
| GET | `/spaces/:spaceId/conversation` | Required | Get or create the space's chat conversation |
| PATCH | `/spaces/:spaceId/chat/messages/:messageId/moderation` | Required (mod) | Moderate a chat message |
| PATCH | `/spaces/:spaceId/chat/reports/:reportId` | Required (mod) | Handle a space chat report |

---

### Utils (`/utils`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/utils/get-metadata` | Required | Fetch Open Graph / metadata for a given URL |

---

### Storage (`/storage`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/storage/images` | Required | Upload and process an image (creates variants) |
| POST | `/storage` | Required | Upload a generic file (no processing) |
| GET | `/storage/:fileId` | Required | Fetch file metadata by ID |
| DELETE | `/storage/:fileId` | Required | Delete a file |

---

### Users (`/users`)
| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/users/suggestions` | None | Fetch user mention suggestions |
| GET | `/users/check-username` | None | Check if a username is available |
| GET | `/users/by-foreign-id` | None | Fetch a user by foreign ID |
| GET | `/users/by-username` | None | Fetch a user by username |
| GET | `/users/:userId` | None | Fetch a user by ID |
| PATCH | `/users/:userId` | Required | Update user profile (supports avatar/banner upload) |
| POST | `/users/:userId/follow` | Required | Follow a user |
| GET | `/users/:userId/follow` | Required | Get follow status |
| GET | `/users/:userId/followers` | None | List user's followers |
| GET | `/users/:userId/followers-count` | None | Count user's followers |
| GET | `/users/:userId/following` | None | List accounts a user follows |
| GET | `/users/:userId/following-count` | None | Count accounts a user follows |
| DELETE | `/users/:userId/follow` | Required | Unfollow a user |
| POST | `/users/:userId/connection` | Required | Send a connection request |
| GET | `/users/:userId/connection` | Required | Get connection status |
| GET | `/users/:userId/connections` | None | List a user's connections |
| GET | `/users/:userId/connections-count` | None | Count a user's connections |
| DELETE | `/users/:userId/connection` | Required | Withdraw/decline a connection |

---

## SDK Hooks

All hooks live in `monorepo/packages/core/src/hooks/`.

### Auth Hooks (`hooks/auth/`)
| Hook | Purpose |
|------|---------|
| `useAuth` | Access current user auth state (user, tokens, sign-in, sign-up, sign-out, etc.) |
| `useAccounts` | Multi-account state — list of linked accounts |
| `useAccountSync` | Internal — syncs account manager state into Redux on mount |
| `useAddAccount` | Add/link a new account to the account manager |
| `useRemoveAccount` | Remove an account from the account manager |
| `useSwitchAccount` | Switch the active account |
| `useSignOutAll` | Sign out from all accounts |
| `useOAuthIdentities` | Fetch, and unlink OAuth identities for the current user |
| `useRequestPasswordReset` | Trigger a password reset email |

---

### Chat Hooks — Conversations (`hooks/chat/conversations/`)
| Hook | Purpose |
|------|---------|
| `useConversations` | List and paginate conversations for the current user |
| `useConversation` | Fetch and manage a single conversation's data |
| `useCreateDirectConversation` | Get or create a DM conversation with another user |
| `useConversationMembers` | List and manage members of a conversation |
| `useSpaceConversation` | Get or create the chat conversation for a space |

---

### Chat Hooks — Messages (`hooks/chat/messages/`)
| Hook | Purpose |
|------|---------|
| `useChatMessages` | Fetch paginated messages in a conversation |
| `useSendMessage` | Send a new message to a conversation |
| `useEditMessage` | Edit an existing message |
| `useDeleteMessage` | Delete a message |
| `useToggleReaction` | Toggle an emoji reaction on a message |
| `useMessageThread` | Fetch and manage a message thread (replies to a message) |

---

### Chat Hooks — Root (`hooks/chat/`)
| Hook | Purpose |
|------|---------|
| `useChatSocket` | Manage the socket.io connection for real-time chat |
| `useConversationData` | Internal data hook used by ConversationProvider |
| `useMarkConversationAsRead` | Mark messages in a conversation as read |
| `useReportMessage` | Report a message |
| `useTotalUnreadCount` | Get the total unread message count across all conversations |
| `useTypingIndicator` | Emit and receive typing indicator events |
| `useUnreadConversationCount` | Get count of conversations with unread messages |

---

### Collections Hooks (`hooks/collections/`)
| Hook | Purpose |
|------|---------|
| `useCollections` | Fetch and navigate collection hierarchy for current user |
| `useCollectionsActions` | Create, update, delete collections; add/remove entities |
| `useCollectionEntitiesWrapper` | Fetch paginated entities in a collection |
| `useIsEntitySaved` | Check if a specific entity is saved in any collection |

---

### Comments Hooks (`hooks/comments/`)
| Hook | Purpose |
|------|---------|
| `useCommentSection` | High-level hook wrapping the full comment section state |
| `useCommentSectionData` | Internal data hook used by CommentSectionProvider |
| `useEntityComments` | Fetch top-level comments for an entity |
| `useReplies` | Fetch replies to a comment |
| `useCreateComment` | Create a new comment or reply |
| `useUpdateComment` | Update an existing comment |
| `useDeleteComment` | Delete a comment |
| `useFetchComment` | Fetch a single comment by ID |
| `useFetchCommentByForeignId` | Fetch a comment by foreign ID |
| `useFetchManyComments` | Fetch paginated comments with filters |
| `useFetchManyCommentsWrapper` | Wrapper for paginated comment fetching |

---

### Entities Hooks (`hooks/entities/`)
| Hook | Purpose |
|------|---------|
| `useEntity` | High-level hook for full entity state (data + actions) |
| `useEntityData` | Internal data hook used by EntityProvider |
| `useFetchEntity` | Fetch a single entity by ID |
| `useFetchEntityByForeignId` | Fetch entity by foreign ID |
| `useFetchEntityByShortId` | Fetch entity by short ID |
| `useFetchManyEntities` | Fetch paginated entities with rich filtering |
| `useFetchManyEntitiesWrapper` | Wrapper for paginated entity fetching |
| `useCreateEntity` | Create a new entity |
| `useUpdateEntity` | Update an entity |
| `useDeleteEntity` | Delete an entity |
| `useFetchDrafts` | Fetch current user's draft entities |
| `usePublishDraft` | Publish a draft entity |
| `useIncrementEntityViews` | Increment view count |

---

### Entity List Hooks (`hooks/entity-lists/`)
| Hook | Purpose |
|------|---------|
| `useEntityList` | Manage a paginated entity list with filtering and sorting state |
| `useEntityListActions` | Actions for entity list (refresh, load more) |
| `useInfusedData` | Merge entity list results with local state overrides |

---

### Projects Hooks (`hooks/projects/`)
| Hook | Purpose |
|------|---------|
| `useProject` | Access current project context (projectId, project config) |
| `useProjectData` | Internal hook for fetching project configuration |

---

### Reactions Hooks (`hooks/reactions/`)
| Hook | Purpose |
|------|---------|
| `useAddReaction` | Add a reaction to an entity or comment |
| `useRemoveReaction` | Remove a reaction from an entity or comment |
| `useReactionToggle` | Toggle a reaction (add if absent, remove if present) |
| `useFetchEntityReactions` | Fetch reactions on an entity |
| `useFetchEntityReactionsWrapper` | Paginated wrapper for entity reactions |
| `useFetchCommentReactions` | Fetch reactions on a comment |
| `useFetchCommentReactionsWrapper` | Paginated wrapper for comment reactions |

---

### Relationships — Connections Hooks (`hooks/relationships/connections/`)
| Hook | Purpose |
|------|---------|
| `useConnectionManager` | Manage connection state for a user pair |
| `useRequestConnection` | Send a connection request |
| `useAcceptConnection` | Accept a pending connection |
| `useDeclineConnection` | Decline a pending connection |
| `useRemoveConnection` | Remove an established connection by connection ID |
| `useRemoveConnectionByUserId` | Remove a connection by user ID |
| `useFetchConnections` | Fetch current user's established connections |
| `useFetchConnectionsByUserId` | Fetch connections for a specific user |
| `useFetchConnectionsCount` | Count current user's connections |
| `useFetchConnectionsCountByUserId` | Count connections for a specific user |
| `useFetchConnectionStatus` | Get connection status between current user and another |
| `useFetchSentPendingConnections` | Fetch sent pending connection requests |
| `useFetchReceivedPendingConnections` | Fetch received pending connection requests |

---

### Relationships — Follows Hooks (`hooks/relationships/follows/`)
| Hook | Purpose |
|------|---------|
| `useFollowManager` | Follow/unfollow state manager for a user pair |
| `useFollowUser` | Follow a user |
| `useUnfollowByFollowId` | Unfollow by follow record ID |
| `useUnfollowUserByUserId` | Unfollow by user ID |
| `useFetchFollowStatus` | Get follow status between current user and another |
| `useFetchFollowers` | Fetch current user's followers |
| `useFetchFollowersByUserId` | Fetch followers for a specific user |
| `useFetchFollowersCount` | Count current user's followers |
| `useFetchFollowersCountByUserId` | Count followers for a specific user |
| `useFetchFollowing` | Fetch accounts current user follows |
| `useFetchFollowingByUserId` | Fetch accounts a specific user follows |
| `useFetchFollowingCount` | Count accounts current user follows |
| `useFetchFollowingCountByUserId` | Count accounts a specific user follows |

---

### Reports Hooks (`hooks/reports/`)
| Hook | Purpose |
|------|---------|
| `useCreateReport` | Submit a content report |
| `useFetchModeratedReports` | Fetch reports from spaces the user can moderate |
| `useHandleSpaceEntityReport` | Handle (resolve) a report on an entity in a space |
| `useHandleSpaceCommentReport` | Handle (resolve) a report on a comment in a space |

---

### Search Hooks (`hooks/search/`)
| Hook | Purpose |
|------|---------|
| `useSearchContent` | Semantic search over entities, comments, and messages |
| `useSearchUsers` | Semantic search over users |
| `useSearchSpaces` | Semantic search over spaces |
| `useAskContent` | AI-powered Q&A — ask a question, get an answer from the content |

---

### Space List Hooks (`hooks/space-lists/`)
| Hook | Purpose |
|------|---------|
| `useSpaceList` | Manage a paginated space list with filtering and sorting state |
| `useSpaceListActions` | Actions for space list (refresh, load more) |

---

### Spaces Hooks (`hooks/spaces/`)
| Hook | Purpose |
|------|---------|
| `useSpace` | High-level hook for full space state (data + actions) |
| `useSpaceData` | Internal data hook used by SpaceProvider |
| `useFetchSpace` | Fetch a space by ID |
| `useFetchSpaceByShortId` | Fetch a space by short ID |
| `useFetchSpaceBySlug` | Fetch a space by slug |
| `useFetchManySpaces` | Fetch paginated spaces with filtering |
| `useFetchUserSpaces` | Fetch spaces the current user is a member of |
| `useFetchSpaceChildren` | Fetch child spaces |
| `useFetchSpaceBreadcrumb` | Fetch ancestor breadcrumb for a space |
| `useFetchSpaceMembers` | Fetch members of a space |
| `useFetchSpaceTeam` | Fetch admins and moderators of a space |
| `useCheckMyMembership` | Check current user's membership status in a space |
| `useCheckSlugAvailability` | Check if a slug is available |
| `useCreateSpace` | Create a new space |
| `useUpdateSpace` | Update a space |
| `useDeleteSpace` | Delete a space |
| `useJoinSpace` | Join a space |
| `useLeaveSpace` | Leave a space |
| `useApproveMember` | Approve a pending membership |
| `useDeclineMember` | Decline a pending membership |
| `useRemoveMember` | Remove a member from a space |
| `useUpdateMemberRole` | Update a member's role |
| `useUnbanMember` | Unban a member |
| `useModerateSpaceEntity` | Moderate (approve/remove) an entity in a space |
| `useModerateSpaceComment` | Moderate (approve/remove) a comment in a space |
| `useSpaceMentions` | Fetch space mention suggestions |
| `useSpacePermissions` | Check current user's permissions in a space |
| `useFetchDigestConfig` | Fetch the digest/newsletter config for a space |
| `useUpdateDigestConfig` | Update the digest/newsletter config |

---

### Spaces Rules Hooks (`hooks/spaces/rules/`)
| Hook | Purpose |
|------|---------|
| `useFetchManyRules` | Fetch all rules for a space |
| `useFetchRule` | Fetch a single rule |
| `useCreateRule` | Create a new rule |
| `useUpdateRule` | Update a rule |
| `useDeleteRule` | Delete a rule |
| `useReorderRules` | Reorder rules by drag-and-drop |

---

### Storage Hooks (`hooks/storage/`)
| Hook | Purpose |
|------|---------|
| `useUploadFile` | Upload a generic file |
| `useUploadImage` | Upload and process an image (returns variants) |

---

### User Hooks (`hooks/user/`)
| Hook | Purpose |
|------|---------|
| `useUser` | Access the current user's full state |
| `useUserActions` | Actions for the current user (update, etc.) |

---

### Users Hooks (`hooks/users/`)
| Hook | Purpose |
|------|---------|
| `useFetchUser` | Fetch a user profile by ID |
| `useFetchUserByForeignId` | Fetch a user by foreign ID |
| `useFetchUserByUsername` | Fetch a user by username |
| `useFetchUserSuggestions` | Fetch user mention/search suggestions |
| `useCheckUsernameAvailability` | Check if a username is available |
| `useUserMentions` | Fetch users for @mention autocomplete |

---

### Crypto Hooks (`hooks/crypto/`)
| Hook | Purpose |
|------|---------|
| `useSignTestingJwt` | Sign a testing JWT (development/testing only) |

---

## Context Providers

All providers live in `monorepo/packages/core/src/context/`.

### ReplykeProvider (`replyke-context.tsx`)
**Purpose**: Root provider for standard (non-integration) mode. Sets up the Redux store and project context.
**Key props**: `projectId` (required), `signedToken` (optional — for external auth)
**Exposes**: `projectId`, `project` (project config)
**Notes**: Wraps children with `ReplykeStoreProvider`. Use this for most apps.

---

### ReplykeIntegrationProvider (`replyke-integration-context.tsx`)
**Purpose**: Alternative root provider for apps that already have a Redux store. Does NOT create its own Redux store — the app must add Replyke reducers manually.
**Key props**: `projectId` (required), `signedToken` (optional)
**Notes**: Requires app to configure `replykeReducers`, `replykeApiReducer`, and `replykeMiddleware` in their own store.

---

### ReplykeStoreProvider (`replyke-store-context.tsx`)
**Purpose**: Internal provider that wraps children with the Replyke Redux `Provider` and initializes auth state.
**Key props**: `projectId`, `signedToken`
**Notes**: Used internally by `ReplykeProvider`. Not typically used directly.

---

### EntityProvider (`entity-context.tsx`)
**Purpose**: Provides entity data and actions to a subtree, identified by entity ID, foreign ID, short ID, or a full entity object.
**Key props**: `entityId` | `foreignId` | `shortId` | `entity`
**Exposes**: Full `UseEntityDataValues` (entity object, loading, vote/react actions, etc.)
**Notes**: Returns null if none of the identifying props are provided.

---

### CommentSectionProvider (`comment-section-context.tsx`)
**Purpose**: Provides comment section state (comments list, pagination, sort, create/delete) to a subtree.
**Key props**: All `UseCommentSectionDataProps` (entityId, spaceId, sortBy, etc.)
**Exposes**: Full `UseCommentSectionDataValues`

---

### SpaceProvider (`space-context.tsx`)
**Purpose**: Provides space data and actions to a subtree, identified by space ID, short ID, or slug.
**Key props**: `spaceId` | `shortId` | `slug` | `space`
**Exposes**: Full `UseSpaceDataValues` (space object, membership, permissions, etc.)
**Notes**: Returns null if none of the identifying props are provided.

---

### ChatProvider (`chat-context.tsx`)
**Purpose**: Manages the socket.io connection for real-time chat. Maintains one persistent connection per authenticated user.
**Key props**: `children`
**Exposes**: `socket`, `connected`, `registerActiveConversation`, `unregisterActiveConversation`
**Notes**: Must wrap any UI that uses chat features. Handles incoming real-time events (new messages, reactions, typing indicators, read state).

---

### ConversationProvider (`conversation-context.tsx`)
**Purpose**: Manages the state of a single conversation: messages, members, real-time updates, and read state sync.
**Key props**: `conversationId`, `onDeleted?`
**Exposes**: Full `UseConversationDataValues` + `conversationId`
**Notes**: Must be inside ChatProvider. Registers itself as the active conversation to suppress duplicate unread increments.

---

### MessageThreadProvider (`message-thread-context.tsx`)
**Purpose**: Manages the state of a message thread (replies to a specific message).
**Key props**: `messageId`, `conversationId`
**Exposes**: Full `UseMessageThreadValues` + `messageId`
**Notes**: Must be inside ConversationProvider.

---

## Client Interfaces

These TypeScript interfaces from `monorepo/packages/core/src/interfaces/` are what developers actually receive and work with. They define the shape of data returned by hooks and API calls.

---

### User (`interfaces/models/User.ts`)

```ts
type UserRole = "admin" | "moderator" | "visitor";

// Public profile — returned when fetching any user
type User = {
  id: string;
  projectId: string;
  foreignId: string | null;
  role: UserRole;
  name: string | null;
  username: string | null;
  avatar: string | null;
  avatarFileId: string | null;
  bannerFileId: string | null;
  avatarFile?: File | null;
  bannerFile?: File | null;
  bio: string | null;           // max 300 chars
  birthdate: Date | null;
  location: { type: "Point"; coordinates: [number, number] } | null;
  metadata: Record<string, any>;
  reputation: number;
  createdAt: Date;
  // NOTE: email, secureMetadata, isVerified, isActive, lastActive, updatedAt are excluded
};

// Extended type for the authenticated user (about themselves)
type AuthUser = UserFull & {
  suspensions: { reason: string | null; startDate: Date; endDate: Date | null }[];
  authMethods: string[];        // e.g. ["password", "google", "github"]
  // NOTE: secureMetadata is excluded
};

// Full internal type (all fields, used server-side)
type UserFull = User & {
  email: string | null;
  secureMetadata: Record<string, any>;
  isVerified: boolean;
  isActive: boolean;
  lastActive: Date;
  updatedAt: Date;
};
```

**Include options**: `"files"` — populates `avatarFile` and `bannerFile`

---

### Entity (`interfaces/models/Entity.ts`)

```ts
interface Entity {
  id: string;
  foreignId: string | null;     // External system reference
  shortId: string;              // Auto-generated short ID for sharing links
  projectId: string;
  sourceId: string | null;      // Feed grouping identifier
  spaceId: string | null;
  space?: Space | null;         // Populated when include contains "space"
  user?: User | null;           // Populated when include contains "user"
  title: string | null;         // max 300 chars
  content: string | null;       // max 100,000 chars
  mentions: Mention[];
  attachments: Record<string, any>[];  // Flexible media attachment objects
  files?: File[];               // Populated when include contains "files"
  keywords: string[];           // Tags/search keywords
  upvotes: string[];            // Array of user IDs (legacy v6 field)
  downvotes: string[];          // Array of user IDs (legacy v6 field)
  reactionCounts: ReactionCounts; // v7 — counts for all 8 reaction types
  userReaction?: ReactionType | null; // Populated when authenticated
  repliesCount: number;
  views: number;
  score: number;                // Algorithmic hotness score
  scoreUpdatedAt: Date;
  location: { type: "Point"; coordinates: [number, number] } | null;
  metadata: Record<string, any>;  // max 1 MB
  topComment: TopComment | null;  // Populated when include contains "topComment"
  isSaved?: boolean;            // Populated when include contains "saved"
  isDraft: boolean | null;
  moderationStatus: "approved" | "removed" | null;
  moderatedAt: Date | null;
  moderatedById: string | null;
  moderatedByType: "client" | "user" | null;
  moderationReason: string | null;
  createdAt: Date;
  updatedAt: Date;
  deletedAt: Date | null;
}

interface TopComment {
  id: string;
  user: User;
  upvotesCount: number;
  content: string;
  createdAt: string;
}
```

**Include options**: `"space"` | `"user"` | `"topComment"` | `"saved"` | `"files"`

---

### Comment (`interfaces/models/Comment.ts`)

```ts
interface Comment {
  id: string;
  projectId: string;
  foreignId: string | null;
  entityId: string;
  entity?: Entity;              // Populated when include contains "entity" or "space"
  userId: string;
  user?: User;                  // Populated when include contains "user"
  parentId: string | null;
  parentComment?: Comment;      // Populated when include contains "parent"
  content: string | null;
  gif: GifData | null;
  mentions: Mention[];
  upvotes: string[];            // Legacy v6
  downvotes: string[];          // Legacy v6
  reactionCounts: ReactionCounts;
  userReaction?: ReactionType | null;
  repliesCount: number;
  metadata: Record<string, any>;
  parentDeletedAt: Date | null;
  userDeletedAt: Date | null;   // Reddit-style soft delete
  moderationStatus: "approved" | "removed" | null;
  moderatedAt: Date | null;
  moderatedById: string | null;
  moderatedByType: "client" | "user" | null;
  moderationReason: string | null;
  createdAt: Date;
  updatedAt: Date;
  deletedAt: Date | null;
}

interface GifData {
  id: string;
  url: string;
  gifUrl: string;
  gifPreviewUrl: string;
  altText: string | undefined;
  aspectRatio: number;
}
```

**Include options**: `"user"` | `"entity"` | `"space"` | `"parent"`

---

### ChatMessage (`interfaces/models/ChatMessage.ts`)

```ts
interface ChatMessage {
  id: string;
  clientId?: string;            // Client-generated UUID for optimistic deduplication
  projectId: string;
  conversationId: string;
  userId: string | null;        // Null when sender's account has been deleted
  content: string | null;
  gif: GifData | null;
  mentions: Mention[];
  files?: File[];               // Opt-in via includeFiles: true
  metadata: Record<string, any>;
  parentMessageId: string | null;     // Thread reply parent
  quotedMessageId: string | null;     // Quoted message reference
  threadReplyCount: number;
  reactionCounts: Record<string, number>; // emoji → count
  userReactions: string[];            // Emojis the requesting user reacted with
  editedAt: Date | null;
  userDeletedAt: Date | null;
  moderationStatus: "approved" | "removed" | null;
  moderatedAt: Date | null;
  moderatedById: string | null;
  moderatedByType: "client" | "user" | null;
  moderationReason: string | null;
  createdAt: Date;
  updatedAt: Date;
  // Populated fields
  user: User | null;
  quotedMessage?: ChatMessage | null;
  parentMessage?: ChatMessage | null;
  sendFailed?: boolean;         // Client-only — set on optimistic send failure
}
```

---

### Conversation (`interfaces/models/Conversation.ts`)

```ts
interface Conversation {
  id: string;
  projectId: string;
  type: "direct" | "group" | "space";
  name: string | null;
  description: string | null;
  spaceId: string | null;
  createdById: string | null;
  avatarFileId: string | null;
  lastMessageAt: Date | null;
  postingPermission: "members" | "admins" | null;
  metadata: Record<string, any>;
  createdAt: Date;
  updatedAt: Date;
  // Computed fields
  memberCount?: number;
  currentMember?: ConversationMember;  // The requesting user's own member row
  avatarFile?: File;
}

// Extended — returned in conversation list views
interface ConversationPreview extends Conversation {
  unreadCount: number;
  lastMessage: ChatMessage | null;   // Truncated to 100 chars
}
```

---

### ConversationMember (`interfaces/models/ConversationMember.ts`)

```ts
type ConversationMemberRole = "admin" | "member";

interface ConversationMember {
  id: string;
  projectId: string;
  conversationId: string;
  userId: string;
  role: ConversationMemberRole | null;
  lastReadAt: Date | null;
  mutedUntil: Date | null;
  isActive: boolean;
  leftAt: Date | null;
  createdAt: Date;
  updatedAt: Date;
  user?: User;
}
```

---

### Space (`interfaces/models/Space.ts`)

```ts
type ReadingPermission = "anyone" | "members";
type PostingPermission = "anyone" | "members" | "admins";
type SpaceMemberRole = "admin" | "moderator" | "member";
type SpaceMemberStatus = "pending" | "active" | "banned" | "rejected";

// Lightweight version for breadcrumbs, child previews, etc.
interface SpacePreview {
  id: string;
  shortId: string;
  name: string;
  slug: string | null;
  avatarFileId: string | null;
  readingPermission?: ReadingPermission;
  parentSpaceId?: string | null;
  depth?: number;
  avatarFile?: File;
}

// Full space object
interface Space {
  id: string;
  projectId: string;
  shortId: string;
  slug: string | null;
  name: string;
  description: string | null;
  avatarFileId: string | null;
  bannerFileId: string | null;
  userId: string;               // Creator
  readingPermission: ReadingPermission;
  postingPermission: PostingPermission;
  requireJoinApproval: boolean;
  parentSpaceId: string | null;
  depth: number;
  metadata: Record<string, any>;
  createdAt: Date;
  updatedAt: Date;
  deletedAt: Date | null;
  // Computed
  membersCount: number;
  childSpacesCount: number;
  isMember?: boolean;
  avatarFile?: File;
  bannerFile?: File;
}

// Returned from single-space fetch endpoints
interface SpaceDetailed extends Space {
  memberPermissions: SpaceMemberPermissions | null;
  parentSpace: SpacePreview | null;
  childSpaces: SpacePreview[];
}

interface SpaceMemberPermissions {
  isAdmin: boolean;
  isModerator: boolean;
  isMember: boolean;
  status: "pending" | "active" | "banned" | null;
  canPost: boolean;
  canModerate: boolean;
  canRead: boolean;
}

// Returned from checkMyMembership endpoint
interface CheckMyMembershipResponse {
  isMember: boolean;
  role: "admin" | "moderator" | "member" | null;
  status: "pending" | "active" | "banned" | "rejected" | null;
  joinedAt: Date | null;
  permissions: {
    canPost: boolean;
    canModerate: boolean;
    canRead: boolean;
    isAdmin: boolean;
    isModerator: boolean;
  };
}

// Digest/newsletter config
interface DigestConfig {
  digestEnabled: boolean;
  digestWebhookUrl: string | null;
  digestWebhookSecret: string | null;   // Masked as "••••••••" when set
  digestScheduleHour: number | null;
  digestTimezone: string | null;
}
```

**Include options**: `"files"` — populates `avatarFile` and `bannerFile`

---

### SpaceMember (`interfaces/models/SpaceMember.ts`)

```ts
interface SpaceMember {
  id: string;
  projectId: string;
  spaceId: string;
  userId: string;
  role: SpaceMemberRole;      // "admin" | "moderator" | "member"
  status: SpaceMemberStatus;  // "pending" | "active" | "banned" | "rejected"
  joinedAt: Date;
  createdAt: Date;
}

// Returned from fetchSpaceMembers — includes public user profile
interface SpaceMemberWithUser {
  membershipId: string;
  role: SpaceMemberRole;
  status: SpaceMemberStatus;
  joinedAt: Date;
  user: {
    id: string;
    username: string;
    displayName: string;
    avatar: string;
    metadata: object;
  };
}
```

---

### Collection (`interfaces/models/Collection.ts`)

```ts
interface Collection {
  id: string;
  projectId: string;
  userId: string;
  parentId: string | null;    // null = root collection
  name: string;
  entityCount?: number;
  createdAt: Date;
  updatedAt: Date;
}
```

---

### Rule (`interfaces/models/Rule.ts`)

```ts
interface Rule {
  id: string;
  projectId: string;
  spaceId: string;
  title: string;
  description: string | null;
  order: number;
  lastApprovedBy: string | null;
  createdAt: Date;
  updatedAt: Date;
}
```

---

### Reaction (`interfaces/models/Reaction.ts`)

```ts
type ReactionType =
  | "upvote"    // +1 reputation
  | "downvote"  // -1 reputation
  | "like"      // +1 reputation
  | "love"      // +2 reputation
  | "wow"       // +1 reputation
  | "sad"       // 0 reputation
  | "angry"     // 0 reputation
  | "funny";    // +1 reputation

interface ReactionCounts {
  upvote: number;
  downvote: number;
  like: number;
  love: number;
  wow: number;
  sad: number;
  angry: number;
  funny: number;
}

// Individual reaction record (from reactions fetch endpoints)
interface Reaction {
  id: string;
  projectId: string;
  targetType: "entity" | "comment";
  targetId: string;
  userId: string;
  reactionType: ReactionType;
  createdAt: string;
  updatedAt: string;
  user?: User;
}
```

---

### File (`interfaces/models/File.ts`)

```ts
interface File {
  id: string;
  projectId: string;
  userId: string | null;
  entityId: string | null;
  commentId: string | null;
  chatMessageId: string | null;
  spaceId: string | null;
  type: "image" | "video" | "document" | "other";
  originalPath: string;         // Proxied URL
  originalSize: number;
  originalMimeType: string;
  position: number;
  metadata: Record<string, any>;
  image?: FileImage;            // Only for type: "image"
  createdAt: Date;
  updatedAt: Date;
}

interface FileImage {
  fileId: string;
  originalWidth: number;
  originalHeight: number;
  variants: Record<string, FileImageVariant>;  // e.g. thumbnail, small, medium, large
  processingStatus: "completed" | "failed";
  processingError: string | null;
  format: string;
  quality: number;
  exifStripped: boolean;
  createdAt: Date;
  updatedAt: Date;
}

interface FileImageVariant {
  path: string;
  publicPath: string;
  width: number;
  height: number;
  size: number;
  format: string;
}
```

---

### Image (`interfaces/models/Image.ts`)

Returned from the image upload endpoint (`POST /storage/images`).

```ts
interface Image {
  fileId: string;
  imageId: string;              // Same as fileId (backward compatibility)
  status: "completed" | "failed";
  original: ImageVariant;
  variants: Record<string, ImageVariant>;  // Named variants defined by upload options
  metadata: {
    originalFormat: string;
    originalSize: number;
    exifStripped: boolean;
    processingTime: number;     // milliseconds
  };
  createdAt: string;
}

interface ImageVariant {
  path: string;
  publicPath: string;
  width: number;
  height: number;
  size: number;
  format: string;
}
```

**Upload modes** (`UploadImageOptions`):
- `exact-dimensions` — specify exact width/height per named variant
- `aspect-ratio-width-based` — aspect ratio + named widths
- `aspect-ratio-height-based` — aspect ratio + named heights
- `original-aspect` — preserve original ratio, scale by max size
- `multi-aspect-ratio` — multiple aspect ratios × named sizes

---

### Follow (`interfaces/models/Follow.ts`)

```ts
interface Follow {
  id: string;
  projectId: string;
  followerId: string;
  followedId: string;
  createdAt: Date;
}
```

---

### Connection (`interfaces/models/Connection.ts`)

```ts
// Established connection — returned in connections list
interface EstablishedConnection {
  id: string;
  connectedUser: User;
  connectedAt: string;
  requestedAt: string;
}

// Pending connection request
interface PendingConnection {
  id: string;
  user: User;
  type: "received" | "sent";
  message?: string;
  createdAt: string;
}

// Union type for connection status between two users
type ConnectionStatusResponse =
  | { status: "none" }
  | { status: "pending"; type: "sent" | "received"; connectionId: string; createdAt: string }
  | { status: "connected"; connectionId: string; connectedAt: string; requestedAt: string }
  | { status: "declined"; type: "sent" | "received"; connectionId: string; respondedAt: string };

type ConnectionStatus =
  | "none"
  | "pending-sent"
  | "pending-received"
  | "connected"
  | "declined-sent"
  | "declined-received";
```

---

### Mention (`interfaces/models/Mention.ts`)

```ts
type Mention = UserMention | SpaceMention;

interface UserMention {
  type: "user";
  id: string;
  foreignId?: string | null;
  username: string;
}

interface SpaceMention {
  type: "space";
  id: string;
  slug: string;
}
```

---

### AppNotification (`interfaces/models/AppNotification.ts`)

All notifications extend a base type. The `type` discriminant narrows the `metadata` shape.

```ts
type AppNotificationType =
  | "system"
  | "entity-comment"
  | "comment-reply"
  | "entity-mention"
  | "comment-mention"
  | "entity-upvote"
  | "comment-upvote"
  | "entity-reaction"
  | "comment-reaction"
  | "entity-reaction-milestone-specific"
  | "entity-reaction-milestone-total"
  | "comment-reaction-milestone-specific"
  | "comment-reaction-milestone-total"
  | "new-follow"
  | "connection-request"
  | "connection-accepted";

interface BaseAppNotification {
  id: string;
  userId: string;
  type: AppNotificationType;
  isRead: boolean;
  metadata: Record<string, any>;
  createdAt: string;
}

// Union of all concrete notification types
type UnifiedAppNotification =
  | SystemNotification
  | EntityCommentNotification
  | CommentReplyNotification
  | EntityMentionNotification
  | CommentMentionNotification
  | EntityUpvoteNotification
  | CommentUpvoteNotification
  | EntityReactionNotification
  | CommentReactionNotification
  | EntityReactionMilestoneSpecificNotification
  | EntityReactionMilestoneTotalNotification
  | CommentReactionMilestoneSpecificNotification
  | CommentReactionMilestoneTotalNotification
  | NewFollowNotification
  | ConnectionRequestNotification
  | ConnectionAcceptedNotification;
```

**Key notification shapes:**

| Type | `action` | Key metadata fields |
|------|----------|---------------------|
| `system` | custom string | `title?`, `content?`, `buttonData` |
| `entity-comment` | `open-comment` | `entityId`, `entityShortId`, `commentId`, initiator info |
| `comment-reply` | `open-comment` | `entityId`, `commentId`, `replyId`, initiator info |
| `entity-mention` | `open-entity` | `entityId`, `entityShortId`, initiator info |
| `comment-mention` | `open-comment` | `entityId`, `commentId`, initiator info |
| `entity-upvote` | `open-entity` | `entityId`, `entityShortId`, initiator info |
| `comment-upvote` | `open-comment` | `entityId`, `commentId`, initiator info |
| `entity-reaction` | `open-entity` | `entityId`, `reactionType`, initiator info |
| `comment-reaction` | `open-comment` | `entityId`, `commentId`, `reactionType`, initiator info |
| `entity-reaction-milestone-specific` | `open-entity` | `entityId`, `reactionType`, `milestoneCount`, `lastThreeUsers` |
| `entity-reaction-milestone-total` | `open-entity` | `entityId`, `milestoneCount`, `reactionCounts`, `lastThreeUsers` |
| `comment-reaction-milestone-specific` | `open-comment` | `commentId`, `reactionType`, `milestoneCount`, `lastThreeUsers` |
| `comment-reaction-milestone-total` | `open-comment` | `commentId`, `milestoneCount`, `reactionCounts`, `lastThreeUsers` |
| `new-follow` | `open-profile` | initiator info |
| `connection-request` | `open-profile` | `connectionId`, initiator info |
| `connection-accepted` | `open-profile` | `connectionId`, initiator info |

---

### Entity Filters (`interfaces/entity-filters/`)

Used when fetching entity lists to filter results.

```ts
interface AttachmentsFilters {
  hasAttachments?: boolean;
}

interface ContentFilters {
  hasContent?: boolean;
  includes?: string | string[];
  doesNotInclude?: string | string[];
}

interface KeywordsFilters {
  includes?: string[];
  doesNotInclude?: string[];
}

type LocationFilters = {
  latitude: number;
  longitude: number;
  radius: number;         // Distance in meters
};

interface MetadataFilters {
  includes?: Record<string, unknown>;       // All key-value pairs must match
  includesAny?: Record<string, unknown>[];  // At least one object must match
  doesNotInclude?: Record<string, unknown>;
  exists?: string[];                        // Keys that must be present
  doesNotExist?: string[];                  // Keys that must be absent
}

interface TitleFilters {
  hasTitle?: boolean;
  includes?: string | string[];
  doesNotInclude?: string | string[];
}
```

---

### Utility Types (`interfaces/`)

```ts
// Generic paginated response wrapper
interface PaginatedResponse<T> {
  data: T[];
  pagination: {
    page: number;
    pageSize: number;
    totalPages: number;
    totalItems: number;
    hasMore: boolean;
  };
}

// Entity sort options
type EntityListSortByOptions =
  | "top"
  | "hot"
  | "new"
  | "controversial"
  | `metadata.${string}`;    // Sort by any metadata property (alphanumeric keys only)

// Space sort options
type SpaceListSortByOptions = "newest" | "members" | "alphabetical";

// Space list filters
interface SpaceListFilters {
  search?: string | null;
  visibility?: "public" | "private" | null;
  memberOf?: boolean;
  parentSpaceId?: string | null;   // "null" string for root spaces, UUID for children
}

// Comment sort options
type CommentsSortByOptions = "top" | "new" | "old";

// Time frame for filtering/analytics
type TimeFrame = "hour" | "day" | "week" | "month" | "year";

// Breadcrumb response from fetchSpaceBreadcrumb
interface SpaceBreadcrumb {
  breadcrumb: SpacePreview[];
  depth: number;
}

// Tree structure for comment sections (client-side state)
type EntityCommentsTree = {
  [id: string]: {
    comment: Comment;
    replies: { [id: string]: Comment & { new: boolean } };
    new: boolean;
    failed?: boolean;
  };
};

// Interface for multi-account token storage adapters (React Native)
interface IAccountStorage {
  getAccountMap(projectId: string): Promise<AccountMap | null>;
  setAccountMap(projectId: string, map: AccountMap): Promise<void>;
  deleteAccountMap(projectId: string): Promise<void>;
}
```

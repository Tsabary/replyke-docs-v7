# Docs v7 Page Tracker

## Status Legend
- [ ] TODO
- [~] IN PROGRESS (agent actively working on this page right now)
- [x] DONE
- [!] BLOCKED — needs human decision or review before continuing

---

## Overview

- [x] index.mdx
- [x] integration-options.mdx
- [x] authentication.mdx
- [x] webhooks.mdx
- [x] semantic-search-ai.mdx
- [x] mcp-server.mdx

## SDK Reference — Setup

- [x] sdk/getting-started.mdx
- [x] sdk/redux-integration.mdx

## SDK Reference — Authentication

- [x] sdk/authentication/overview.mdx
- [x] sdk/authentication/built-in.mdx
- [x] sdk/authentication/external.mdx
- [x] sdk/authentication/oauth.mdx
- [x] sdk/authentication/multi-account.mdx

## SDK Reference — Current User

- [x] sdk/current-user/overview.mdx

## SDK Reference — User Profiles

- [x] sdk/user-profiles/overview.mdx

## SDK Reference — Entities

- [x] sdk/entities/overview.mdx
- [x] sdk/entities/provider-and-hook.mdx
- [x] sdk/entities/drafts-and-publishing.mdx

## SDK Reference — Entity Lists

- [x] sdk/entity-lists/overview.mdx
- [x] sdk/entity-lists/fetch-entities.mdx
- [x] sdk/entity-lists/infuse-data.mdx
- [x] sdk/entity-lists/filters.mdx

## SDK Reference — Comments

- [x] sdk/comments/overview.mdx
- [x] sdk/comments/comment-section.mdx

## SDK Reference — Reactions

- [x] sdk/reactions/overview.mdx

## SDK Reference — Spaces

- [x] sdk/spaces/overview.mdx
- [x] sdk/spaces/provider-and-hook.mdx
- [x] sdk/spaces/membership.mdx
- [x] sdk/spaces/moderation.mdx
- [x] sdk/spaces/rules.mdx
- [x] sdk/spaces/digest-newsletter.mdx
- [x] sdk/spaces/space-lists.mdx

## SDK Reference — Chat

- [x] sdk/chat/overview.mdx
- [x] sdk/chat/setup.mdx
- [x] sdk/chat/conversations.mdx
- [x] sdk/chat/messages.mdx
- [x] sdk/chat/real-time.mdx
- [x] sdk/chat/threads.mdx

## SDK Reference — Collections

- [ ] sdk/collections/overview.mdx
- [ ] sdk/collections/managing-collections.mdx

## SDK Reference — Relationships

- [ ] sdk/relationships/overview.mdx
- [ ] sdk/relationships/use-follow-manager.mdx
- [ ] sdk/relationships/use-connection-manager.mdx

## SDK Reference — App Notifications

- [ ] sdk/app-notifications/overview.mdx
- [ ] sdk/app-notifications/hook.mdx
- [ ] sdk/app-notifications/notification-templates.mdx
- [ ] sdk/app-notifications/webhook-integration.mdx

## SDK Reference — Search & AI

- [ ] sdk/search/overview.mdx
- [ ] sdk/search/semantic-search.mdx
- [ ] sdk/search/ask.mdx

## SDK Reference — Storage

- [ ] sdk/storage/overview.mdx

## SDK Reference — Moderation

- [ ] sdk/moderation/overview.mdx

## Hooks Reference — Auth

- [x] hooks/auth/use-auth.mdx
- [x] hooks/auth/use-accounts.mdx
- [x] hooks/auth/use-add-account.mdx
- [x] hooks/auth/use-remove-account.mdx
- [x] hooks/auth/use-switch-account.mdx
- [x] hooks/auth/use-sign-out-all.mdx
- [x] hooks/auth/use-oauth-identities.mdx
- [x] hooks/auth/use-request-password-reset.mdx

## Hooks Reference — Chat: Conversations

- [x] hooks/chat/conversations/use-conversations.mdx
- [x] hooks/chat/conversations/use-conversation.mdx
- [x] hooks/chat/conversations/use-create-direct-conversation.mdx
- [x] hooks/chat/conversations/use-conversation-members.mdx
- [x] hooks/chat/conversations/use-space-conversation.mdx

## Hooks Reference — Chat: Messages

- [x] hooks/chat/messages/use-chat-messages.mdx
- [x] hooks/chat/messages/use-send-message.mdx
- [x] hooks/chat/messages/use-edit-message.mdx
- [x] hooks/chat/messages/use-delete-message.mdx
- [x] hooks/chat/messages/use-toggle-reaction.mdx
- [x] hooks/chat/messages/use-message-thread.mdx

## Hooks Reference — Chat: Core

- [x] hooks/chat/use-chat-socket.mdx
- [x] hooks/chat/use-mark-conversation-as-read.mdx
- [x] hooks/chat/use-report-message.mdx
- [x] hooks/chat/use-total-unread-count.mdx
- [x] hooks/chat/use-typing-indicator.mdx
- [x] hooks/chat/use-unread-conversation-count.mdx

## Hooks Reference — Collections

- [ ] hooks/collections/use-collections.mdx
- [ ] hooks/collections/use-collections-actions.mdx
- [ ] hooks/collections/use-collection-entities-wrapper.mdx
- [ ] hooks/collections/use-is-entity-saved.mdx

## Hooks Reference — Comments

- [x] hooks/comments/use-comment-section.mdx
- [x] hooks/comments/use-entity-comments.mdx
- [x] hooks/comments/use-replies.mdx
- [x] hooks/comments/use-create-comment.mdx
- [x] hooks/comments/use-update-comment.mdx
- [x] hooks/comments/use-delete-comment.mdx
- [x] hooks/comments/use-fetch-comment.mdx
- [x] hooks/comments/use-fetch-comment-by-foreign-id.mdx
- [x] hooks/comments/use-fetch-many-comments.mdx

## Hooks Reference — Crypto

- [ ] hooks/crypto/use-sign-testing-jwt.mdx

## Hooks Reference — Entities

- [x] hooks/entities/use-entity.mdx
- [x] hooks/entities/use-fetch-entity.mdx
- [x] hooks/entities/use-fetch-entity-by-foreign-id.mdx
- [x] hooks/entities/use-fetch-entity-by-short-id.mdx
- [x] hooks/entities/use-fetch-many-entities.mdx
- [x] hooks/entities/use-create-entity.mdx
- [x] hooks/entities/use-update-entity.mdx
- [x] hooks/entities/use-delete-entity.mdx
- [x] hooks/entities/use-fetch-drafts.mdx
- [x] hooks/entities/use-publish-draft.mdx
- [x] hooks/entities/use-increment-entity-views.mdx

## Hooks Reference — Entity Lists

- [x] hooks/entity-lists/use-entity-list.mdx
- [x] hooks/entity-lists/use-entity-list-actions.mdx
- [x] hooks/entity-lists/use-infused-data.mdx

## Hooks Reference — Reactions

- [x] hooks/reactions/use-add-reaction.mdx
- [x] hooks/reactions/use-remove-reaction.mdx
- [x] hooks/reactions/use-reaction-toggle.mdx
- [x] hooks/reactions/use-fetch-entity-reactions.mdx
- [x] hooks/reactions/use-fetch-comment-reactions.mdx

## Hooks Reference — Follows

- [ ] hooks/follows/use-follow-manager.mdx
- [ ] hooks/follows/use-follow-user.mdx
- [ ] hooks/follows/use-unfollow-by-follow-id.mdx
- [ ] hooks/follows/use-unfollow-user-by-user-id.mdx
- [ ] hooks/follows/use-fetch-follow-status.mdx
- [ ] hooks/follows/use-fetch-followers.mdx
- [ ] hooks/follows/use-fetch-followers-by-user-id.mdx
- [ ] hooks/follows/use-fetch-followers-count.mdx
- [ ] hooks/follows/use-fetch-followers-count-by-user-id.mdx
- [ ] hooks/follows/use-fetch-following.mdx
- [ ] hooks/follows/use-fetch-following-by-user-id.mdx
- [ ] hooks/follows/use-fetch-following-count.mdx
- [ ] hooks/follows/use-fetch-following-count-by-user-id.mdx

## Hooks Reference — Connections

- [ ] hooks/connections/use-connection-manager.mdx
- [ ] hooks/connections/use-request-connection.mdx
- [ ] hooks/connections/use-accept-connection.mdx
- [ ] hooks/connections/use-decline-connection.mdx
- [ ] hooks/connections/use-remove-connection.mdx
- [ ] hooks/connections/use-remove-connection-by-user-id.mdx
- [ ] hooks/connections/use-fetch-connections.mdx
- [ ] hooks/connections/use-fetch-connections-by-user-id.mdx
- [ ] hooks/connections/use-fetch-connections-count.mdx
- [ ] hooks/connections/use-fetch-connections-count-by-user-id.mdx
- [ ] hooks/connections/use-fetch-connection-status.mdx
- [ ] hooks/connections/use-fetch-sent-pending-connections.mdx
- [ ] hooks/connections/use-fetch-received-pending-connections.mdx

## Hooks Reference — App Notifications

- [ ] hooks/app-notifications/use-app-notifications.mdx

## Hooks Reference — Reports

- [ ] hooks/reports/use-create-report.mdx
- [ ] hooks/reports/use-fetch-moderated-reports.mdx
- [ ] hooks/reports/use-handle-space-entity-report.mdx
- [ ] hooks/reports/use-handle-space-comment-report.mdx

## Hooks Reference — Search

- [ ] hooks/search/use-search-content.mdx
- [ ] hooks/search/use-search-users.mdx
- [ ] hooks/search/use-search-spaces.mdx
- [ ] hooks/search/use-ask-content.mdx

## Hooks Reference — Spaces

- [x] hooks/spaces/use-space.mdx
- [x] hooks/spaces/use-fetch-space.mdx
- [x] hooks/spaces/use-fetch-space-by-short-id.mdx
- [x] hooks/spaces/use-fetch-space-by-slug.mdx
- [x] hooks/spaces/use-fetch-many-spaces.mdx
- [x] hooks/spaces/use-fetch-user-spaces.mdx
- [x] hooks/spaces/use-fetch-space-children.mdx
- [x] hooks/spaces/use-fetch-space-breadcrumb.mdx
- [x] hooks/spaces/use-fetch-space-members.mdx
- [x] hooks/spaces/use-fetch-space-team.mdx
- [x] hooks/spaces/use-check-my-membership.mdx
- [x] hooks/spaces/use-check-slug-availability.mdx
- [x] hooks/spaces/use-create-space.mdx
- [x] hooks/spaces/use-update-space.mdx
- [x] hooks/spaces/use-delete-space.mdx
- [x] hooks/spaces/use-join-space.mdx
- [x] hooks/spaces/use-leave-space.mdx
- [x] hooks/spaces/use-approve-member.mdx
- [x] hooks/spaces/use-decline-member.mdx
- [x] hooks/spaces/use-remove-member.mdx
- [x] hooks/spaces/use-update-member-role.mdx
- [x] hooks/spaces/use-unban-member.mdx
- [x] hooks/spaces/use-moderate-space-entity.mdx
- [x] hooks/spaces/use-moderate-space-comment.mdx
- [x] hooks/spaces/use-space-mentions.mdx
- [x] hooks/spaces/use-space-permissions.mdx
- [x] hooks/spaces/use-fetch-digest-config.mdx
- [x] hooks/spaces/use-update-digest-config.mdx

## Hooks Reference — Spaces: Rules

- [x] hooks/spaces/rules/use-fetch-many-rules.mdx
- [x] hooks/spaces/rules/use-fetch-rule.mdx
- [x] hooks/spaces/rules/use-create-rule.mdx
- [x] hooks/spaces/rules/use-update-rule.mdx
- [x] hooks/spaces/rules/use-delete-rule.mdx
- [x] hooks/spaces/rules/use-reorder-rules.mdx

## Hooks Reference — Space Lists

- [x] hooks/space-lists/use-space-list.mdx
- [x] hooks/space-lists/use-space-list-actions.mdx

## Hooks Reference — Storage

- [ ] hooks/storage/use-upload-file.mdx
- [ ] hooks/storage/use-upload-image.mdx

## Hooks Reference — Current User

- [x] hooks/user/use-user.mdx
- [x] hooks/user/use-user-actions.mdx

## Hooks Reference — User Profiles

- [x] hooks/users/use-fetch-user.mdx
- [x] hooks/users/use-fetch-user-by-foreign-id.mdx
- [x] hooks/users/use-fetch-user-by-username.mdx
- [x] hooks/users/use-fetch-user-suggestions.mdx
- [x] hooks/users/use-check-username-availability.mdx
- [x] hooks/users/use-user-mentions.mdx

## API Reference — Getting Started

- [ ] api-reference/getting-started.mdx
- [ ] api-reference/authentication.mdx

## API Reference — Auth

- [x] api-reference/auth/sign-up.mdx
- [x] api-reference/auth/sign-in.mdx
- [x] api-reference/auth/sign-out.mdx
- [x] api-reference/auth/change-password.mdx
- [x] api-reference/auth/request-access-token.mdx
- [x] api-reference/auth/verify-external-user.mdx
- [x] api-reference/auth/request-password-reset.mdx
- [x] api-reference/auth/reset-password.mdx

## API Reference — OAuth

- [x] api-reference/oauth/authorize.mdx
- [x] api-reference/oauth/link.mdx
- [x] api-reference/oauth/list-identities.mdx
- [x] api-reference/oauth/unlink-identity.mdx

## API Reference — Users

- [ ] api-reference/users/fetch-user.mdx
- [ ] api-reference/users/fetch-user-by-foreign-id.mdx
- [ ] api-reference/users/fetch-user-by-username.mdx
- [ ] api-reference/users/fetch-user-suggestions.mdx
- [ ] api-reference/users/check-username-availability.mdx
- [ ] api-reference/users/update-user.mdx

## API Reference — User Follow Operations

- [ ] api-reference/users/follows/follow-user.mdx
- [ ] api-reference/users/follows/get-follow-status.mdx
- [ ] api-reference/users/follows/get-followers.mdx
- [ ] api-reference/users/follows/get-followers-count.mdx
- [ ] api-reference/users/follows/get-following.mdx
- [ ] api-reference/users/follows/get-following-count.mdx
- [ ] api-reference/users/follows/unfollow-user.mdx

## API Reference — User Connection Operations

- [ ] api-reference/users/connections/request-connection.mdx
- [ ] api-reference/users/connections/get-connection-status.mdx
- [ ] api-reference/users/connections/get-connections.mdx
- [ ] api-reference/users/connections/get-connections-count.mdx
- [ ] api-reference/users/connections/remove-connection.mdx

## API Reference — Follow Endpoints

- [ ] api-reference/follows/fetch-following.mdx
- [ ] api-reference/follows/fetch-followers.mdx
- [ ] api-reference/follows/fetch-following-count.mdx
- [ ] api-reference/follows/fetch-followers-count.mdx
- [ ] api-reference/follows/delete-follow.mdx

## API Reference — Connection Endpoints

- [ ] api-reference/connections/fetch-connections.mdx
- [ ] api-reference/connections/fetch-connections-count.mdx
- [ ] api-reference/connections/fetch-sent-pending.mdx
- [ ] api-reference/connections/fetch-received-pending.mdx
- [ ] api-reference/connections/accept-connection.mdx
- [ ] api-reference/connections/decline-connection.mdx
- [ ] api-reference/connections/remove-connection.mdx

## API Reference — Entities

- [ ] api-reference/entities/create-entity.mdx
- [ ] api-reference/entities/fetch-entity.mdx
- [ ] api-reference/entities/fetch-entity-by-foreign-id.mdx
- [ ] api-reference/entities/fetch-entity-by-short-id.mdx
- [ ] api-reference/entities/fetch-many-entities.mdx
- [ ] api-reference/entities/fetch-drafts.mdx
- [ ] api-reference/entities/publish-entity.mdx
- [ ] api-reference/entities/fetch-top-comment.mdx
- [ ] api-reference/entities/update-entity.mdx
- [ ] api-reference/entities/delete-entity.mdx
- [ ] api-reference/entities/upvote-entity.mdx
- [ ] api-reference/entities/downvote-entity.mdx
- [ ] api-reference/entities/remove-entity-upvote.mdx
- [ ] api-reference/entities/remove-entity-downvote.mdx
- [ ] api-reference/entities/add-reaction.mdx
- [ ] api-reference/entities/remove-reaction.mdx
- [ ] api-reference/entities/fetch-reactions.mdx
- [ ] api-reference/entities/increment-views.mdx

## API Reference — Comments

- [ ] api-reference/comments/create-comment.mdx
- [ ] api-reference/comments/fetch-comment.mdx
- [ ] api-reference/comments/fetch-comment-by-foreign-id.mdx
- [ ] api-reference/comments/fetch-many-comments.mdx
- [ ] api-reference/comments/update-comment.mdx
- [ ] api-reference/comments/delete-comment.mdx
- [ ] api-reference/comments/upvote-comment.mdx
- [ ] api-reference/comments/downvote-comment.mdx
- [ ] api-reference/comments/remove-comment-upvote.mdx
- [ ] api-reference/comments/remove-comment-downvote.mdx
- [ ] api-reference/comments/add-reaction.mdx
- [ ] api-reference/comments/remove-reaction.mdx
- [ ] api-reference/comments/fetch-reactions.mdx

## API Reference — Chat: Conversations

- [x] api-reference/chat/conversations/list-conversations.mdx
- [x] api-reference/chat/conversations/create-group.mdx
- [x] api-reference/chat/conversations/create-direct.mdx
- [x] api-reference/chat/conversations/unread-count.mdx
- [x] api-reference/chat/conversations/fetch-conversation.mdx
- [x] api-reference/chat/conversations/update-conversation.mdx
- [x] api-reference/chat/conversations/delete-conversation.mdx

## API Reference — Chat: Members

- [x] api-reference/chat/members/list-members.mdx
- [x] api-reference/chat/members/add-member.mdx
- [x] api-reference/chat/members/remove-member.mdx
- [x] api-reference/chat/members/leave-conversation.mdx
- [x] api-reference/chat/members/update-member-role.mdx

## API Reference — Chat: Messages

- [x] api-reference/chat/messages/list-messages.mdx
- [x] api-reference/chat/messages/send-message.mdx
- [x] api-reference/chat/messages/fetch-message.mdx
- [x] api-reference/chat/messages/edit-message.mdx
- [x] api-reference/chat/messages/delete-message.mdx
- [x] api-reference/chat/messages/toggle-reaction.mdx
- [x] api-reference/chat/messages/fetch-reaction-users.mdx
- [x] api-reference/chat/messages/mark-as-read.mdx
- [x] api-reference/chat/messages/report-message.mdx

## API Reference — Collections

- [ ] api-reference/collections/fetch-root.mdx
- [ ] api-reference/collections/is-entity-saved.mdx
- [ ] api-reference/collections/create-sub-collection.mdx
- [ ] api-reference/collections/fetch-sub-collections.mdx
- [ ] api-reference/collections/fetch-entities.mdx
- [ ] api-reference/collections/add-entity.mdx
- [ ] api-reference/collections/remove-entity.mdx
- [ ] api-reference/collections/update-collection.mdx
- [ ] api-reference/collections/delete-collection.mdx

## API Reference — App Notifications

- [ ] api-reference/app-notifications/fetch-notifications.mdx
- [ ] api-reference/app-notifications/count-unread.mdx
- [ ] api-reference/app-notifications/mark-as-read.mdx
- [ ] api-reference/app-notifications/mark-all-as-read.mdx

## API Reference — Reports

- [ ] api-reference/reports/create-report.mdx
- [ ] api-reference/reports/fetch-moderated-reports.mdx

## API Reference — Search

- [ ] api-reference/search/search-content.mdx
- [ ] api-reference/search/search-users.mdx
- [ ] api-reference/search/search-spaces.mdx
- [ ] api-reference/search/ask.mdx

## API Reference — Spaces

- [x] api-reference/spaces/create-space.mdx
- [x] api-reference/spaces/fetch-spaces.mdx
- [x] api-reference/spaces/fetch-space.mdx
- [x] api-reference/spaces/fetch-space-by-short-id.mdx
- [x] api-reference/spaces/fetch-space-by-slug.mdx
- [x] api-reference/spaces/fetch-user-spaces.mdx
- [x] api-reference/spaces/check-slug.mdx
- [x] api-reference/spaces/update-space.mdx
- [x] api-reference/spaces/delete-space.mdx
- [x] api-reference/spaces/fetch-children.mdx
- [x] api-reference/spaces/fetch-breadcrumb.mdx
- [x] api-reference/spaces/digest/fetch-digest-config.mdx
- [x] api-reference/spaces/digest/update-digest-config.mdx

## API Reference — Space Membership

- [x] api-reference/spaces/membership/join-space.mdx
- [x] api-reference/spaces/membership/leave-space.mdx
- [x] api-reference/spaces/membership/check-my-membership.mdx
- [x] api-reference/spaces/membership/list-members.mdx
- [x] api-reference/spaces/membership/list-team.mdx
- [x] api-reference/spaces/membership/approve-member.mdx
- [x] api-reference/spaces/membership/decline-member.mdx
- [x] api-reference/spaces/membership/ban-member.mdx
- [x] api-reference/spaces/membership/unban-member.mdx
- [x] api-reference/spaces/membership/update-member-role.mdx

## API Reference — Space Rules

- [x] api-reference/spaces/rules/list-rules.mdx
- [x] api-reference/spaces/rules/fetch-rule.mdx
- [x] api-reference/spaces/rules/create-rule.mdx
- [x] api-reference/spaces/rules/update-rule.mdx
- [x] api-reference/spaces/rules/delete-rule.mdx
- [x] api-reference/spaces/rules/reorder-rules.mdx

## API Reference — Space Moderation

- [x] api-reference/spaces/moderation/moderate-entity.mdx
- [x] api-reference/spaces/moderation/moderate-comment.mdx
- [x] api-reference/spaces/moderation/moderate-message.mdx
- [x] api-reference/spaces/moderation/handle-entity-report.mdx
- [x] api-reference/spaces/moderation/handle-comment-report.mdx
- [x] api-reference/spaces/moderation/handle-message-report.mdx

## API Reference — Space Chat

- [x] api-reference/spaces/chat/get-space-conversation.mdx

## API Reference — Storage

- [ ] api-reference/storage/upload-image.mdx
- [ ] api-reference/storage/upload-file.mdx
- [ ] api-reference/storage/fetch-file.mdx
- [ ] api-reference/storage/delete-file.mdx

## API Reference — Utils

- [ ] api-reference/utils/get-metadata.mdx

## API Reference — Crypto

- [ ] api-reference/crypto/sign-testing-jwt.mdx

## Data Models

- [ ] data-models/user.mdx
- [ ] data-models/entity.mdx
- [ ] data-models/comment.mdx
- [x] data-models/space.mdx
- [x] data-models/space-member.mdx
- [x] data-models/conversation.mdx
- [x] data-models/conversation-member.mdx
- [x] data-models/chat-message.mdx
- [ ] data-models/collection.mdx
- [ ] data-models/rule.mdx
- [ ] data-models/reaction.mdx
- [ ] data-models/file.mdx
- [ ] data-models/follow.mdx
- [ ] data-models/connection.mdx
- [ ] data-models/app-notification.mdx

## Components — Overview

- [ ] components/overview.mdx

## Components — Installation

- [ ] components/installation/cli-setup.mdx
- [ ] components/installation/configuration.mdx
- [ ] components/installation/quick-start.mdx

## Components — Component Types

- [ ] components/components/overview.mdx

## Components — Threaded Comments

- [ ] components/components/threaded/overview.mdx
- [ ] components/components/threaded/installation.mdx
- [ ] components/components/threaded/props-api.mdx
- [ ] components/components/threaded/file-structure.mdx
- [ ] components/components/threaded/features.mdx

## Components — Social Comments

- [ ] components/components/social/overview.mdx
- [ ] components/components/social/installation.mdx
- [ ] components/components/social/props-api.mdx
- [ ] components/components/social/file-structure.mdx
- [ ] components/components/social/features.mdx

## Components — Notification Control

- [ ] components/components/notifications/overview.mdx
- [ ] components/components/notifications/installation.mdx
- [ ] components/components/notifications/props-api.mdx
- [ ] components/components/notifications/customization.mdx
- [ ] components/components/notifications/integration-examples.mdx

## Components — Customization

- [ ] components/customization/overview.mdx
- [ ] components/customization/colors-theming.mdx
- [ ] components/customization/layout-structure.mdx
- [ ] components/customization/styling-variants.mdx
- [ ] components/customization/adding-features.mdx
- [ ] components/customization/advanced.mdx

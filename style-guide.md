# Docs v7 — Style Guide

This guide defines the patterns and conventions for all pages in the `docs-v7/` site. Agents writing content in Phases 3 and 4 must follow these patterns.

---

## General Rules

1. **No H1 heading that repeats the title.** Mintlify renders the frontmatter `title` automatically as an H1. Do not add `# Title` at the top of the page.
2. **Headers follow H2 → H3 → H4 hierarchy.** Never skip levels.
3. **Use Mintlify callout components** (`<Note>`, `<Warning>`, `<Tip>`) — not bold text or blockquotes.
4. **Use `<Steps>` + `<Step>` for sequential instructions** — not numbered markdown lists.
5. **Use `<CardGroup>` + `<Card>` for feature overview grids** on overview/index pages.
6. **Descriptions are accurate to source code**, not inferred. Read the controller/hook before writing.
7. **Write in American English.** Use clear, direct language.
8. **Import paths in code examples use `@replyke/react-js`** (web) or `@replyke/react-native`** (native) as appropriate. Core-only imports use `@replyke/core`.

---

## Frontmatter

Every MDX file must have frontmatter:

```mdx
---
title: "Page Title"
description: "One sentence describing what this page covers."
---
```

For API endpoint pages, add the `api` field:

```mdx
---
title: "Sign In"
api: "POST /:projectId/api/v7/auth/sign-in"
description: "Authenticate a user with email and password"
---
```

- The `api` field enables the interactive playground in Mintlify.
- Use the v7 base path: `/:projectId/api/v7/`

---

## Page Pattern: API Endpoint

Best example from v6: `docs/api-reference/auth/sign-in.mdx`

```mdx
---
title: "Sign In"
api: "POST /:projectId/api/v7/auth/sign-in"
description: "Authenticate a user using email and password"
---

One-sentence description that restates the endpoint's purpose with any important context.

## Body Parameters

<ParamField body="email" type="string" required>
  User's registered email address.
</ParamField>

<ParamField body="password" type="string" required>
  User's password.
</ParamField>

## Response

<ResponseField name="accessToken" type="string">
  JWT access token for authenticating API requests. Expires in 30 minutes.
</ResponseField>

<ResponseField name="user" type="object">
  <Expandable title="properties">
    <ResponseField name="id" type="string">
      Unique user identifier (UUID).
    </ResponseField>
    ...
  </Expandable>
</ResponseField>

## Error Responses

<AccordionGroup>
  <Accordion title="Missing Fields — 400 Bad Request">
    ```json
    {
      "error": "Email and password are required.",
      "code": "auth/missing-fields"
    }
    ```
  </Accordion>
</AccordionGroup>
```

### ParamField attributes

| Attribute | Values | Notes |
|-----------|--------|-------|
| `body` | field name | For request body fields |
| `query` | field name | For query string params |
| `path` | field name | For URL path params |
| `type` | `string`, `number`, `boolean`, `object`, `array`, etc. | |
| `required` | (flag, no value) | Only for required fields |

### ResponseField notes
- Use `<Expandable title="properties">` for nested objects.
- List every field developers can expect to receive.
- Mark optional fields implicitly by omitting "required" from the description.

---

## Page Pattern: SDK Hook (Hooks Reference tab)

Best example from v6: `docs/hooks/users/use-fetch-user.mdx`

```mdx
---
title: "useFetchUser"
description: "Fetch a user's public profile by their ID"
---

## Overview

Brief description of what this hook does and when to use it. One or two sentences maximum.

## Usage Example

```tsx
import { useFetchUser } from "@replyke/react-js";

function UserProfile({ userId }: { userId: string }) {
  const fetchUser = useFetchUser();

  const handleFetch = async () => {
    const user = await fetchUser({ userId });
    console.log(user);
  };

  return <button onClick={handleFetch}>Load Profile</button>;
}
```

## Parameters

The hook returns a function. That function accepts:

<ParamField path="userId" type="string" required>
  The ID of the user to fetch.
</ParamField>

## Returns

<ResponseField name="User" type="User">
  The public profile of the fetched user. See [User data model](/data-models/user).
</ResponseField>
```

### Notes on hook pages
- If a hook is a context hook (returns state, not a callable function), describe the return values as state fields, not as a function's return.
- If a hook requires a provider in the tree, add a `<Note>` at the top:
  ```mdx
  <Note>This hook must be used inside a `ConversationProvider`.</Note>
  ```
- Link back to the relevant data model page for complex return types.

---

## Page Pattern: SDK Integration Guide (SDK Reference tab)

These pages explain how to use a feature end-to-end. They cover multiple hooks together in a narrative flow.

```mdx
---
title: "Setting Up Chat"
description: "Configure ChatProvider and ConversationProvider for real-time messaging"
---

Brief intro paragraph explaining what this guide covers and what the developer will be able to do after following it.

## Prerequisites

<Note>Chat requires `ChatProvider` to be placed in the component tree above any chat UI.</Note>

## Step 1: Wrap with ChatProvider

<Steps>
  <Step title="Install the provider">
    Place `ChatProvider` inside `ReplykeProvider` but outside your conversation components:

    ```tsx
    import { ChatProvider } from "@replyke/react-js";

    function App() {
      return (
        <ReplykeProvider projectId="your-project-id">
          <ChatProvider>
            {/* your app */}
          </ChatProvider>
        </ReplykeProvider>
      );
    }
    ```
  </Step>
  <Step title="Verify the connection">
    ...
  </Step>
</Steps>

## Next Steps

<CardGroup cols={2}>
  <Card title="Conversations" href="/sdk/chat/conversations">
    Create and list conversations
  </Card>
  <Card title="Messages" href="/sdk/chat/messages">
    Send and receive messages
  </Card>
</CardGroup>
```

---

## Page Pattern: Data Model

Best example from v6: `docs/data-models/user.mdx`

```mdx
---
title: "ChatMessage"
description: "The shape of a message object returned by the Chat API and SDK"
---

Brief description of what this data type represents and where it appears.

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string` | Unique message identifier (UUID). |
| `conversationId` | `string` | ID of the conversation this message belongs to. |
| `content` | `string \| null` | Message text content. |
| `clientId` | `string \| null` | Client-generated ID for optimistic deduplication. |
| `createdAt` | `Date` | Timestamp when the message was sent. |

## Nested Types

### QuotedMessage

When a message quotes another, the `quotedMessage` field contains:

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string` | ID of the quoted message. |
| `content` | `string \| null` | Text content of the quoted message. |
```

### Notes on data model pages
- Use markdown tables for flat field lists.
- Use `###` subsections for nested types.
- The source of truth is `monorepo/packages/core/src/interfaces/` — not the Sequelize models.
- Note which fields are optional (use `T | null` or `T | undefined` in the Type column).

---

## Page Pattern: Overview / Guide Page

For feature overview pages in the SDK Reference tab or the top-level Overview tab.

```mdx
---
title: "Spaces"
description: "Build community spaces with membership, permissions, moderation, and chat"
---

One paragraph introducing the feature: what it does and the main use case.

## Key Concepts

<CardGroup cols={2}>
  <Card title="Membership" icon="users" href="/sdk/spaces/membership">
    Join/leave spaces, approval workflows, role management
  </Card>
  <Card title="Moderation" icon="shield" href="/sdk/spaces/moderation">
    Approve or remove content, handle reports
  </Card>
  <Card title="Rules" icon="list" href="/sdk/spaces/rules">
    Community rules displayed to members
  </Card>
  <Card title="Digest" icon="envelope" href="/sdk/spaces/digest-newsletter">
    Periodic content summaries delivered to a webhook
  </Card>
</CardGroup>

## Core Architecture

Explanation of the main provider/hook pattern used for this feature.

```tsx
// Example showing the key integration pattern
```

## What's Next

Link to the first implementation page.
```

---

## Mintlify Components Quick Reference

### Callouts

```mdx
<Note>Use for important information the developer needs to know.</Note>

<Warning>Use for things that could break the integration or cause data loss.</Warning>

<Tip>Use for optional but helpful advice.</Tip>
```

### Steps

```mdx
<Steps>
  <Step title="Step title">
    Step content here.
  </Step>
  <Step title="Next step">
    More content.
  </Step>
</Steps>
```

### Card Groups

```mdx
<CardGroup cols={2}>
  <Card title="Card Title" icon="icon-name" href="/path/to/page">
    One sentence description.
  </Card>
</CardGroup>
```

Icons come from Font Awesome. Common choices: `bolt`, `users`, `shield`, `envelope`, `code`, `gear`, `magnifying-glass`, `comments`, `bell`, `folder`, `key`, `lock`, `circle-check`, `arrow-right`.

### Expandable (for nested response fields)

```mdx
<ResponseField name="user" type="object">
  <Expandable title="properties">
    <ResponseField name="id" type="string">
      Unique identifier.
    </ResponseField>
  </Expandable>
</ResponseField>
```

### Accordion Groups (for error responses)

```mdx
<AccordionGroup>
  <Accordion title="Not Found — 404">
    ```json
    { "error": "Resource not found.", "code": "feature/not-found" }
    ```
  </Accordion>
</AccordionGroup>
```

### Code Groups (for multi-language examples)

```mdx
<CodeGroup>
  ```tsx React
  import { useSendMessage } from "@replyke/react-js";
  ```

  ```tsx React Native
  import { useSendMessage } from "@replyke/react-native";
  ```
</CodeGroup>
```

---

## Cross-linking Conventions

- API endpoint pages should link to corresponding SDK hook pages: `See also: [useSendMessage](/hooks/chat/messages/use-send-message)`
- Hook pages should link to the SDK guide for their feature: `For integration guidance, see [Chat Messages](/sdk/chat/messages)`
- Hook pages should link to data model pages for complex return types: `Returns a [ChatMessage](/data-models/chat-message) object`
- Data model pages should list the API endpoints and hooks that return that type

---

## What NOT to Do

- Do not add a `# Title` H1 that repeats the frontmatter title
- Do not use `>` blockquotes for callouts — use `<Note>`, `<Warning>`, `<Tip>`
- Do not use numbered markdown lists for sequential steps — use `<Steps>`
- Do not guess behavior — read the source code
- Do not copy from v6 docs — verify everything against v7 source
- Do not add commentary, opinions, or marketing copy — docs are factual and neutral

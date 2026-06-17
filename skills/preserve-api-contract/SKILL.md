---
name: preserve-api-contract
description: Use when modifying any backend route, controller, or service that returns a response — prevents silent API contract breakage that crashes frontend consumers.
license: MIT
metadata:
  author: BhaskarDey772
  version: "1.0"
---

# Preserve API Contract

## Overview

Before touching any backend route or service, snapshot the current response shape. After editing, compare field-by-field. If the shape changed, **stop and flag it explicitly** — do not silently apply the change.

Silent response shape changes (renamed fields, removed fields, changed types, restructured nesting) break frontend consumers with no error at the point of change.

## When to Use

Any edit to a file that contains:
- `res.json(`, `res.send(` in a route/controller
- `return { ... }` in a service that feeds a route
- A Mongoose/ORM query whose result flows into a response
- A response builder, serializer, or DTO

## The Protocol

### Step 1 — Snapshot BEFORE editing

Read the current endpoint. Write the exact response shape inline as a comment:

```js
// SNAPSHOT: POST /api/order/fulfill
// {
//   success: Boolean,
//   data: {
//     orderId: String,
//     status: String,
//     merchant: { _id: String, name: String }
//   }
// }
```

Include: field names, types, nesting level, array vs object.

### Step 2 — Edit

Make your changes.

### Step 3 — Compare AFTER editing

Re-read your changed code and produce the new response shape. Compare field-by-field:

| Field | Before | After | Status |
|-------|--------|-------|--------|
| `data.orderId` | String | String | ✅ same |
| `data.merchant` | Object | — | 🚨 REMOVED |
| `data.merchantId` | — | String | ⚠️ ADDED |

### Step 4 — If shape changed, flag and wait

Do NOT silently apply breaking changes. State clearly before proceeding:

> **API Contract Change Detected**
> - Removed: `data.merchant` (object)
> - Added: `data.merchantId` (string)
> - Consumers using `data.merchant.name` will break.
>
> Approve this change, or I will revert to the original shape.

Wait for explicit user approval before committing a breaking change.

## What Counts as Breaking

| Change | Breaking? |
|--------|-----------|
| Rename a field | 🚨 YES |
| Remove a field | 🚨 YES |
| Change type (`String` → `Number`) | 🚨 YES |
| Flatten nested object | 🚨 YES |
| Wrap value in array | 🚨 YES |
| Add new optional field | ✅ Safe |
| Add new nested object | ✅ Safe |

## Common Rationalizations — All Wrong

| Excuse | Reality |
|--------|---------|
| "I'm just refactoring internals" | If the response shape changes, it's a contract change. |
| "The field name is cleaner now" | Cleaner names break consumers. Flag it first. |
| "It returns the same data" | Same data in a different shape is still breaking. |
| "The frontend probably doesn't use that field" | "Probably" crashes apps. Snapshot and compare. |
| "This is a minor restructure" | Minor restructures cause major crashes. No exceptions. |

## Red Flags — STOP and Snapshot

You are about to violate this rule if you think:
- "I'll just rename this field while I'm here"
- "This restructuring won't affect the output"
- "I'll clean up the response shape too"
- "This is just an internal change"

All of these mean: **snapshot first → edit → compare → flag if changed → wait for approval.**

# Campus Administrator Privileges for sahniadit@gmail.com

Grant comprehensive administrator privileges exclusively to `sahniadit@gmail.com` across Findr, enabling full editing, status overrides, and listing management for all items in the campus registry.

### User Review & Critical Decisions

> [!IMPORTANT]
> The user confirmed the permission model: Admin privileges are granted exclusively to `sahniadit@gmail.com`. Non-admin accounts retain standard user permissions (only editing their own listings).

- **Admin Account**: `sahniadit@gmail.com` (Adit Sahni)
- **Privilege Scope**:
  - Edit any item listing across campus (title, category, status, location, date, description, image, and contact info).
  - Mark any item as Reunited or Delete invalid/spam listings.
  - Access an "All Campus Listings (Admin)" view in the Dashboard to manage the entire registry.
  - Distinctive yet tasteful "Campus Admin" badge displayed in the profile and navigation.

---

### 1. Overview & Core Concept

- **What It Does**: Identifies whether the current user is `sahniadit@gmail.com`. If true, the system grants administrator mode, unlocking the "Edit Listing" and "Delete" actions on all item cards and modals across the application.
- **Target Audience**: Adit Sahni (`sahniadit@gmail.com`) as the campus registry administrator and moderator.
- **Key Value**: Provides effortless governance and maintenance of all campus lost & found listings without opening unrestricted edit access to general visitors.

---

### 2. User Experience & Visual Design

- **Key User Flows**:
  1. **Item Details Modal**:
     - When viewing any item (even posted by Marcus, Elena, or others), `sahniadit@gmail.com` sees an active **"Edit Listing"** button alongside an "Admin" badge.
     - Clicking opens the full `EditItemModal`, allowing immediate modification of any field.
     - Saving persists updates to `localStorage` and triggers the success toast notification.
  2. **Dashboard Management**:
     - Profile header features an **"Admin"** pill: `Admin · sahniadit@gmail.com`.
     - In the listings filter, an **"All Campus Listings"** tab is active for the admin, allowing inline editing, reunion marking, or removal of any listing.
  3. **Non-Admin View**:
     - Other users only see contact and share buttons on listings they did not create.

- **Visual Identity & Theme**:
  - Consistent Apple-inspired design language: clean lines, refined typography, and subtle monochromatic pill with cherry-red accent dot (`#C1122F`).
  - Tactile buttons with subtle micro-interactions and smooth modal transitions.

---

### 3. Key Product Decisions & Trade-Offs

- **Decision 1: Email-Based Admin Evaluation vs. Hardcoded Global Access**
  - *Chosen Approach*: Implement a reactive helper `isAdmin` evaluated directly as `userProfile.email.toLowerCase() === 'sahniadit@gmail.com'`.
  - *Why*: Directly aligns with the user's explicit instruction while ensuring permissions are clean, predictable, and simple to audit.

- **Decision 2: Seamless Shared Edit Modal**
  - *Chosen Approach*: Reuse the existing `EditItemModal` for admin edits rather than creating a separate redundant form.
  - *Why*: Maintains UX consistency and code efficiency.

---

### 4. Technical Architecture & Component Mapping

```
┌─────────────────────────────────────────────────────────────┐
│                       ItemsContext                          │
│   - userProfile: { email: 'sahniadit@gmail.com', ... }      │
│   - isAdmin: boolean (email === 'sahniadit@gmail.com')      │
│   - canEditItem(item): isOwner || isAdmin                   │
│   - updateItem(id, updates), deleteItem(id), etc.           │
└──────────────┬──────────────────────────────┬───────────────┘
               │                              │
               ▼                              ▼
┌─────────────────────────────┐  ┌────────────────────────────┐
│      ItemDetailsModal       │  │       DashboardPage        │
│  - Checks canEditItem()     │  │  - Shows Admin badge       │
│  - Shows Edit button for    │  │  - "All Listings" tab with │
│    admin on any post        │  │    inline Edit & Delete    │
└──────────────┬──────────────┘  └────────────┬───────────────┘
               │                              │
               └──────────────┬───────────────┘
                              ▼
               ┌──────────────────────────────┐
               │        EditItemModal         │
               │  - Pre-fills item data       │
               │  - Updates live state        │
               └──────────────────────────────┘
```

- **Interactive State Transitions**:
  - `ItemsContext.tsx`: Expose `isAdmin: boolean` and `canEditItem: (item: Item) => boolean`.
  - `ItemDetailsModal.tsx`: Replace `isOwner` restriction on Edit/Delete buttons with `canEditItem(selectedItem)`.
  - `DashboardPage.tsx`: Display admin badge and enable managing all campus listings for `sahniadit@gmail.com`.

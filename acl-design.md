
# Crestline Asset Request — ACL Security

## Overview

The `Crestline Asset Request` (`u_crestline_asset_request`) table controls access using role-based ACLs. Access was configured, then verified by impersonating each test user and confirming the expected behavior directly on the table.

## Access Model

| User | Role      | Create | Read | Update | Delete |
|------|-----------|:------:|:----:|:------:|:------:|
| Maya | `itil`    | ✅     | ✅   | ✅    | ❌   |
| Riya | —         | ❌     | ❌   | ❌    | ❌   |
| Sam  | —         | ❌     | ❌   | ❌    | ❌   |

## ACL Configuration

| Operation | Required role | Reason |
|-----------|---------------|--------|
| Create    | `itil`        | Only IT staff should be able to log new asset requests directly on the table. |
| Read      | `itil`        | Request details are limited to the support team handling them. |
| Write/Update | `itil`     | Only IT staff should change request status, priority, or assignment. |
| Delete    | Restricted (no role granted) | Requests should be closed rather than deleted, to preserve an audit trail. |

## Module Access

The **Crestline Asset Requests** module is restricted to users with the `itil` role.

**Override application menu roles** was enabled on the module so that `itil` users can open it without needing access to the parent **System Definition** menu. This keeps IT support users scoped to just the module they need.

## Testing

Each scenario below was verified by impersonating the user and attempting the action directly, not just by inspecting the ACL configuration.

- **Maya (`itil`):** confirmed she can create, read, and update records.
- **Delete:** tested with the delete role granted and then removed; access changed as expected in both directions.
- **Riya (no role):** confirmed no module access and no ability to view or create records.
- **Sam (no role):** confirmed no module access and no ability to view or create records.
- ACL behavior was deliberately modified and then restored to its original state after each test, to confirm the configuration change was actually responsible for the access difference (not a caching or session artifact).

## Next steps (planned)

- Add a **Manager** role: read-only access to all requests, no create/update/delete.
- Add an **Employee/requester** access level: create access, plus read access limited to records where `Requested for` equals the current user.
- Add a field-level ACL on **Priority** so only `itil` can edit it after a request is created, while requesters can still view it.

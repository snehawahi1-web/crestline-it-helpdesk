# UI Policies — Crestline Asset Request

## Objective

Configure and test UI Policies on the Crestline Asset Request table to dynamically control field behavior based on the selected Status and Request type.

## UI Policy 1: Closed Requires Description

**Condition:** Status is Closed

### UI Policy Action

| Field | Configuration |
|---|---|
| Description | Mandatory = True |

**Reverse if false:** Enabled

### Testing Results

- Changed Status to Closed and verified that Description became mandatory.
- Cleared Description and confirmed that ServiceNow displayed a mandatory-field validation message identifying Description.
- Changed Status back to Open and verified that the mandatory requirement was reversed.

**Result:** Passed.

## UI Policy 2: Hardware Issue Shows Asset

**Condition:** Request type is Hardware Issue

### UI Policy Action

| Field | Configuration |
|---|---|
| Asset | Visible = True |

**Reverse if false:** Enabled

### Testing Results

- Selected Hardware Issue and verified that the Asset field was visible.
- Changed Request type to Software Issue and verified that the Asset field became hidden.
- Confirmed that the Asset field's visibility changed according to the selected Request type.

**Result:** Passed.

## Non-Admin Testing

Both UI Policies were tested while impersonating Maya, a non-admin IT support user with the `itil` role.

- **Closed Requires Description:** Description became mandatory when Status was set to Closed.
- **Hardware Issue Shows Asset:** Asset was visible for Hardware Issue and hidden for Software Issue.

Both policies behaved as expected for Maya.

### Access Control Observation

Sam Employee could not access the Crestline Asset Request list under the existing ACL configuration, so non-admin UI Policy testing was performed using Maya, who had the required table access through the `itil` role.

## Key Concepts Learned

- UI Policies control form behavior based on conditions.
- One UI Policy can contain multiple UI Policy Actions.
- UI Policy Actions can control field visibility, mandatory status, and read-only behavior.
- Reverse if false reverses the configured UI Policy Actions when the condition is no longer met.
- UI Policies improve the user experience but do not replace server-side security or data validation.
- Non-admin testing requires a test user who actually has table access; the ACL design from an earlier phase determined which user could be used here.

## Outcome

Two UI Policies were configured and tested on the Crestline Asset Request table to enforce a required description for closed requests and display the Asset field for hardware issues. Both policies were verified under both admin and non-admin (`itil`-role) impersonation.

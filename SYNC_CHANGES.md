# API Sync Behavior Changes

## Summary

Automatic API syncing has been **disabled by default**. The system now only syncs data with the Slide API in two scenarios:

1. **Before sending scheduled emails** - The system will automatically sync data before generating and sending each scheduled report email
2. **When manually triggered** - Users can click the "Sync Now" button on the dashboard to manually sync data at any time

Additionally, **audit log syncing has been optimized** to never re-download existing audit logs, since audit logs are immutable and never change once created.

## What Changed

### 1. Default Auto-Sync Setting Changed to OFF

**Files Modified:**
- `lib/scheduler.py` - Changed default from 'true' to 'false' (line 75)
- `lib/database.py` - Changed initial database preference from 'true' to 'false' (line 92)
- `app.py` - Updated API endpoints to use 'false' as default (lines 2130, 2140)
- `lib/admin_utils.py` - Updated admin utility default to 'false' (line 84)

**Impact:**
- New users will have auto-sync disabled by default
- Existing users who have already set their preference will not be affected
- Users can still enable automatic syncing via the dashboard if they prefer

### 2. Sync Before Scheduled Emails

**File Modified:**
- `lib/email_scheduler.py` - Added `_sync_before_email()` method and integrated it into `_execute_schedule()`

**How It Works:**
1. When a scheduled email is due, the system first triggers a sync with the Slide API
2. The sync retrieves the latest data (from the last 90 days)
3. If sync is already in progress, the system waits up to 5 minutes for it to complete
4. If sync fails or times out, the email is still sent using existing data
5. After successful sync, the email report is generated and sent with the most up-to-date information

**Benefits:**
- Ensures scheduled reports always contain the most current data
- Reduces unnecessary API calls when no emails are due
- Provides better control over when syncing occurs

### 3. Manual Sync Still Available

**No Changes Required:**
- The "Sync Now" button on the dashboard continues to work as before
- Users can manually trigger a sync at any time
- Manual sync is useful for viewing updated data in the dashboard without waiting for a scheduled email

### 4. Audit Log Sync Optimization

**File Modified:**
- `lib/sync.py` - Added `_get_latest_audit_time()` method and optimized audit log fetching

**How It Works:**
1. Before syncing audit logs, the system queries the database for the most recent audit log timestamp
2. If audit logs already exist, only fetch logs newer than the latest one we have
3. If no audit logs exist yet, use the standard 90-day cutoff date
4. This prevents re-downloading immutable audit logs that never change

**Benefits:**
- Significantly reduces API calls and bandwidth for audit logs
- Faster sync times after the initial sync
- More efficient use of resources
- Audit logs are immutable, so there's no data loss risk

## User Experience

### For New Users
- Auto-sync will be OFF by default
- Data will only sync:
  - When they click "Sync Now"
  - Automatically before scheduled email reports are sent

### For Existing Users
- If they previously enabled auto-sync, it will remain enabled
- If they had auto-sync disabled, nothing changes
- They can toggle auto-sync on/off at any time via the dashboard

## Technical Details

### Sync Behavior Before Email Sending

The `_sync_before_email()` method:
1. Retrieves and decrypts the stored API key
2. Checks if a sync is already in progress
3. Starts a new sync if needed
4. Waits for sync completion (up to 5 minutes)
5. Logs appropriate messages for monitoring
6. Gracefully handles errors (email still sends if sync fails)

### Configuration

Users can still configure:
- **Auto-Sync Toggle**: Enable/disable automatic periodic syncing
- **Sync Frequency**: Choose how often to sync (1, 3, 6, 12, or 24 hours)

These settings are in the dashboard under the "Data Sync" section.

## Rollback Instructions

If you need to revert to the old behavior (auto-sync ON by default):

1. In `lib/scheduler.py` line 75, change `'false'` to `'true'`
2. In `lib/database.py` line 92, change `'false'` to `'true'`
3. In `app.py` lines 2130 and 2140, change `'false'` to `'true'`
4. In `lib/admin_utils.py` line 84, change `'false'` to `'true'`
5. Restart the application

To remove pre-email syncing:
1. Remove the `_sync_before_email()` method call from `_execute_schedule()` in `lib/email_scheduler.py`
2. Restart the application

## Testing Recommendations

1. **Test scheduled email with sync:**
   - Create a test schedule
   - Wait for it to trigger
   - Check logs to verify sync occurs before email is sent

2. **Test manual sync:**
   - Click "Sync Now" button
   - Verify data updates correctly

3. **Test auto-sync toggle:**
   - Enable auto-sync in dashboard
   - Verify sync occurs at specified intervals

4. **Test new user experience:**
   - Create a new API key/user
   - Verify auto-sync is OFF by default

5. **Test audit log optimization:**
   - Perform an initial sync (will fetch all audit logs from last 90 days)
   - Check logs for the audit log count
   - Perform a second sync immediately
   - Verify that very few (or zero) audit logs are fetched the second time
   - Check logs for message: "Skipping existing audit logs, fetching only after [timestamp]"

## Future Optimization Opportunities

The audit log optimization implemented here could potentially be applied to other immutable or append-only data sources:

- **File Restores** - Once a file restore is completed, it doesn't change
- **Image Exports** - Export records are immutable once created
- **Backups** - Individual backup records don't change (though new ones are created)

For each of these, the same pattern could be applied:
1. Query for the most recent record timestamp in the database
2. Only fetch records newer than what we already have
3. Reduce API calls and improve sync performance

This would require careful analysis of each data type to ensure no data is missed during incremental syncing.


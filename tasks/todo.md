# iPhoneImport - handoff / where we left off

Last updated: 2026-10-10. Repo: https://github.com/xmorera/iPhoneImport (branch `main`, in sync with origin).
Code is unchanged from the fork baseline; this file records the review and the agreed improvement plan.

## Pick up on another machine
1. `git clone https://github.com/xmorera/iPhoneImport.git` (or `git pull origin main`)
2. `python -m venv iivenv` ; `iivenv\Scripts\activate` ; `pip install -r requirements.txt`
3. Open Claude Code in the folder and say: "read tasks/todo.md and continue with the plan".
4. Use the same synced `--metadata-folder` on every machine.

## Files
- `__main__.py` - driver: load imported list, walk device, diff, copy via IFileOperation, write `imported_<ts>.txt`.
- `win32utils.py` - Windows Shell COM helpers (walk_dcim, copy_multiple_files, ...).

## Known problems found in review
- [ ] Omitting `--metadata-folder` crashes at the end (`os.path.join(None, ...)`), after the copy.
- [ ] Metadata is written even if the copy failed/partially failed (no result check on `PerformOperations`).
- [ ] `--skip-copy` still writes metadata, so a later real run skips those files.
- [ ] Folder-suffix hack (`_a` -> `__`) can collide and is fragile.
- [ ] Leftover debug prints in `walk_dcim`; unused `copy_single_file`; `except BaseException`; typos
      (`desination`, `get_diplay_name`); `remove_prefix` shadows `str`.
- [ ] `requirements.txt` is UTF-16; re-save as UTF-8.

## Improvement plan (target: 2026-10-24 for steps 1-4)
1. Record only files that actually copied (IFileOperationProgressSink, per-item results). Dry-run writes nothing.
2. Verify each copy (destination exists, size matches source) before marking imported.
3. Replace pile of `imported_*.txt` with one state file (SQLite/JSON) keyed by name+size+date taken.
4. Resilience: default metadata folder inside destination; copy in batches of 50-200 and record per batch;
   clean message when phone is locked/disconnected; log file + exit code.
5. Automation: Task Scheduler task on device connect or daily.
6. Cleanup + tests for pure logic (path normalization, diffing).

## Open decision
- Keep deleted-on-phone photos in the backup (recommended: yes, never delete); optionally add a report mode
  listing PC files no longer on the phone.

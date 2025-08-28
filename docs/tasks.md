- [x] 1. Strict shell mode with safe wrappers
  - Enable errexit, nounset, pipefail; add deliberate guards (e.g., || true) and central helpers so failures are surfaced without breaking intentional flows.

- [x] 2. Managed temporary workspace
  - Create a single mktemp-based TMP_DIR; place FIFO, issue_ticks, picture blocks, and other transient files inside; prune everything in one cleanup path.

- [ ] 3. Centralized issue logging
  - Provide a _log_issue(item, phase, message[, extra]) that appends standardized lines to the per-run log and updates issue_ticks; use consistently across operations.

- [ ] 4. Find-based file discovery with deterministic order
  - Use find -print0 and mapfile -d '' (optionally sort -z) to robustly gather FLACs (handles odd filenames, symlinks) and provide stable processing order.

- [ ] 5. Non-interactive/headless UI mode
  - Auto-detect TTY and support a --no-ui flag to disable cursor control/colors and print simple per-file lines while preserving full logging.

- [ ] 6. Dry-run mode
  - Add --dry-run to simulate actions; emit “Would …” messages, apply skip logic, avoid file modifications and artifact creation; show indicator in the banner.

- [ ] 7. Capability-based color initialization
  - Detect TTY and terminal capabilities (tput) to enable/disable colors automatically while still honoring an explicit -n to force disable.

- [ ] 8. Adaptive job defaults and clear CPU policy
  - Compute sensible defaults from cores (e.g., global=nproc, compress=nproc/threads with floor 1), enforce bounds, and surface starvation flags in the banner; apply threads via --threads consistently when supported.

- [ ] 9. Central flac progress/exec wrapper
  - Encapsulate flac invocations in _run_flac_with_progress to parse percentage, capture errors, and unify reporting across compress/test/decode.

- [ ] 10. ReplayGain precheck optimization
  - Quickly detect whether an album already has all ReplayGain tags before invoking add-replay-gain to skip no-op work.

- [ ] 11. Safer, faster retagging pipeline
  - Extract all tags once via --export-tags-to=-, filter/normalize in-memory, then atomically write with --remove-all-tags and --import-tags-from=-.

- [ ] 12. Process-group-aware job termination
  - Send signals to job process groups (e.g., kill -TERM -- "-$pid") or escalate with a short timeout to ensure children/grandchildren are cleaned.

- [ ] 13. mktemp-backed WAV/artwork staging
  - Create temp WAV directories and artwork staging areas with mktemp; track and remove them reliably on exit or interruption.

- [ ] 14. Nullglob safety
  - Enable nullglob in contexts where unmatched globs may occur to avoid unintended literal expansions.

- [ ] 15. Optional headless parallel scheduler
  - Provide a wait -n based scheduler for non-TUI runs (or when GNU parallel is available), maintaining fixed concurrency without the FIFO UI machinery.
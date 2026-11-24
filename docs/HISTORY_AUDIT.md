# Public portfolio history audit

The workflow scans the reachable history of the 16 repositories that were public when the audit list was created. No private repository is cloned. A mirror clone includes advertised branches and tags; inaccessible, deleted, unreachable objects, external forks, binary archives and provider records are outside the guarantee. An empty repository is not scanned.

The official Gitleaks container is pinned to a version tag. During scanning the container has no network access, a read-only repository mount and no credentials supplied by this workflow. Output is limited to status and counts in fixed, broad rule categories. The scanner writes a redacted temporary report; only aggregate counts are emitted, never raw findings, paths, secret values, rule-specific metadata or reports. The temporary directory is deleted on job exit. Findings are potential secrets, not proof that credentials are still valid. Review privately using a local clone. Revoke exposed credentials first; deleting them from current files does not revoke them. No history rewrite is performed by the workflow.

A successful scan does not prove the absence of secrets. If any job fails because of clone, runner or scanner errors, treat the result as unavailable, not clean. The workflow runs only on its own changes or manual dispatch, not on a schedule.

## Remediation order

1. Identify findings locally in a private working copy using a redacted scanner report.
2. Confirm whether each item is a real credential, a test fixture or a public identifier. Do not test credentials against live services merely to determine validity.
3. Revoke and rotate genuine exposed credentials through the issuing provider; update deployment secret storage separately.
4. Remove live values from the current code, and evaluate whether a coordinated history rewrite is necessary. Do not force-push shared branches without explicit approval.
5. Repeat the scan after remediation; keep a private record of unresolved findings. A previously identified token must be revoked even if a scanner misses it.

The additional snapshot job scans only the current default branch of the six flagged repositories and the previously identified Telegram bot repository. The purpose is to distinguish current-tree findings from history-only findings, not to certify other branches or validate credentials. Snapshot findings are reported with the same fixed categories and no locations or values.

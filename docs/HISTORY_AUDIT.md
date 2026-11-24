# Public portfolio history audit

The workflow scans the reachable history of the 16 repositories that were public when the audit list was created. No private repository is cloned. A mirror clone includes advertised branches and tags; inaccessible, deleted, unreachable objects, external forks, binary archives and provider records are outside the guarantee. An empty repository is not scanned.

The official Gitleaks container is pinned to a version tag. During scanning the container has no network access, a read-only repository mount and no credentials supplied by this workflow. Output is reduced to a status; raw findings, paths, secret values and reports are never printed or uploaded. Findings are potential secrets, not proof that credentials are still valid. Review privately using a local clone. Revoke exposed credentials first; deleting them from current files does not revoke them. No history rewrite is performed by the workflow.

A successful scan does not prove the absence of secrets. If any job fails because of clone, runner or scanner errors, treat the result as unavailable, not clean. The workflow runs only on its own changes or manual dispatch, not on a schedule.

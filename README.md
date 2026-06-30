cfgmgr:  
- Merge/Delete kubeconfig file to ~/.kube/config 
 
secrets:
- Base64 decode secrets data

entry:
- kubectl exec bash/sh in container
- `kubectl entry <fzf query term> [-n namespace] [-c container]`

log:
- kubectl logs with fzf
- `kubectl log <fzf query term> [-f] [-p] [-n namespace] [-c container] ...`

desc:
- kubectl describe with fzf
- `kubectl desc [resource] [fzf query term]`

g: 
- kubectl get with fzf
- `kubectl desc [resource] [fzf query term] [options]`

diff-rs:
- kubectl diff-rs <replicaset>
- kubectl diff-rs <replicaset> LAST LAST-3

rs:
- A Deployment's whole rollout history: ReplicaSets oldest->newest, then a diff of every CONSECUTIVE revision (each block = what that rollout changed). diff-rs compares TWO; this walks all retained revisions.
- `kubectl rs <deployment> [-n NS | -A]`  (single token — `kubectl-rs`; kubectl reads each dash as a word boundary, so a `kubectl-rs-timeline` name would dispatch as `kubectl rs timeline`)
- filters by ownerReferences (correct lineage, no cross-deployment interleaving); sanitizes via `kubectl neat` (--no-neat for jq scrub, --raw for full yaml)
- diffs render with `dyff` (semantic, path-addressed) when installed, else colorized `diff -u`; force the latter with `--plain`
- auto-pages on a TTY via `$PAGER` (else `less -RF`, else `bat`); piped/redirected output is never paged; `-P`/`--no-pager` to disable
- `--no-diff`: timeline table only
- unknown flags / anything after `--` pass through to `kubectl get rs` (--context, --kubeconfig, -l ...)

recreate:  
kubectl recreate -f <file>

cordonx:
- kubectl cordonx <node1> [node2 ...]
- kubectl cordonx --undo

find:
- kubectl find <resource> <pattern1> [pattern2] [...]
- kubectl find po -i
- kubectl find po -s

vatt: 
- pvc to volumeattachment
- kubectl vatt <pvc_name_pattern>

prune:
- Force-delete pods whose NAME matches a pattern, with confirmation
- `kubectl prune <pod-name-pattern> [-n NAMESPACE | -A]`
- `kubectl prune --all [-n NAMESPACE | -A]` — delete all UNHEALTHY pods (READY not X/X or STATUS not Running/Completed/Succeeded)
- default: current namespace; always --force --grace-period 0

fdrain:
- Drain node(s), force-deleting pods the eviction API can't move because a PodDisruptionBudget would be violated (drain --force does NOT bypass PDBs)
- Only acts on ALREADY-CORDONED (SchedulingDisabled) nodes — cordon first; uncordoned matches are skipped. Prompts once for the destructive op
- `kubectl fdrain <node> [node ...]` — runs `kubectl drain --delete-emptydir-data --ignore-daemonsets` (defaults) per node in parallel, then force-deletes (--force --grace-period 0) any pod still pinned by a zero-allowance PDB
- `kubectl fdrain -l <selector>` — target every cordoned node matching a label selector (e.g. `-l node.info/kubeletVersion=v1.28`)
- `kubectl fdrain -y ...` — skip the destructive-op and per-pod confirmations
- unrecognized flags (e.g. `--force`) pass through to `kubectl drain`; options: --poll, --grace (let eviction run first), --timeout

force-sync:
- Annotate an ExternalSecret with force-sync=<epoch> to trigger an immediate sync
- `kubectl force-sync [-n NS] [query]`
- picks the ExternalSecret via fzf (fuzzy match on the optional query)

grep:
- `kubectl get <resource> | grep <query>` shortcut; query matches any column
- kubectl grep <resource>[/<query>] [query]
- kubectl grep pod 7d                  (any column: NAME, STATUS, AGE, ...)
- kubectl grep 'pod*' nginx            (multi-resource via resource glob, parallel)
- kubectl grep '*/nginx'               (any resource, row contains 'nginx')
- -c / --compact: `<ns>\t<r>/<name>` per line (awk/xargs friendly)
- -nr / --not-ready: drop healthy rows (Completed|Succeeded|X/X)
- -E / --regex: treat query as raw regex (anchors, quantifiers)
- unknown flags pass through to kubectl get (-owide, --show-labels, ...)
# Documentation CI and Windows readiness repair — 2026-10-06

## Objective and status

Correct my unnecessary documentation-only push and the newly exposed Windows
readiness and macOS Intel MCP timeout fixtures. Record Alex's requirement to include documentation with
implementation before CI, without an automatic post-release documentation push.

**In progress: two slow-start regressions reproduced the defect; all thirteen
corrected command-settlement cases and ten workflow/release-policy checks passed
locally. Native Windows/macOS focused and corrected exact-main acceptance are pending.**

Owning [RRC PRD](../Agent_Vesper_Release_Recovery_Controller_PRD.md).
The [release report](2026-10-06-rrc-parity-production-release.md) retains its
immutable `7e837db3` source scope: one `v0.24.5` publication, eleven green
prerequisites, seven green producing jobs, sixteen verified assets and native
`Complete`. No second release is created.

## Cause and retained evidence

- I separately pushed documentation commit
  `3df2feb69dc120b13d02a143f5df6aba8536d8d1`. All four main-push workflows ran
  unconditionally, unnecessarily launching program suites for the report.
- Foundation run `37420009037`, Windows job `112127036862`, failed
  `descendant_writing_after_leader_exit_is_cleaned` and
  `leader_exit_does_not_wait_for_descendant_held_pipe` before `settlement.ready`.
  Both report command timeout, exit code 1 and verified cleanup; nine other cases
  passed. This is distinct from the corrected xAI SSE fixture.
- The readiness helper allows fifteen seconds, but those two invocation sites
  imposed a five-second overall command timeout, killing the fixture before
  the allowed startup period ended.
- Alex rejected documentation-triggered suites. Canonical `37420009038`, MSRV
  `37420008988` and foundation `37420009037` were deliberately cancelled.
  Web-driver `37420009017` had already succeeded and refused cancellation.
  All eleven jobs settled. Cancellation is not a passing result or an outage.
- Native RRC records a separate post-release main epoch, preserving the completed
  publication and its prior ledger through native archival. Actual current-main
  causal evidence remains distinct from release publication proof.

- The retained Intel macOS log (job `112127037028`) also fails MCP
  `timeout_is_bounded_quarantined_and_never_replayed` during discovery, before
  the intended hanging request. The same 150 ms startup defect was recorded in
  [September 29 investigation](2026-09-29-post-release-quality-check-investigation.md).
  Cancellation does not erase this actual failure.

## Changes, methods and exact local proof

- Root `AGENTS.md` in production and the primary workspace records the requested
  same-commit documentation regulation. Publication-only receipts stay in the
  owned local report for delivery/next authorized code change; a separate
  documentation push requires an explicit request.
- Four prerequisite workflows exclude README/DOX prose, documentation Markdown
  and named foundation report/evidence companions. Source, fixtures, dependencies,
  workflows, bundled skills and release-objective provenance retain their gates.
  The exact-commit publishing gate refuses absent/skipped workflows.
- Both leader fixtures use a shared twenty-second overall invocation allowance,
  matching other readiness fixtures. The fifteen-second readiness bound and
  post-readiness assertions remain unchanged. Delayed descendant cleanup-marker
  timers wait for `settlement.ready` before their two-second delay.
- Two actual `RunCommand` regressions introduce six seconds of startup. Governed
  `cargo test --locked -p vesper-agent --all-features --test command_settlement
  fixture_allows_slow_startup_before_settlement -- --test-threads=2` reproduced
  both failures in 5.03 seconds with the old allowance.
- Corrected governed `cargo test --locked -p vesper-agent --all-features --test
  command_settlement -- --test-threads=2`: **13/13 passed in 22.29 seconds**.
- `python scripts/test_release_gate.py`: **10/10 passed**, including consistent
  exclusions, executable-input inclusion and refusal to tag a filtered source.
  All workflow YAML parses with PyYAML BaseLoader. `cargo fmt --all -- --check`
  and `git diff --check` passed.

- The MCP regression forces 300 ms Python startup and reproduced the discovery
  failure under the old 150 ms budget. It now warms the real connection with a
  ten-second startup allowance, then tests the unchanged 150 ms hanging-request
  deadline, quarantine and explicit reset. The mutable timeout seam exists only
  under `cfg(test)`; production operation deadlines are unchanged.

## Files and DOX

Root `AGENTS.md`; four prerequisite workflows and `.github/AGENTS.md`;
`crates/vesper-agent/tests/command_settlement.rs` and its owning `AGENTS.md`;
`crates/vesper-mcp/src/session{,_tests}.rs` and its owning `AGENTS.md`;
`scripts/test_release_gate.py` and `scripts/AGENTS.md`; this report, foundation
ownership/index and the owning PRD status link.

Nearest contracts record concrete CI/test/script rules. Parent crate/application/
documentation responsibilities and child indexes are unchanged. Primary edits
are restricted to the explicitly requested root regulation; unrelated dirty work
remains. The only production-source addition is a test-only MCP seam; production behavior
and version are unchanged.

## Deviations, unresolved items and readiness

The separate documentation push was my process mistake. Its Windows failures and
deliberately cancelled jobs remain retained. No installer, user authentication,
version bump, new tag or replacement release asset is performed for this repair.

Local proof cannot certify Windows. Native focused acceptance must check exact
corrected fixture bytes; the code/workflow correction must pass fresh exact-main
gates. Include documentation in that correction. Keep later results locally
without another documentation-only push. Published-release completion and
current-main health remain separate, truthful results.

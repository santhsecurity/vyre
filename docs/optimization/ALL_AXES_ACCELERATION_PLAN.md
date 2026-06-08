# Vyre all-axes acceleration plan

This file is the source-of-truth acceleration plan for Vyre. It replaces the generated label catalog with evidence-backed work rows. A row exists only when it names a local Vyre surface, a specific fix or improvement, a research basis, and a proof gate.

## Grounding rule

Every row must satisfy this contract before code starts:

| Field | Requirement |
| --- | --- |
| ID | Stable row id used in commits, claims, benchmarks, and evidence artifacts. |
| Axis | One owner lane from `docs/optimization/OWNERSHIP.toml` or one explicit cross-lane seam. |
| Local evidence | Concrete Vyre file, test, doc, or release artifact read before the row was written. |
| Research basis | Primary or official source, or an internal Vyre contract for pure repository hygiene rows. |
| Work | One specific innovation, fix, or improvement. No label-only catalog entries. |
| Proof gate | Exact test, benchmark, matrix, release evidence, or audit command that proves behavior. |
| Dedup seam | Existing primitive, schema, or boundary that the work must reuse or consolidate. |

Rows marked `research-backed innovation` require a comparative note in the proof artifact. Until the comparative note exists, the row is an improvement or fix, not an innovation claim.

## What was removed

The generated 10,000-label appendices are removed from this plan. They had no source evidence, no research citation, no owner surface, and no proof gate. Large numeric catalogs create planning noise and hide real work.

## Local Vyre evidence read

| Evidence | Finding used by this plan |
| --- | --- |
| `Cargo.toml` | Vyre is a multi-crate compiler/runtime/driver workspace with canonical shared dependencies on Surge grammar generation, Weir dataflow, wgpu, Naga, Metal, CUDA, emit crates, and benchmark crates. |
| `docs/optimization/README.md` | Optimization work has Layer 1 IR-pure, shared driver, Layer 2 backend, runtime megakernel, and benchmark lanes. |
| `docs/optimization/TAXONOMY.md` | Layer placement is already defined; plan rows must not mix backend names into shared crates. |
| `docs/optimization/OWNERSHIP.toml` | Write sets and required `cargo_full` commands exist for coordination, foundation, driver, runtime, benchmark, and op-matrix lanes. |
| `docs/optimization/HOT_PATHS.toml` | Hot-path contracts name scheduler, SoA facts, validator skip cache, Naga emit cache, specialization keys, wire encode, loop passes, and CUDA resident dispatch. |
| `docs/optimization/BENCH_TARGETS.toml` | Benchmark targets include release workloads for condition evaluation, string bitmap scatter, metadata, entropy, quantifiers, alias reaching definitions, IFDS witness, callgraph traversal, C AST traversal, megakernel queue, egraph saturation, sparse compaction, and dataflow. |
| `docs/optimization/OP_MATRIX.toml` | Op/backend coverage is already represented as data and must stay the coverage source. |
| `docs/RECURSION_THESIS.md` | Self-consumer gaps remain for multiple Tier 2.5 primitives and need a recursion gate. |
| `docs/lego-block-rule.md` | New primitives must pass the composition/reuse gate before promotion. |
| `vyre-foundation/src/optimizer/eqsat.rs` | In-tree equality saturation substrate exists with EGraph, union-find, rebuild, and extraction, deliberately avoiding an external dependency. |
| `vyre-foundation/src/optimizer/fact_substrate.rs` | Shape, use, and type facts share a fingerprint-keyed cache, currently thread-local with multiple cache slots. |
| `vyre-foundation/src/optimizer/rewrite_proof.rs` | Rewrite proof obligations emit typed SMT-LIB-style equivalence checks. |
| `vyre-lower/src/pre_emit.rs` | `lower_for_emit` is the canonical Program-to-KernelDescriptor production boundary with verify and cleanup. |
| `vyre-lower/src/lib.rs` | `verify_then_optimize` and `full_report` already bundle descriptor verification, analyses, and rewrite stats. |
| `vyre-lower/src/emit_adversarial_corpus.rs` | Emit adversarial descriptors exist and must be shared by emit backends. |
| `vyre-emit-metal/src/lib.rs` | Metal emission goes through `vyre-emit-naga`, validates Naga, emits MSL, and writes a stable artifact schema. |
| `vyre-driver/src/device_profile.rs` | Backend-neutral `DeviceProfile` already carries capabilities, timing quality, hardware counters, cache sizes, and tuning fields. |
| `vyre-driver/src/observability.rs` | Driver observability centralizes dispatch telemetry and substrate decision metrics. |
| `vyre-driver/src/evidence.rs` | Source provenance, dirty worktree fingerprints, dispatch timing evidence, artifacts, replay evidence, and `EvidenceBundle` exist. |
| `vyre-driver-cuda/src/backend/cuda_graph.rs` | CUDA graph capture/replay exists with shape-bound host pointer persistence and no-allocation-during-capture constraints. |
| `vyre-driver-cuda/src/backend/resident_dispatch.rs` | CUDA resident dispatch is split into async, batch, borrowed, sequence, sync, timed, and helper modules. |
| `vyre-runtime/src/megakernel/mod.rs` | Persistent megakernel owns protocol, handlers, builder, execution, resident buffers, recovery, scheduler, speculation, telemetry, task ABI, and workspace layout. |
| `vyre-runtime/src/megakernel/scheduler.rs` | Priority partitions, starvation guard, tenant fairness, and probe budgeting are already encoded. |
| `vyre-runtime/src/megakernel/ring.rs` | Host ring producer/consumer traits define the ring seam above byte protocol. |
| `vyre-runtime/src/megakernel/task.rs` | Continuation task ABI supports pause, yield, requeue, resume, priority, and task lineage. |
| `vyre-runtime/src/megakernel/telemetry.rs` | Ring/control telemetry has structured fallible decode surfaces plus infallible panic wrappers. |
| `vyre-libs/src/security/facts.rs` | Security fact schema, columnar packing, fact validation, proof path, and finding bundles already exist. |
| `vyre-libs/src/security/flows_to_with_sanitizer.rs` | Source-to-sink-with-sanitizer query composes bitset and graph primitives and emits fact-backed findings. |
| `vyre-libs/src/security/family_mask.rs` | Hardcoded source/sink/sanitizer family mapping exists and violates the Tier B data-file direction. |
| `vyre-bench/src/cases/*` and release evidence docs | Benchmark cases and release evidence already track CPU baselines, workload fingerprints, source provenance, GPU environment, and evidence status. |

## External research basis

| Key | Source | Use in this plan |
| --- | --- | --- |
| `CUDA_FEATURES` | <https://docs.nvidia.com/cuda/cuda-programming-guide/part4.html> | CUDA Graphs, stream-ordered allocator, cooperative groups, dependent launch, async barriers, async data copies, L2/cache controls. |
| `CUDA_OCCUPANCY` | <https://docs.nvidia.com/cuda/cuda-driver-api/group__CUDA__OCCUPANCY.html> | Occupancy APIs for launch configuration proof and resource-aware block sizing. |
| `CUDA_COOP` | <https://docs.nvidia.com/cuda/cuda-programming-guide/04-special-topics/cooperative-groups.html> | Cooperative groups, group partition hazards, grid synchronization, group reductions/scans, async copy alignment. |
| `METAL_COMMAND_BUFFERS` | <https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/MTLBestPracticesGuide/CommandBuffers.html> | Command buffer batching and CPU/GPU queue balance. |
| `METAL_COUNTERS` | <https://developer.apple.com/documentation/metal/gpu-counters-and-counter-sample-buffers> | Hardware counter sample buffers and timing-quality evidence. |
| `METAL_HEAPS` | <https://developer.apple.com/documentation/metal/memory-heaps> | Resource heap/hazard tracking proof for resident allocation and aliasing plans. |
| `MLIR_PASS` | <https://mlir.llvm.org/docs/PassManagement/> | Pass ownership, analysis preservation, pass failure, stats, instrumentation, crash reproduction. |
| `MLIR_TRANSFORM` | <https://mlir.llvm.org/docs/Dialects/Transform/> | Fine-grained transformation control separated from payload IR. |
| `MLIR_CANON` | <https://mlir.llvm.org/docs/Canonicalization/> | Canonicalization as best-effort, case-by-case transformation with explicit legality. |
| `CODEQL_FLOW` | <https://codeql.github.com/docs/writing-codeql-queries/about-data-flow-analysis/> | Local/global dataflow, taint edges, sources/sinks, aliasing challenges, graph size risks. |
| `SOUFFLE_DOCS` | <https://souffle-lang.github.io/docs.html> | Datalog for static analysis, component model, performance/scalability techniques. |
| `SOUFFLE_EXEC` | <https://souffle-lang.github.io/execute> | Fact I/O, compile vs interpret modes, profiling and provenance options. |
| `EGG` | <https://github.com/egraphs-good/egg> | Equality saturation use cases, e-graphs for optimizers/verifiers, benchmark discipline. |
| `WGPU_NAGA` | <https://github.com/gfx-rs/wgpu> | wgpu native paths use Naga for platform shader translation and track WGSL support drift through implementation bugs. |
| `TREE_SITTER` | <https://tree-sitter.github.io/tree-sitter/> | Incremental, robust parser baseline and CPU comparison target for parser/gpu fact extraction. |

## Evidence-backed plan items

| ID | Axis | Local evidence | Research basis | Work | Proof gate | Dedup seam |
| --- | --- | --- | --- | --- | --- | --- |
| VX-001 | coordination | This file contained generated label appendices | `MLIR_PASS`, `docs/optimization/AGENT_CONTRACT.md` | Replace label-only planning with evidence rows that require local surface, research basis, work, proof, and dedup seam. | `rg` proves no generated appendix sections remain; each row has non-empty table cells. | This file only; no parallel roadmap appendices. |
| VX-002 | coordination | `docs/optimization/OWNERSHIP.toml`, `docs/optimization/CLAIMS.toml` | `MLIR_PASS` | Split mega-claims into lane-sized claims with one owner lane, one write set, and one proof command group per claim. | Claim audit rejects a claim whose lanes span unrelated write sets without a seam row. | `OWNERSHIP.toml` remains the lane source. |
| VX-003 | coordination | `docs/optimization/HOT_PATHS.toml` | `MLIR_PASS` | Expand hot-path contracts from historical rows to current dispatch, emit, ring, telemetry, and CUDA graph surfaces, then make removal require a proof artifact. | `cargo_full run --bin xtask -- hot-path-scan` plus a row-count audit against active benchmark cases. | `HOT_PATHS.toml` is the only hot-path list. |
| VX-004 | op_matrix | `docs/optimization/OP_MATRIX.toml` | `MLIR_CANON` | Make op/backend support claims fail closed unless OP matrix row, conformance test, backend test, and benchmark target agree. | `cargo_full run --bin xtask -- op-matrix-check` rejects missing or duplicate truth rows. | `OP_MATRIX.toml` remains the coverage source. |
| VX-005 | bench_harness | `docs/optimization/BENCH_TARGETS.toml`, `vyre-bench/src/registry/mod.rs` | `CUDA_FEATURES`, `METAL_COUNTERS` | Cross-check every release benchmark case id against a canonical target, baseline class, metric, and timing-quality requirement. | `cargo_full test -p vyre-bench benchmark_target_contracts` with positive, missing-target, and stale-target fixtures. | `BENCH_TARGETS.toml` owns thresholds. |
| VX-006 | foundation_optimizer | `vyre-foundation/src/optimizer/eqsat.rs` | `EGG`, `MLIR_CANON` | Add extraction budgets, saturation stop reasons, class growth telemetry, and rewrite-family identity to every e-graph run. | Foundation tests assert stop reason, class count, extraction cost, and no silent compatibility fallback. | Existing in-tree `EGraph` stays canonical. |
| VX-007 | foundation_optimizer | `vyre-foundation/src/optimizer/rewrite_proof.rs` | `EGG`, `MLIR_CANON` | Wire rewrite proof obligations into optimizer rewrite registration so each arithmetic rewrite emits a solver-consumable equivalence artifact. | Rewrite-proof tests assert rewrite id, preconditions, before/after SMT, and invalid sort rejection. | `rewrite_proof.rs` is the proof schema. |
| VX-008 | foundation_optimizer | `vyre-foundation/src/optimizer/fact_substrate.rs` | `MLIR_PASS` | Replace repeated per-pass facts with a scheduler-owned fact substrate handle that records preserved, invalidated, and recomputed analyses per pass. | Scheduler test proves two mutating passes invalidate facts once and two read-only passes reuse facts without extra derivation. | `FactSubstrate` remains the cache. |
| VX-009 | foundation_optimizer | `docs/optimization/HOT_PATHS.toml`, `fact_substrate.rs` | `MLIR_PASS` | Add allocation and clone counters for scheduler, Program SoA, validator skip cache, Naga emit cache, specialization key, wire encode, loop passes, and CUDA resident dispatch. | Hot-path scan emits per-file allocation budget deltas and fails on new hot-path heap work. | `HOT_PATHS.toml` plus one scanner. |
| VX-010 | foundation_optimizer | `vyre-foundation/src/optimizer/eqsat_gpu.rs` from source inventory | `EGG`, `SOUFFLE_DOCS` | Turn the GPU e-graph snapshot into a measured bridge: CPU e-graph -> compact columns -> GPU equivalence application -> CPU extraction parity. | Egraph benchmark case records CPU saturation time, GPU apply time, recall parity, and class id determinism. | Existing `eqsat` and `eqsat_gpu` modules. |
| VX-011 | foundation_wire | `docs/optimization/HOT_PATHS.toml`, `vyre-driver/src/evidence.rs` | Internal Vyre evidence contract | Make Program fingerprints, source tree fingerprints, and workload fingerprints share one digest version ledger. | Unit tests mutate source, evidence, test-only files, and untracked files and assert expected fingerprint changes. | `vyre-driver/src/evidence.rs` provenance functions. |
| VX-012 | lower_emit | `vyre-lower/src/pre_emit.rs` | `MLIR_PASS`, `MLIR_TRANSFORM` | Audit all concrete drivers and emit crates so production emission enters through `lower_for_emit` or a documented already-lowered artifact path. | `rg`-backed boundary test fails on driver-local Program-to-shader lowering. | `vyre-lower::pre_emit::lower_for_emit`. |
| VX-013 | lower_emit | `vyre-lower/src/lib.rs` | `MLIR_PASS` | Promote `full_report` into the single descriptor diagnostic surface used by emit failures, backend compile errors, and benchmark artifacts. | Tests assert report includes summary, histogram, perf audit, verify status, optimization stats, and descriptor id. | `vyre-lower::FullReport`. |
| VX-014 | lower_emit | `vyre-lower/src/emit_adversarial_corpus.rs` | `WGPU_NAGA`, `MLIR_PASS` | Use one adversarial descriptor corpus across Naga, PTX, SPIR-V, Metal, WGPU, and CUDA emit tests. | Emit crate tests iterate the shared corpus and assert structured errors or valid artifacts per case. | `emit_adversarial_corpus.rs`. |
| VX-015 | lower_emit | `vyre-emit-metal/src/lib.rs` | `WGPU_NAGA`, `METAL_COMMAND_BUFFERS` | Keep Metal MSL emission behind shared Naga emission and add artifact hash, binding map, sidecar buffer, and threadgroup metadata parity tests. | `cargo_full test -p vyre-emit-metal artifact_contracts` validates schema and error messages. | `vyre-emit-naga` remains shared shader IR seam. |
| VX-016 | driver_cuda | `vyre-driver-cuda/src/backend/cuda_graph.rs` | `CUDA_FEATURES` | Harden CUDA graph replay by keying capture on input/output layout, host pointer identity, stream identity, module identity, and transfer accounting policy. | CUDA graph tests mutate each key component and require a recapture or a precise invalid-shape error. | Existing `CachedCudaGraph` and input identity keys. |
| VX-017 | driver_cuda | `cuda_graph.rs`, `resident_dispatch.rs` | `CUDA_FEATURES` | Add a stream-ordered allocation path for resident/captured buffers where CUDA capabilities support it, with capture-time allocation rejection preserved. | CUDA tests prove no allocation occurs during capture and stream-ordered frees do not force device-wide sync. | Existing CUDA allocation helpers. |
| VX-018 | driver_cuda | `vyre-driver/src/device_profile.rs` | `CUDA_OCCUPANCY` | Fill CUDA `DeviceProfile` occupancy fields from CUDA function attributes and occupancy APIs, then route launch sizing through profile evidence. | Launch tests assert block size, active blocks per SM, shared memory, register usage, and fallback reason. | `DeviceProfile` owns capability projection. |
| VX-019 | driver_cuda | `vyre-driver/src/observability.rs`, grid-sync telemetry fields | `CUDA_COOP` | Add a cooperative-grid path for true grid synchronization when capability and launch geometry allow it; keep split dispatch as explicit fallback. | Tests assert cooperative path, split fallback, telemetry counters, and unsupported-device diagnostics. | Shared grid-sync split counters. |
| VX-020 | driver_cuda | `vyre-emit-ptx/src/emitter/async_copy.rs` from source inventory | `CUDA_COOP` | Add async-copy alignment proof and shared-memory staging telemetry to PTX emission for copy/compute overlap candidates. | PTX pattern tests assert aligned `cp.async` shape or an explicit scalar fallback reason. | Existing PTX async-copy emitter. |
| VX-021 | driver_cuda | `vyre-driver-cuda/src/backend/resident_dispatch/*` | `CUDA_FEATURES` | Consolidate resident sequence replay, fused sequence, borrowed output, timed path, and async path behind one resident dispatch plan object. | CUDA resident tests assert identical plan ids and timing evidence across sync, async, sequence, and fused entrypoints. | Existing resident dispatch submodules. |
| VX-022 | driver_cuda | `release/evidence/docs/cuda-release-path.md`, `vyre-driver/src/evidence.rs` | `CUDA_FEATURES` | Attach CUDA driver/runtime version, GPU model, timing quality, source fingerprint, and replay capsule to every CUDA performance artifact. | Release evidence semantic tests reject missing GPU provenance, missing source identity, and contradictory pass summaries. | `EvidenceBundle` is the artifact wrapper. |
| VX-023 | driver_metal | `vyre-driver-metal/src/lib.rs`, `vyre-driver-metal/src/runtime.rs` | `METAL_COMMAND_BUFFERS` | Add Metal command-buffer batching policy tied to workload shape, enqueue/wait timing, and resident sequence reuse. | Metal tests assert one-command-buffer batching for fused resident sequences and timing fields in evidence. | Shared resident timing evidence. |
| VX-024 | driver_metal | `DeviceProfile::timing_quality`, Metal runtime metrics | `METAL_COUNTERS` | Probe Metal counter set support and upgrade timing quality from host-only to device timestamps or hardware counters when the device exposes them. | Metal capability test asserts counter presence maps to `DeviceTimingQuality` and benchmark reports cite it. | `DeviceProfile` timing-quality enum. |
| VX-025 | driver_metal | Metal resource output/readback tests named in `vyre-driver-metal/src/lib.rs` | `METAL_HEAPS` | Add heap-backed resident allocation and hazard-tracking evidence for buffers reused across compiled resident dispatch. | Metal tests assert heap allocation path, readback-free resource output, and hazard mode classification. | Metal runtime resource allocator seam. |
| VX-026 | driver_wgpu | `vyre-driver-wgpu/tests/megakernel_emit.rs`, Naga shared path from rg output | `WGPU_NAGA` | Make WGPU/Naga drift explicit by recording Naga version, WGSL lowering contract, validator result, and unsupported feature reason per pipeline artifact. | WGPU pipeline-cache tests assert key changes on WGSL lowering contract and validator errors retain descriptor context. | `vyre-emit-naga` and WGPU pipeline cache. |
| VX-027 | driver_wgpu | Dirty local `vyre-driver-wgpu/src/megakernel/dispatcher.rs` and `megakernel_divergent_recall.rs` noted by status | `WGPU_NAGA` | Finish divergent recall proof without touching unrelated dirty work: dispatcher must return structured success/error for every sequence case, never empty output. | Targeted WGPU megakernel divergent-recall test asserts non-empty response, CPU parity, and dropped-detector count. | Runtime megakernel planner owns semantics; WGPU owns command glue. |
| VX-028 | runtime_megakernel | `vyre-runtime/src/megakernel/ring.rs` | `CUDA_COOP` | Strengthen ring producer/consumer ABI with publication-order tests, status-word-last proof, misaligned slot rejection, and out-of-range diagnostics. | Runtime ring tests cover valid publish/read, wrong byte length, invalid slot, done count overflow guard, and consumer parity. | `RingProducer` and `RingConsumer` traits. |
| VX-029 | runtime_megakernel | `vyre-runtime/src/megakernel/scheduler.rs` | `CUDA_COOP` | Convert priority partition, tenant fairness, and starvation guard into measurable scheduler fairness reports. | Scheduler tests assert max probes, lower-priority service after threshold, tenant throttle, and no u32 overflow. | Existing scheduler functions and policy constants. |
| VX-030 | runtime_megakernel | `vyre-runtime/src/megakernel/task.rs` | `CUDA_COOP` | Build continuation task lifecycle tests across Ready, Running, Paused, Yielded, Requeued, Done, and Faulted states. | ABI tests assert word encoding, schedulable states, pause/resume flags, lineage ids, and conversion from legacy work item. | `TaskWorkItem` ABI. |
| VX-031 | runtime_megakernel | `vyre-runtime/src/megakernel/telemetry.rs` | `METAL_COUNTERS`, `CUDA_FEATURES` | Remove infallible telemetry decode usage from production paths and route invalid control/ring bytes through structured errors. | Runtime tests assert malformed buffers return `PipelineError` with fix text and no production panic path is used. | Existing `try_decode*` functions. |
| VX-032 | runtime_megakernel | `MegakernelDispatch`, `MegakernelReport`, `MegakernelTelemetry` exports | `CUDA_FEATURES` | Add a persistent-dispatch report schema that records queue depth, published/claimed/done slots, requeues, fairness counters, and launch geometry. | Megakernel latency bench emits the schema and release evidence semantic tests validate it. | Existing telemetry structs. |
| VX-033 | security_static_analysis | `vyre-libs/src/security/facts.rs` | `CODEQL_FLOW` | Make source, sink, sanitizer, auth, edge, call, control, dataflow, and derived facts the only security finding input shape. | Fact tests reject LLM-only findings, missing proof paths, duplicate ids, missing provenance parents, and invalid spans. | `AnalysisFactTable` and `FindingProofBundle`. |
| VX-034 | security_static_analysis | `flows_to_with_sanitizer.rs`, `facts.rs` | `CODEQL_FLOW` | Add fact-backed source-to-sink query wrappers for auth-check dominance, sanitizer dominance, path length, and family masks. | Security tests assert positive, negative, sanitizer-killed, auth-killed, and missing-fact cases with proof path roles. | Existing flow composition kernels. |
| VX-035 | security_static_analysis | `vyre-libs/src/security/family_mask.rs` | `SOUFFLE_DOCS`, `CODEQL_FLOW` | Move hardcoded source/sink/sanitizer family mappings into Tier B TOML data and generate the runtime table from that file. | Data contract test mutates TOML fixture and proves generated masks change while code remains untouched. | One family data file under `rules/`. |
| VX-036 | security_static_analysis | `AnalysisFactColumns` in `facts.rs` | `SOUFFLE_EXEC` | Add Souffle-compatible fact import/export for facts, columns, and finding proof paths so CPU Datalog and GPU Vyre queries share evidence. | Roundtrip tests write `.facts`, read them, run the same query, and compare finding id plus proof path. | `AnalysisFactTable::to_columnar`. |
| VX-037 | security_static_analysis | `facts.rs` proof-path schema | `SOUFFLE_DOCS`, `SOUFFLE_EXEC` | Add minimal proof-path extraction and compression for large dataflow findings. | Property tests assert every emitted finding has source, edge/control/call path, sanitizer/auth consideration, sink, and stable ordering. | `FindingProofBundle`. |
| VX-038 | libs_parsing | `Cargo.toml` Surge/Weir dependencies, parser tests from rg output | `TREE_SITTER`, `CODEQL_FLOW` | Define parser bootstrap boundary: Tree-sitter or Surge creates trusted CPU parse/fact baselines; Vyre GPU kernels accelerate token, motif, and fact projection. | Parser benchmark compares GPU projection to Tree-sitter baseline on C, Go, Rust, and Linux corpus fixtures. | Surge grammar generation and existing parser tests. |
| VX-039 | libs_parsing | `vyre-libs/tests/rust_gpu_lexer_plan.rs`, C keyword/preprocessor tests from rg output | `TREE_SITTER` | Replace scattered GPU lexer tests with one language-agnostic packed-source lexer harness that supports per-source errors and batch isolation. | Harness tests assert single source, batched source, unknown byte isolation, empty source, and corpus parity. | Existing packed source buffers and lexer plans. |
| VX-040 | libs_dataflow | `Cargo.toml` Weir dependency, `vyre-libs/src/security/facts.rs` | `CODEQL_FLOW`, `SOUFFLE_DOCS` | Create one adapter from Weir/CodeQL-like dataflow facts into Vyre fact columns, including alias, call, control, and taint edges. | Adapter tests assert local flow, global flow, alias, unresolved call target, and taint-only edge behavior. | `AnalysisFactTable` is the interchange schema. |
| VX-041 | bench_harness | `vyre-bench/src/cases/release_workloads.rs`, `BENCH_TARGETS.toml` | `CUDA_FEATURES`, `METAL_COUNTERS` | Store active device time, enqueue time, wait time, wall time, host-transfer bytes, and readback bytes as separate metrics. | Benchmark schema tests reject conflated timing and missing transfer accounting. | `DispatchTimingEvidence` and benchmark metrics API. |
| VX-042 | bench_harness | `vyre-driver/src/evidence.rs`, `xtask/src/benchmark_evidence_semantics.rs` from rg output | Internal Vyre evidence contract | Make stale summary data impossible by deriving suite pass/fail summaries from case evidence at validation time. | Evidence semantic tests reject contradictory summary counts, missing blockers, and dirty source collapse. | `EvidenceBundle` plus xtask semantic validator. |
| VX-043 | bench_harness | `vyre-bench/tests/thesis_workload_contracts.rs` from rg output | `CODEQL_FLOW`, `TREE_SITTER`, `CUDA_FEATURES` | Enforce release suite diversity across parsing, graph traversal, pattern matching, megakernel, zero-copy ingest, optimizer, and security dataflow. | Test asserts each evidence class has at least one benchmark case, target row, and CPU baseline. | Release workload registry. |
| VX-044 | bench_harness | `vyre-bench/src/cases/scan_ac_irregular/*` from rg output | `CUDA_FEATURES` | Add Aho-Corasick irregular literal scan benchmarks with output-only match triples resident between samples. | Bench target `scan.ac.irregular_literals.4m` records CPU AC baseline, GPU output triples, count preflight, and replay filters. | Existing scan AC benchmark case. |
| VX-045 | bench_harness | `vyre-bench/src/cases/dataflow_irregular/*` from rg output | `CODEQL_FLOW`, `SOUFFLE_DOCS` | Expand IFDS, callgraph, and CSR queue benchmarks to cover sparse frontier, skewed rows, witness extraction, and closure. | Bench tests assert active-source requirement, CSR monotonicity, queue materialization fingerprint, and source-to-sink witness. | Existing dataflow irregular cases. |
| VX-046 | architecture_dedup | `DeviceProfile`, backend capability methods | `CUDA_OCCUPANCY`, `METAL_COUNTERS`, `WGPU_NAGA` | Eliminate parallel capability structs by projecting CUDA, WGPU, Metal, SPIR-V, and reference capabilities into `DeviceProfile`. | Boundary test rejects new backend capability records outside concrete driver probe code and `DeviceProfile` projection. | `vyre-driver/src/device_profile.rs`. |
| VX-047 | architecture_dedup | `FactSubstrate`, `AnalysisFactTable`, `reaching_def_facts` from rg output | `CODEQL_FLOW`, `SOUFFLE_DOCS` | Separate optimizer facts and security facts through typed adapters, not duplicate schemas. | Tests prove optimizer reaching-def facts convert to security/dataflow facts only through an explicit adapter with provenance. | One adapter layer, no schema cloning. |
| VX-048 | architecture_dedup | `lower_for_emit`, `vyre-emit-metal`, `vyre-emit-naga`, driver rg output | `MLIR_PASS`, `WGPU_NAGA` | Enforce one Program-to-descriptor-to-artifact path across backends and delete driver-local lowering forks as they are replaced. | Boundary test rejects `Program` lowering code under concrete driver emit modules. | `vyre-lower` plus emit crates. |
| VX-049 | architecture_dedup | `ring.rs`, `protocol.rs`, task ABI exports | `CUDA_COOP` | Keep megakernel protocol bytes, ring publication, task ABI, and telemetry decoding in runtime, with drivers limited to buffer binding and command submission. | Import-boundary test rejects protocol byte construction in driver crates. | Runtime megakernel modules. |
| VX-050 | architecture_dedup | `family_mask.rs`, hardcoded map output from rg | `SOUFFLE_DOCS` | Convert security taxonomy, rule families, scan signatures, and parser keyword sets to Tier B data files with generated loaders. | Hardcoded-list audit fails on new literal family tables in Rust source. | One `rules/` source per data family. |
| VX-051 | self_substrate | `docs/RECURSION_THESIS.md` | `SOUFFLE_DOCS`, `EGG` | Implement a recursion gate that walks registered op ids and proves each Tier 2.5 primitive has a Vyre-self consumer or an explicit non-recursive classification. | `cargo_full run --bin xtask -- recursion-gate` fails on missing self-consumers and names the op id. | Existing op registry. |
| VX-052 | testing | `xtask/README.md`, `OP_MATRIX.toml`, `BENCH_TARGETS.toml` | `MLIR_PASS` | Generate contract tests from OP matrix and benchmark target data instead of hand-maintaining duplicate support lists. | Generated tests assert file, rule/op id, backend, output bytes or structured error, and exit code. | TOML data is the source. |
| VX-053 | testing | `vyre-lower/src/emit_adversarial_corpus.rs`, `vyre-core/tests/wire_malformed_adversarial.rs` from rg output | `WGPU_NAGA`, `MLIR_PASS` | Expand adversarial tests around malformed wire bytes, invalid descriptors, unsupported shader constructs, bad fact tables, and malformed ring/control buffers. | Adversarial suite asserts no silent success, no empty result, no panic in production API, and actionable fix text. | Existing adversarial corpora. |
| VX-054 | testing | `vyre-self-substrate/src/integration/release/release_validation_matrix.rs` from rg output | `CUDA_FEATURES` | Make every CUDA-identified release gate require a GPU probe immediately before execution and reject raw cargo commands. | Release matrix tests assert GPU probe insertion, unique gate ids, valid package names, and `./cargo_full` command shape. | Release validation matrix. |
| VX-055 | dogfood_ux | `vyre-debug/SPEC_NAGA_FAILURE_TRIAGE.md`, emit/driver diagnostic surfaces | `WGPU_NAGA`, `MLIR_PASS` | Add failure capsules for shader validation and backend dispatch: descriptor full report, source provenance, artifact hash, backend profile, and reproduction command. | Debug tests assert Naga validation failure includes descriptor id, op id, type context, artifact identity, and reproduction command. | `FullReport` and `EvidenceBundle`. |
| VX-056 | performance_research | `BENCH_TARGETS.toml` egraph, dataflow, C AST, megakernel targets | `EGG`, `CODEQL_FLOW`, `TREE_SITTER`, `CUDA_FEATURES` | Create a research comparison matrix for Vyre against CPU e-graph, CodeQL/Souffle-style dataflow, Tree-sitter parsing, and GPU launch baselines. | Benchmark reports include baseline implementation name, version/source fingerprint, workload shape, correctness oracle, and speed ratio. | `BENCH_TARGETS.toml` plus evidence artifacts. |
| VX-057 | performance_research | `DeviceProfile`, `observability.rs`, CUDA/Metal/WGPU backends | `CUDA_OCCUPANCY`, `METAL_COUNTERS`, `WGPU_NAGA` | Add auto-tuning records that bind launch geometry decisions to device profile, measured timing quality, and cache key. | Tuner tests assert same workload/device/profile reuses decision and profile change invalidates it. | Existing tuner/autotune store modules. |
| VX-058 | performance_research | `vyre-driver/src/launch_fusion.rs`, `megakernel_*` modules from source inventory | `CUDA_FEATURES`, `METAL_COMMAND_BUFFERS` | Compare launch fusion, CUDA graphs, Metal command-buffer batching, WGPU pipeline reuse, and persistent megakernel for the same workload shapes. | Benchmark table records per-shape winner, overhead class, and fallback reason. | Shared launch plan schema. |
| VX-059 | robustness | `browser_sequence` bug report from Meridian discussion is analogous to tool empty-output failures; Vyre has emit/dispatch APIs | `MLIR_PASS` | For Vyre APIs, ban empty success responses at boundaries: emit, lower, dispatch, telemetry, fact query, benchmark run, and evidence validation all return structured success or structured error. | Boundary tests assert empty input, invalid op, unsupported backend, malformed descriptor, and malformed fact table produce non-empty diagnostics. | Existing error types per crate. |
| VX-060 | organization | `docs/optimization/LEGACY_DOCS.md`, generated bad plan history | Internal Vyre documentation contract | Add a doc index gate that accepts only canonical docs for active plans and marks historical docs as evidence-only with links to replacement sources. | Doc gate rejects active work items outside canonical plan, roadmap, OP matrix, bench targets, or claims files. | `LEGACY_DOCS.md` controls doc status. |

## Execution order

1. Land coordination guards: VX-001 through VX-005, VX-060.
2. Land seam and dedup guards: VX-046 through VX-050.
3. Land evidence and diagnostics: VX-041 through VX-045, VX-055.
4. Land foundation optimizer proof work: VX-006 through VX-011.
5. Land lower/emit path hardening: VX-012 through VX-015.
6. Land backend-specific performance work: VX-016 through VX-027.
7. Land megakernel runtime work: VX-028 through VX-032.
8. Land security/static-analysis and parser/dataflow work: VX-033 through VX-040.
9. Land self-substrate, generated tests, adversarial tests, and release matrix gates: VX-051 through VX-054.
10. Land comparative performance research: VX-056 through VX-058.
11. Land boundary robustness: VX-059.

## Required proof vocabulary

Every implementation tied to this plan must use these proof terms consistently:

| Term | Meaning |
| --- | --- |
| `truth` | Output equals reference semantics and asserts exact file/op/fact/line/result details. |
| `negative twin` | The nearest non-vulnerable or non-optimizable case remains rejected or unchanged. |
| `adversarial` | Malformed, oversized, contradictory, unsupported, or hostile input produces a structured error. |
| `property` | Generated inputs cover invariants beyond hand-written examples. |
| `differential` | Compared against CPU reference, peer tool, or alternate backend. |
| `perf` | Benchmark proves active-time or resource improvement against named baseline. |
| `evidence` | Artifact includes source fingerprint, workload fingerprint, environment, command, and pass/fail semantics. |
| `dedup` | One primitive, schema, parser, config source, or boundary remains after the change. |

## Document hygiene gate

The plan file must pass these checks before commit:

```bash
rg -n 'generated label|10,000-label|label-only catalog entry without' docs/optimization/ALL_AXES_ACCELERATION_PLAN.md
rg -n '^\| VX-[0-9]{3} ' docs/optimization/ALL_AXES_ACCELERATION_PLAN.md
```

The repo-wide hygiene marker scan defined in the global agent contract must return no matches. The VX row count must equal the number of work rows in the evidence-backed plan table.

# repro_rules_js_rbe_bug

While building this target suceeds:

```
bazel build --remote_executor=YOUR_REMOTE_EXECUTOR //:compile_ts_typecheck
```

And creating a `js_binary` providing the outputs of that target works just fine as well:

```
bazel run --remote_executor=YOUR_REMOTE_EXECUTOR //:start
```

The following target fails when using RBE. Not providing the `--remote_executor` flag makes the build succeed.

```
bazel build --remote_executor=YOUR_REMOTE_EXECUTOR //backend/packages/logging:compile_ts_typecheck
```

The error message is as follows:

```
ERROR: /home/andre/Projects/repro_rules_js_rbe_bug/backend/packages/logging/BUILD:11:11: Compiling TypeScript project @//backend/packages/logging:compile_ts_typings [tsc -p backend/packages/logging/tsconfig.json] failed: (Exit 1): tsc_worker.sh failed: error executing command (from target //backend/packages/logging:compile_ts_typings) bazel-out/k8-opt-exec-2B5CBBC6/bin/external/npm_typescript/tsc_worker.sh @bazel-out/k8-fastbuild/bin/backend/packages/logging/index.d.ts-0.params
WARNING: Running TsProject as a standalone process
Started a standalone process to perform this action but this might lead to some unexpected behavior with tsc due to being run non-sandboxed.
Your build might be misconfigured, try putting "build --strategy=TsProject=worker" into your .bazelrc or add "supports_workers = False" attribute into this ts_project target.

backend/packages/logging/index.ts(1,52): error TS2307: Cannot find module '@opentelemetry/api' or its corresponding type declarations.

Target //backend/packages/logging:compile_ts_typecheck failed to build
Use --verbose_failures to see the command lines of failed build steps.
INFO: Elapsed time: 1.269s, Critical Path: 1.14s
INFO: 7 processes: 1 remote cache hit, 3 internal, 3 local.
FAILED: Build did NOT complete successfully
```

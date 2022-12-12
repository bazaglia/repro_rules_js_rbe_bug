load("@bazel_skylib//lib:partial.bzl", "partial")
load("@aspect_rules_ts//ts:defs.bzl", "ts_project")
load("@aspect_rules_swc//swc:defs.bzl", "swc")
load("@aspect_rules_js//npm:defs.bzl", "npm_package")
load("@npm//:defs.bzl", "npm_link_all_packages")

# Link node_modules
npm_link_all_packages(name = "node_modules")

# Transpile to js
ts_project(
    name = "compile_ts",
    srcs = ["index.ts"],
    declaration = True,
    source_map = True,
    transpiler = partial.make(
        swc,
        source_maps = "true",
        swcrc = ".swcrc",
    ),
    deps = [":node_modules"],
)

# Provide npm package with transpiled code and declaration files
npm_package(
    name = "logging",
    srcs = [":compile_ts"],
    include_runfiles = False,
    visibility = ["//visibility:public"],
)

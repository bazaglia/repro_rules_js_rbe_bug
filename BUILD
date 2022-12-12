load("@aspect_rules_ts//ts:defs.bzl", "ts_project")
load("@aspect_rules_swc//swc:defs.bzl", "swc")
load("@aspect_rules_js//js:defs.bzl", "js_binary")
load("@bazel_skylib//lib:partial.bzl", "partial")
load("@npm//:defs.bzl", "npm_link_all_packages")

npm_link_all_packages(name = "node_modules")

js_binary(
    name = "start",
    data = [":compile_ts"],
    entry_point = "example.js",
    node_options = ["--enable-source-maps"],
)

ts_project(
    name = "compile_ts",
    srcs = ["example.ts"],
    declaration = True,
    source_map = True,
    transpiler = partial.make(
        swc,
        source_maps = "true",
        swcrc = ".swcrc",
    ),
    deps = [":node_modules"],
)

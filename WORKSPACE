workspace(name="dev_aduu_rules_claat")

load("//:go.bzl", "claat_go_dependencies")

claat_go_dependencies()

load(
    "@io_bazel_rules_go//go:deps.bzl",
    "go_rules_dependencies",
    "go_register_toolchains",
)

go_rules_dependencies()

go_register_toolchains()

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()

load("//:claat.bzl", "claat_dependencies")

claat_dependencies()

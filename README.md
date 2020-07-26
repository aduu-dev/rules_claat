# Rule for generating html codelab

Known limitations:

- only uses markdown files, no embedding any content

Inside WORKSPACE:

```s
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "dev_aduu_rules_claat",
    sha256 = "6b72bebeff6ff2280ff08512fa92fb1b95120c2e9d18dceee8288ecd19123dcf",
    strip_prefix = "rules_claat-0.1.1",
    urls = ["https://github.com/aduu-dev/rules_claat/archive/v0.1.1.zip"],
)

# Only in case you haven't included rules_go and gazelle yet.
load("@dev_aduu_rules_claat//:go.bzl", "claat_go_dependencies")

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

# rules_claat

load("@dev_aduu_rules_claat//:claat.bzl", "claat_dependencies")

claat_dependencies()

```

Inside BUILD.bazel:

```s
load("@dev_aduu_rules_claat//:claat.bzl", "claat")

filegroup(
    name = "example-mds",
    srcs = glob(["*.md"]),
)

claat(
    name = "examples",
    srcs = [
        ":example-mds",
    ],
)
```

Build and list result:

```sh
bazel build //examples:examples # BUILD.bazel above is located in //examples here.

# /claat/examples.tar comes from the fact that we are inside //examples and we created //examples:example.tar
ls $(bazel info bazel-genfiles)/examples/examples.tar
# Output:
#   bazel-out/darwin-fastbuild/bin/examples/examples.tar
```

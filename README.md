# Rule for generating html codelab

Known limitations:

- only uses markdown files, no embedding any content

Inside WORKSPACE:

```s
# Only in case you haven't included rules_go and gazelle yet.
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

# rules_claat

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "com_github_googlecodelabs_tools",
    sha256 = "8992b65ec5d98725352601b6fbbbf9585fa784a03c02e8dd927b23da3a84d39f",
    strip_prefix = "claat-2.2.1",
    urls = ["https://github.com/fabstu/claat/archive/v2.2.1.zip"],
)

load("//:claat.bzl", "claat_dependencies")

claat_dependencies()

```

Inside BUILD.bazel:

```s
load("@com_github_googlecodelabs_tools//claat:claat.bzl", "claat")

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
bazel build //claat:examples # BUILD.bazel above is located in //claat here.

# /claat/examples.tar comes from the fact that we are inside //claat and we created //claat:example.tar
ls $(bazel info bazel-genfiles)/claat/examples.tar
# Output:
# bazel-out/darwin-fastbuild/bin/claat/examples.tar
```

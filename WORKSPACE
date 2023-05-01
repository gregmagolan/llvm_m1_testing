workspace(name = "testing")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# https://github.com/grailbio/bazel-toolchain/releases
http_archive(
    name = "com_grail_bazel_toolchain",
    patch_args = ["-p1"],
    patches = ["//:com_grail_bazel_toolchain.patch"],
    sha256 = "b54aa3b00a64a3dea06d30f0ff423e91bcea43019c5ff1c319f726f1666c3ff2",
    strip_prefix = "bazel-toolchain-2f6e6adf93f4bf34d7bce7ad797f53c82d998ba8",
    urls = ["https://github.com/grailbio/bazel-toolchain/archive/2f6e6adf93f4bf34d7bce7ad797f53c82d998ba8.tar.gz"],
)

_SYSROOT_BUILD_FILE = """
filegroup(
    name = "sysroot",
    srcs = glob(["*/**"]),
    visibility = ["//visibility:public"],
)
"""

http_archive(
    name = "org_chromium_sysroot_linux_arm64",
    build_file_content = _SYSROOT_BUILD_FILE,
    sha256 = "e39b700d8858d18868544c8c84922f6adfa8419f3f42471b92024ba38eff7aca",
    urls = ["https://commondatastorage.googleapis.com/chrome-linux-sysroot/toolchain/2202c161310ffde63729f29d27fe7bb24a0bc540/debian_stretch_arm64_sysroot.tar.xz"],
)

http_archive(
    name = "org_chromium_sysroot_linux_x86_64",
    build_file_content = _SYSROOT_BUILD_FILE,
    sha256 = "84656a6df544ecef62169cfe3ab6e41bb4346a62d3ba2a045dc5a0a2ecea94a3",
    urls = ["https://commondatastorage.googleapis.com/chrome-linux-sysroot/toolchain/2202c161310ffde63729f29d27fe7bb24a0bc540/debian_stretch_amd64_sysroot.tar.xz"],
)

load("@com_grail_bazel_toolchain//toolchain:deps.bzl", "bazel_toolchain_dependencies")

bazel_toolchain_dependencies()

load("@com_grail_bazel_toolchain//toolchain:rules.bzl", "llvm_toolchain")

llvm_toolchain(
    name = "llvm_toolchain",
    llvm_version = "15.0.7",
    sha256 = {
        "darwin-arm64": "867c6afd41158c132ef05a8f1ddaecf476a26b91c85def8e124414f9a9ba188d",
        "darwin-x86_64": "d16b6d536364c5bec6583d12dd7e6cf841b9f508c4430d9ee886726bd9983f1c",
    },
    strip_prefix = {
        "darwin-arm64": "clang+llvm-15.0.7-arm64-apple-darwin22.0",
        "darwin-x86_64": "clang+llvm-15.0.7-x86_64-apple-darwin21.0",
    },
    sysroot = {
        "linux-aarch64": "@org_chromium_sysroot_linux_arm64//:sysroot",
        "linux-x86_64": "@org_chromium_sysroot_linux_x86_64//:sysroot",
    },
    urls = {
        "darwin-arm64": ["https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.7/clang+llvm-15.0.7-arm64-apple-darwin22.0.tar.xz"],
        "darwin-x86_64": ["https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.7/clang+llvm-15.0.7-x86_64-apple-darwin21.0.tar.xz"],
    },
)

load("@aspect_bazel_lib//lib:transitions.bzl", "platform_transition_filegroup")
load("@rules_pkg//pkg:pkg.bzl", "pkg_tar")
load("@rules_pkg//pkg:mappings.bzl", "pkg_files")

pkg_tar(
    name = "clang_bins",
    strip_prefix = "external/llvm-project/clang",
    package_dir = "bin",
    srcs = [
        "@llvm-project//clang",
    ],
    symlinks = {
        "clang++": "./clang",
        "clang-cpp": "./clang",
    },
)

pkg_tar(
    name = "lld_bins",
    strip_prefix = "external/llvm-project/lld",
    package_dir = "bin",
    srcs = [
        "@llvm-project//lld",
    ],
    symlinks = {
        "ld.lld": "./lld",
    },
)

pkg_tar(
    name = "llvm_bins",
    strip_prefix = "external/llvm-project/llvm",
    package_dir = "bin",
    srcs = [
        "@llvm-project//llvm:llvm-ar",
        "@llvm-project//llvm:llvm-as",
        "@llvm-project//llvm:llvm-nm",
        "@llvm-project//llvm:llvm-objcopy",
    ],
    symlinks = {
        "llvm-strip": "./llvm-objcopy",
    },
    empty_files = [
        "bin/llvm-cov",
        "bin/llvm-dwp",
        "bin/llvm-objdump",
        "bin/llvm-profdata",
    ],
)

PLATFORMS = [
    "@zig_sdk//platform:darwin_amd64",
    "@zig_sdk//platform:darwin_arm64",
    "@zig_sdk//libc_aware/platform:linux_arm64_musl",
    "@zig_sdk//libc_aware/platform:linux_amd64_musl",
]

pkg_tar(
    name = "dist",
    extension = ".tar.xz",
    srcs = [
	"@llvm-raw//:clang_lib_headers",
    	"@llvm-raw//:libcxx_include",
    ],
    deps = [
        ":clang_bins",
        ":lld_bins",
        ":llvm_bins",
    ],
)

[
    platform_transition_filegroup(
        name = "for_" + platform.split(':')[1],
        target_platform = platform,
        srcs = [":dist"],
    )
    for platform in PLATFORMS
]

filegroup(
    name = "for_all_platforms",
    srcs = ["for_" + platform.split(':')[1] for platform in PLATFORMS],
)

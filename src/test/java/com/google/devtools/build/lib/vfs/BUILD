load("@rules_java//java:defs.bzl", "java_library", "java_test")

package(
    default_testonly = 1,
    default_visibility = ["//src:__subpackages__"],
)

filegroup(
    name = "srcs",
    testonly = 0,
    srcs = glob(["**"]) + [
        "//src/test/java/com/google/devtools/build/lib/vfs/util:srcs",
    ],
    visibility = ["//src:__subpackages__"],
)

# Tests for Windows-specific functionality that can run cross-platform.
CROSS_PLATFORM_WINDOWS_TESTS = [
    "PathFragmentWindowsTest.java",
    "RootedPathAndCasingTest.java",
    "WindowsPathTest.java",
]

# Tests for Windows-specific functionality that run on Windows.
WINDOWS_ON_WINDOWS_TESTS = [
]

# All Windows-specific tests. Use this to exclude Windows tests from globs.
ALL_WINDOWS_TESTS = CROSS_PLATFORM_WINDOWS_TESTS + WINDOWS_ON_WINDOWS_TESTS

java_library(
    name = "SymlinkAwareFileSystemTest",
    srcs = ["SymlinkAwareFileSystemTest.java"],
    deps = [
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:build",
        "//src/main/java/com/google/devtools/build/lib:os_util",
        "//src/main/java/com/google/devtools/build/lib/vfs:pathfragment",
        "//src/test/java/com/google/devtools/build/lib/testutil",
        "//src/test/java/com/google/devtools/build/lib/testutil:JunitUtils",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_library(
    name = "VfsTests_lib",
    srcs = glob(
        [
            "*.java",
            "inmemoryfs/*.java",
        ],
        exclude = ALL_WINDOWS_TESTS + [
            "SymlinkAwareFileSystemTest.java",
        ],
    ),
    deps = [
        ":SymlinkAwareFileSystemTest",
        ":testutil",
        "//src/main/java/com/google/devtools/build/lib:build",
        "//src/main/java/com/google/devtools/build/lib/clock",
        "//src/main/java/com/google/devtools/build/lib/skyframe/serialization",
        "//src/main/java/com/google/devtools/build/lib/skyframe/serialization/testutils",
        "//src/main/java/com/google/devtools/build/lib/unix",
        "//src/main/java/com/google/devtools/build/lib/vfs:pathfragment",
        "//src/main/java/com/google/devtools/build/lib/vfs/inmemoryfs",
        "//src/test/java/com/google/devtools/build/lib/testutil",
        "//src/test/java/com/google/devtools/build/lib/testutil:JunitUtils",
        "//src/test/java/com/google/devtools/build/lib/testutil:TestThread",
        "//src/test/java/com/google/devtools/build/lib/testutil:TestUtils",
        "//src/test/java/com/google/devtools/build/lib/vfs/util",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

java_test(
    name = "VfsTests",
    size = "medium",
    tags = ["no_windows"],
    test_class = "com.google.devtools.build.lib.AllTests",
    runtime_deps = [
        ":VfsTests_lib",
        "//src/test/java/com/google/devtools/build/lib:test_runner",
    ],
)

java_library(
    name = "VfsWindowsTests_lib",
    srcs = CROSS_PLATFORM_WINDOWS_TESTS + ["PathAbstractTest.java"],
    deps = [
        "//src/main/java/com/google/devtools/build/lib/clock",
        "//src/main/java/com/google/devtools/build/lib/vfs",
        "//src/main/java/com/google/devtools/build/lib/vfs:pathfragment",
        "//src/main/java/com/google/devtools/build/lib/vfs/inmemoryfs",
        "//src/test/java/com/google/devtools/build/lib/testutil",
        "//src/test/java/com/google/devtools/build/lib/testutil:JunitUtils",
        "//third_party:guava",
        "//third_party:guava-testlib",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

# Tests windows specific path handling on Unix.
java_test(
    name = "VfsWindowsTests",
    size = "small",
    jvm_flags = [
        "-Dblaze.os=Windows",
        "-Dbazel.windows_unix_root=C:/fake/msys",
    ],
    test_class = "com.google.devtools.build.lib.AllTests",
    runtime_deps = [
        ":VfsWindowsTests_lib",
        "//src/test/java/com/google/devtools/build/lib:test_runner",
    ],
)

java_library(
    name = "testutil",
    testonly = 1,
    srcs = ["FileSystemTest.java"],
    deps = [
        "//src/main/java/com/google/devtools/build/lib:util",
        "//src/main/java/com/google/devtools/build/lib/unix",
        "//src/main/java/com/google/devtools/build/lib/vfs",
        "//src/main/java/com/google/devtools/build/lib/vfs:pathfragment",
        "//src/test/java/com/google/devtools/build/lib/testutil",
        "//src/test/java/com/google/devtools/build/lib/testutil:JunitUtils",
        "//src/test/java/com/google/devtools/build/lib/testutil:TestUtils",
        "//third_party:guava",
        "//third_party:junit4",
        "//third_party:truth",
    ],
)

test_suite(
    name = "windows_tests",
    tags = [
        "-no_windows",
        "-slow",
    ],
)

test_suite(
    name = "all_windows_tests",
    tests = [
        ":windows_tests",
    ],
)

##
# Copyright 2013 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
##

workspace(name = "com_google_closure_templates")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:jvm.bzl", "jvm_maven_import_external")

RULES_JVM_EXTERNAL_TAG = "4.2"
RULES_JVM_EXTERNAL_SHA = "cd1a77b7b02e8e008439ca76fd34f5b07aecb8c752961f9640dea15e9e5ba1ca"

http_archive(
    name = "rules_jvm_external",
    strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
    sha256 = RULES_JVM_EXTERNAL_SHA,
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG,
)

load("@rules_jvm_external//:repositories.bzl", "rules_jvm_external_deps")

rules_jvm_external_deps()

load("@rules_jvm_external//:setup.bzl", "rules_jvm_external_setup")

rules_jvm_external_setup()

load("@rules_jvm_external//:defs.bzl", "maven_install")
load("@rules_jvm_external//:specs.bzl", "maven")

SERVER_URLS = [
    "https://mirror.bazel.build/repo1.maven.org/maven2",
    "https://repo1.maven.org/maven2",
]

maven_install(
    artifacts = [
        "args4j:args4j:2.33",
        "com.google.code.findbugs:jsr305:3.0.2",
        "com.google.code.gson:gson:2.9.0",
        "com.google.common.html.types:types:1.0.8",
        "com.google.errorprone:error_prone_annotations:2.14.0",
        "com.google.escapevelocity:escapevelocity:0.9.1",
        "com.google.flogger:flogger:0.7.4",
        "com.google.flogger:flogger-system-backend:0.7.4",
        "com.google.flogger:google-extensions:0.7.4",
        "com.google.guava:guava:31.1-jre",
        maven.artifact(
            "com.google.guava",
            "guava-testlib",
            "31.1-jre",
            testonly = True
        ),
        "com.google.inject:guice:5.1.0",
        maven.artifact(
            "com.google.truth",
            "truth",
            "1.1.3",
            testonly = True,
        ),
        maven.artifact(
            "com.google.truth.extensions",
            "truth-java8-extension",
            "1.1.3",
            testonly = True
        ),
        "com.ibm.icu:icu4j:71.1",
        "javax.inject:javax.inject:1",
        maven.artifact(
            "junit",
            "junit",
            "4.13.2",
            testonly = True,
        ),
        "net.java.dev.javacc:javacc:6.1.2",
        "org.apache.ant:ant:1.10.12",
        "org.json:json:20211205",
        "org.ow2.asm:asm:9.3",
        "org.ow2.asm:asm-commons:9.3",
        "org.ow2.asm:asm-tree:9.3",
        "org.ow2.asm:asm-util:9.3",
    ],
    maven_install_json = "//:maven_install.json",
    override_targets = {
        "com.google.auto.value:auto-value": "@com_google_auto_value_auto_value",
        "com.google.auto.value:auto-value-annotations": "@com_google_auto_value_auto_value_annotations",
        "com.google.protobuf:protobuf-java": "@com_google_protobuf//:protobuf_java",
    },
    repositories = SERVER_URLS,
    strict_visibility = True,
)

load("@maven//:defs.bzl", "pinned_maven_install")

pinned_maven_install()

jvm_maven_import_external(
    name = "com_google_auto_value_auto_value",
    artifact = "com.google.auto.value:auto-value:1.9",
    artifact_sha256 = "fd39087fa111da2b12b14675fee740043f0e78e4bfc7055cf3443bfffa3f572b",
    extra_build_file_content = """
java_plugin(
    name = "AutoAnnotationProcessor",
    output_licenses = ["unencumbered"],
    processor_class = "com.google.auto.value.processor.AutoAnnotationProcessor",
    tags = ["annotation=com.google.auto.value.AutoAnnotation;genclass=${package}.AutoAnnotation_${outerclasses}${classname}_${methodname}"],
    deps = [":processor"],
)

java_plugin(
    name = "AutoOneOfProcessor",
    output_licenses = ["unencumbered"],
    processor_class = "com.google.auto.value.processor.AutoOneOfProcessor",
    tags = ["annotation=com.google.auto.value.AutoValue;genclass=${package}.AutoOneOf_${outerclasses}${classname}"],
    deps = [":processor"],
)

java_plugin(
    name = "AutoValueProcessor",
    output_licenses = ["unencumbered"],
    processor_class = "com.google.auto.value.processor.AutoValueProcessor",
    tags = ["annotation=com.google.auto.value.AutoValue;genclass=${package}.AutoValue_${outerclasses}${classname}"],
    deps = [":processor"],
)

java_plugin(
    name = "MemoizedValidator",
    output_licenses = ["unencumbered"],
    processor_class = "com.google.auto.value.extension.memoized.processor.MemoizedValidator",
    deps = [":processor"],
)

java_library(
    name = "com_google_auto_value_auto_value",
    exported_plugins = [
        ":AutoAnnotationProcessor",
        ":AutoOneOfProcessor",
        ":AutoValueProcessor",
        ":MemoizedValidator",
    ],
    exports = ["@com_google_auto_value_auto_value_annotations"],
)
""",
    generated_rule_name = "processor",
    server_urls = SERVER_URLS,
    exports = ["@com_google_auto_value_auto_value_annotations"],
)

# This isn't part of the maven_install above so we can set a custom visibility.
jvm_maven_import_external(
    name = "com_google_auto_value_auto_value_annotations",
    artifact = "com.google.auto.value:auto-value-annotations:1.9",
    artifact_sha256 = "fa5469f4c44ee598a2d8f033ab0a9dcbc6498a0c5e0c998dfa0c2adf51358044",
    default_visibility = [
        "@com_google_auto_value_auto_value//:__pkg__",
        "@maven//:__pkg__",
    ],
    neverlink = True,
    server_urls = SERVER_URLS,
)

# Apache 2.0
http_archive(
    name = "rules_java",
    url = "https://github.com/bazelbuild/rules_java/releases/download/4.0.0/rules_java-4.0.0.tar.gz",
    sha256 = "34b41ec683e67253043ab1a3d1e8b7c61e4e8edefbcad485381328c934d072fe",
)
load("@rules_java//java:repositories.bzl", "rules_java_dependencies", "rules_java_toolchains")
rules_java_dependencies()
rules_java_toolchains()

# Apache 2.0
http_archive(
    name = "rules_proto",
    sha256 = "e017528fd1c91c5a33f15493e3a398181a9e821a804eb7ff5acdd1d2d6c2b18d",
    strip_prefix = "rules_proto-4.0.0-3.20.0",
    urls = [
        "https://github.com/bazelbuild/rules_proto/archive/refs/tags/4.0.0-3.20.0.tar.gz",
    ],
)

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()

rules_proto_toolchains()

http_archive(
    name = "rbe_default",
    sha256 = "c5f784299a319f39bbe51e38a49e2e779cc2eb3b1b06ce4e71d5e537ac95e134",
    urls = ["https://storage.googleapis.com/rbe-toolchain/bazel-configs/bazel_4.1.0/rbe-ubuntu1604/latest/rbe_default.tar"],
)

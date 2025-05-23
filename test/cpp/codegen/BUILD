# Copyright 2017 gRPC authors.
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

load("//bazel:grpc_build_system.bzl", "grpc_cc_binary", "grpc_cc_test", "grpc_package", "grpc_sh_test")

licenses(["notice"])

grpc_package(name = "test/cpp/codegen")

grpc_cc_test(
    name = "codegen_test_full",
    srcs = ["codegen_test_full.cc"],
    external_deps = [
        "gtest",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:grpc++",
        "//test/core/test_util:grpc_test_util",
    ],
)

grpc_cc_test(
    name = "codegen_test_minimal",
    srcs = ["codegen_test_minimal.cc"],
    external_deps = [
        "gtest",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:grpc++",
        "//test/core/test_util:grpc_test_util",
    ],
)

grpc_cc_test(
    name = "proto_utils_test",
    srcs = ["proto_utils_test.cc"],
    external_deps = [
        "gtest",
        "protobuf",
    ],
    tags = [
        # TODO: some include error (may already be fixed)
        "grpc:fails-internally",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:grpc++",
        "//test/core/test_util:grpc_test_util",
    ],
)

grpc_cc_binary(
    name = "golden_file_test",
    testonly = True,
    srcs = ["golden_file_test.cc"],
    external_deps = [
        "absl/flags:flag",
        "gtest",
    ],
    tags = [
        # TODO: Refers to a package that doesn't exist internally.
        "grpc:fails-internally",
    ],
    deps = [
        "//:grpc++",
        "//test/core/test_util:grpc_test_util",
        "//test/cpp/util:test_config",
    ],
)

genrule(
    name = "copy_compiler_test_grpc_pb_h",
    srcs = ["//src/proto/grpc/testing:_compiler_test_cc_grpc_grpc_codegen"],
    outs = ["compiler_test.grpc.pb.h"],
    cmd = "cat $(GENDIR)/src/proto/grpc/testing/compiler_test.grpc.pb.h > $@",
)

genrule(
    name = "copy_compiler_test_mock_grpc_pb_h",
    srcs = ["//src/proto/grpc/testing:_compiler_test_cc_grpc_grpc_codegen"],
    outs = ["compiler_test_mock.grpc.pb.h"],
    cmd = "cat $(GENDIR)/src/proto/grpc/testing/compiler_test_mock.grpc.pb.h > $@",
)

grpc_sh_test(
    name = "run_golden_file_test",
    srcs = ["run_golden_file_test.sh"],
    data = [
        ":compiler_test.grpc.pb.h",
        ":compiler_test_golden",
        ":compiler_test_mock.grpc.pb.h",
        ":compiler_test_mock_golden",
        ":golden_file_test",
    ],
    uses_polling = False,
)

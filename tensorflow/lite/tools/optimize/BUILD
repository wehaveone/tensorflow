load("//tensorflow/lite:special_rules.bzl", "tflite_portable_test_suite")
load("//tensorflow:tensorflow.bzl", "tf_cc_test")

package(default_visibility = [
    "//visibility:public",
])

licenses(["notice"])  # Apache 2.0

exports_files(glob([
    "testdata/*.bin",
]))

cc_library(
    name = "quantization_utils",
    srcs = ["quantization_utils.cc"],
    hdrs = ["quantization_utils.h"],
    deps = [
        "//tensorflow/lite:framework",
        "//tensorflow/lite/c:c_api_internal",
        "//tensorflow/lite/kernels/internal:round",
        "//tensorflow/lite/kernels/internal:types",
        "//tensorflow/lite/schema:schema_fbs",
    ],
)

tf_cc_test(
    name = "quantization_utils_test",
    srcs = ["quantization_utils_test.cc"],
    tags = [
        "tflite_not_portable_android",
        "tflite_not_portable_ios",
    ],
    deps = [
        ":quantization_utils",
        "//tensorflow/lite:framework",
        "//tensorflow/lite/schema:schema_fbs",
        "@com_google_googletest//:gtest",
        "@flatbuffers",
    ],
)

cc_library(
    name = "quantize_weights",
    srcs = ["quantize_weights.cc"],
    hdrs = ["quantize_weights.h"],
    deps = [
        ":quantization_utils",
        "@com_google_absl//absl/memory",
        "@flatbuffers",
        "//tensorflow/lite:framework",
        # TODO(suharshs): Move the relevant quantization utils to a non-internal location.
        "//tensorflow/lite/kernels/internal:tensor_utils",
        "//tensorflow/lite/schema:schema_fbs",
        "//tensorflow/core:tflite_portable_logging",
    ],
)

tf_cc_test(
    name = "quantize_weights_test",
    srcs = ["quantize_weights_test.cc"],
    args = [
        "--test_model_file=$(location //tensorflow/lite/tools/optimize:testdata/single_conv_weights_min_0_max_plus_10.bin)",
    ],
    data = [
        "//tensorflow/lite/tools/optimize:testdata/single_conv_weights_min_0_max_plus_10.bin",
        "//tensorflow/lite/tools/optimize:testdata/weight_shared_between_convs.bin",
    ],
    tags = [
        "tflite_not_portable_android",
        "tflite_not_portable_ios",
    ],
    deps = [
        ":quantize_weights",
        ":test_util",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/lite:framework",
        "//tensorflow/lite/schema:schema_fbs",
        "@com_google_googletest//:gtest",
        "@flatbuffers",
    ],
)

cc_library(
    name = "subgraph_quantizer",
    srcs = ["subgraph_quantizer.cc"],
    hdrs = ["subgraph_quantizer.h"],
    deps = [
        ":quantization_utils",
        "//tensorflow/lite:framework",
        "//tensorflow/lite/core/api",
        "//tensorflow/lite/kernels/internal:round",
        "//tensorflow/lite/kernels/internal:tensor_utils",
        "//tensorflow/lite/schema:schema_fbs",
        "@com_google_absl//absl/memory",
        "@flatbuffers",
    ],
)

cc_library(
    name = "test_util",
    testonly = 1,
    srcs = ["test_util.cc"],
    hdrs = ["test_util.h"],
    deps = [
        "//tensorflow/lite:framework",
        "//tensorflow/lite/core/api",
        "@com_google_googletest//:gtest",
        "@flatbuffers",
    ],
)

tf_cc_test(
    name = "subgraph_quantizer_test",
    srcs = ["subgraph_quantizer_test.cc"],
    args = [
        "--test_model_file=$(location //tensorflow/lite/tools/optimize:testdata/single_conv_weights_min_0_max_plus_10.bin)",
    ],
    data = [
        "//tensorflow/lite/tools/optimize:testdata/multi_input_add_reshape.bin",
        "//tensorflow/lite/tools/optimize:testdata/single_avg_pool_min_minus_5_max_plus_5.bin",
        "//tensorflow/lite/tools/optimize:testdata/single_conv_weights_min_0_max_plus_10.bin",
        "//tensorflow/lite/tools/optimize:testdata/single_conv_weights_min_minus_127_max_plus_127.bin",
        "//tensorflow/lite/tools/optimize:testdata/single_softmax_min_minus_5_max_plus_5.bin",
    ],
    tags = [
        "tflite_not_portable_android",
        "tflite_not_portable_ios",
    ],
    deps = [
        ":subgraph_quantizer",
        ":test_util",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/lite:framework",
        "//tensorflow/lite/schema:schema_fbs",
        "@com_google_googletest//:gtest",
        "@flatbuffers",
    ],
)

cc_library(
    name = "quantize_model",
    srcs = ["quantize_model.cc"],
    hdrs = ["quantize_model.h"],
    deps = [
        ":subgraph_quantizer",
        "//tensorflow/lite:framework",
        "//tensorflow/lite/core/api",
        "//tensorflow/lite/schema:schema_fbs",
        "@com_google_absl//absl/memory",
        "@flatbuffers",
    ],
)

tf_cc_test(
    name = "quantize_model_test",
    srcs = ["quantize_model_test.cc"],
    args = [
        "--test_model_file=$(location //tensorflow/lite/tools/optimize:testdata/single_conv_weights_min_0_max_plus_10.bin)",
    ],
    data = [
        "//tensorflow/lite/tools/optimize:testdata/single_conv_weights_min_0_max_plus_10.bin",
    ],
    tags = [
        "tflite_not_portable_android",
        "tflite_not_portable_ios",
    ],
    deps = [
        ":quantize_model",
        ":test_util",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/lite:framework",
        "//tensorflow/lite/schema:schema_fbs",
        "@com_google_googletest//:gtest",
        "@flatbuffers",
    ],
)

tflite_portable_test_suite()

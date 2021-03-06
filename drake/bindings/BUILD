# -*- python -*-
# This file contains rules for Bazel; see drake/doc/bazel.rst.

load("//tools:cpplint.bzl", "cpplint")
load("//tools:python_lint.bzl", "python_lint")
load("//tools:drake.bzl", "drake_cc_binary")
load("//tools:gurobi.bzl", "gurobi_test_tags")
load("//tools:mosek.bzl", "mosek_test_tags")

package(default_visibility = ["//visibility:private"])

drake_cc_binary(
    name = "python/pydrake/_pydrake_common.so",
    srcs = [
        "pybind11/pydrake_common.cc",
    ],
    copts = [
        "-Wno-#warnings",
        "-Wno-cpp",
        "-Wno-unknown-warning-option",
    ],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/common",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/_pydrake_autodiffutils.so",
    srcs = [
        "pybind11/pydrake_autodiff_types.h",
        "pybind11/pydrake_autodiffutils.cc",
    ],
    copts = [
        "-Wno-#warnings",
        "-Wno-cpp",
        "-Wno-unknown-warning-option",
    ],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "@eigen",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/_pydrake_rbtree.so",
    srcs = [
        "pybind11/pydrake_autodiff_types.h",
        "pybind11/pydrake_rbtree.cc",
    ],
    copts = [
        "-Wno-#warnings",
        "-Wno-cpp",
        "-Wno-unknown-warning-option",
    ],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/multibody:rigid_body_tree",
        "//drake/multibody/parsers",
        "@eigen",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/parsers.so",
    srcs = ["pybind11/pydrake_parsers.cc"],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/multibody/parsers",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/solvers/_pydrake_ik.so",
    srcs = ["pybind11/pydrake_ik.cc"],
    copts = [
        "-Wno-#warnings",
        "-Wno-cpp",
        "-Wno-unknown-warning-option",
    ],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/multibody:inverse_kinematics",
        "//drake/multibody:rigid_body_tree",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/solvers/_pydrake_mathematicalprogram.so",
    srcs = [
        "pybind11/pydrake_mathematicalprogram.cc",
        "pybind11/pydrake_symbolic_types.h",
    ],
    copts = [
        "-Wno-#warnings",
        "-Wno-cpp",
        "-Wno-unknown-warning-option",
    ],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/common:symbolic",
        "//drake/solvers:mathematical_program",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/solvers/gurobi.so",
    srcs = ["pybind11/pydrake_gurobi.cc"],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/solvers:mathematical_program",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/solvers/mosek.so",
    srcs = ["pybind11/pydrake_mosek.cc"],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/solvers:mathematical_program",
        "@pybind11",
    ],
)

drake_cc_binary(
    name = "python/pydrake/_pydrake_symbolic.so",
    srcs = [
        "pybind11/pydrake_symbolic.cc",
        "pybind11/pydrake_symbolic_types.h",
    ],
    copts = [
        "-Wno-#warnings",
        "-Wno-cpp",
        "-Wno-unknown-warning-option",
    ],
    linkshared = 1,
    linkstatic = 1,
    deps = [
        "//drake/common:symbolic",
        "@pybind11",
    ],
)

genrule(
    name = "autogenerated_path_py_genrule",
    srcs = ["python/autogenerated_path.py.in"],
    outs = ["python/pydrake/autogenerated_path.py"],
    cmd = "sed 's#@RELATIVE_PATH_PYDRAKE_DRAKE@#../../..#g' $< > $@",
)

py_library(
    name = "pydrake",
    srcs = [
        "python/pydrake/__init__.py",
        "python/pydrake/autodiffutils.py",
        "python/pydrake/common.py",
        "python/pydrake/forwarddiff.py",
        "python/pydrake/rbtree.py",
        "python/pydrake/solvers/__init__.py",
        "python/pydrake/solvers/ik.py",
        "python/pydrake/solvers/mathematicalprogram.py",
        "python/pydrake/symbolic.py",
        ":autogenerated_path_py_genrule",
    ],
    data = [
        ":python/pydrake/_pydrake_autodiffutils.so",
        ":python/pydrake/_pydrake_common.so",
        ":python/pydrake/_pydrake_rbtree.so",
        ":python/pydrake/_pydrake_symbolic.so",
        ":python/pydrake/parsers.so",
        ":python/pydrake/solvers/_pydrake_ik.so",
        ":python/pydrake/solvers/_pydrake_mathematicalprogram.so",
        ":python/pydrake/solvers/gurobi.so",
        ":python/pydrake/solvers/mosek.so",
    ],
    imports = ["python"],
    visibility = ["//visibility:public"],
)

py_test(
    name = "pydrake_atlas_constructor_test",
    size = "small",
    srcs = ["python/pydrake/test/testAtlasConstructor.py"],
    data = ["//drake/examples/Atlas:models"],
    main = "python/pydrake/test/testAtlasConstructor.py",
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_common_test",
    size = "small",
    srcs = ["python/pydrake/test/testCommon.py"],
    main = "python/pydrake/test/testCommon.py",
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_forward_diff_test",
    size = "small",
    srcs = ["python/pydrake/test/testForwardDiff.py"],
    main = "python/pydrake/test/testForwardDiff.py",
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_mathematical_program_gurobi_solver_test",
    size = "small",
    srcs = ["python/pydrake/test/testGurobiSolver.py"],
    main = "python/pydrake/test/testGurobiSolver.py",
    tags = gurobi_test_tags(),
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_mathematical_program_mosek_solver_test",
    size = "small",
    srcs = ["python/pydrake/test/testMosekSolver.py"],
    main = "python/pydrake/test/testMosekSolver.py",
    tags = mosek_test_tags(),
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_mathematical_program_test",
    size = "small",
    srcs = ["python/pydrake/test/testMathematicalProgram.py"],
    main = "python/pydrake/test/testMathematicalProgram.py",
    tags = gurobi_test_tags(),
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_pr2ik_test",
    size = "small",
    srcs = ["python/pydrake/test/testPR2IK.py"],
    data = ["//drake/examples/PR2:models"],
    main = "python/pydrake/test/testPR2IK.py",
    tags = ["snopt"],
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_rbm_forward_kin_test",
    size = "small",
    srcs = ["python/pydrake/test/testRBTTransformPoints.py"],
    data = ["//drake/examples/Pendulum:models"],
    main = "python/pydrake/test/testRBTTransformPoints.py",
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_rbt_com_test",
    size = "small",
    srcs = ["python/pydrake/test/testRBTCoM.py"],
    data = ["//drake/examples/Pendulum:models"],
    main = "python/pydrake/test/testRBTCoM.py",
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_rbtik_test",
    size = "small",
    srcs = ["python/pydrake/test/testRBTIK.py"],
    data = ["//drake/examples/Pendulum:models"],
    main = "python/pydrake/test/testRBTIK.py",
    tags = ["snopt"],
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_symbolic_variables_test",
    size = "small",
    srcs = ["python/pydrake/test/testSymbolic.py"],
    main = "python/pydrake/test/testSymbolic.py",
    deps = [":pydrake"],
)

py_test(
    name = "pydrake_urdf_parser_test",
    size = "small",
    srcs = ["python/pydrake/test/test_urdf_parser.py"],
    data = ["//drake/examples/PR2:models"],
    main = "python/pydrake/test/test_urdf_parser.py",
    deps = [":pydrake"],
)

cpplint()

python_lint()

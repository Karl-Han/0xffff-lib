> [name=iosmanthus Teng] 备注一下名字, 不然不认得

> [name=Karl Han] 这份修改可以在这里看到[Test branch](https://github.com/Karl-Han/0xffff-lib/tree/test)
# 0xffff-lib

[![Build Status](https://travis-ci.org/0xffff-one/0xffff-lib.svg?branch=master)](https://travis-ci.org/0xffff-one/0xffff-lib)

> [name=iosmanthus Teng] 这个地方会不会不通顺 我可能当时没有怎么想好, 半夜写的. 动机这里
> [name=Alex Hsieh] 我都添美的在gayhub上改的，想要我直接复制咩，那我直接复制了
## Motivation
`0xffff-lib` is a low-level core library, with a goal of encouraging newbies to become familiar with modern C++ development, core courses of computer science, and how to contribute to open source projects by building a data structure and algorithm library from scratch with the help of testing framework [`googletest`](https://github.com/google/googletest) and building system [`bazel`](https://www.bazel.build/).

## Compilation

### Dependencies

1. Install `bazel`, which is a build system used to build our library: https://docs.bazel.build/versions/master/install.html
2. Install `g++`, which is the default c++ compiler in mainstream Linux distributions.

### Testing

1. `bazel test`: to run all tests.
2. `bazel test tests --test_output=all`: for verbose information.

## Project structure

Currently, our library is quite simple and crude. We need more contributors to enhance and polish it, which is one of the project's motivations: encourage newbies to contribute to open source projects.

```
.
├── README.md
├── src
│   └── collections # Collection type
│       ├── BUILD # Bazel build file
│       └── ...
├── tests # Where testing codes live
│   ├── BUILD
│   └── test-vector.cc
└── WORKSPACE # import extenal dependencies
```

### Tutorial: adding a module

For instance, we are going to add a matrix calculation module to our library. Considering the module is currently non-existent in our library, we need to create a directory called `matrix`. Now our `src` directory looks like this:

```
.
├── collections
│   ├── BUILD
│   └── vector.h
└── matrix
    ├── BUILD
    ├── matrix.cc
    ├── matrix.h
    ├── svd.h
    └── ...
```

As the example above has demonstrated, we need a `BUILD` file to describe how to build the `matrix` module. `bazel` reads the `BUILD` file and determines how the module will be built and its visibility to other modules. Let's see how to write a basic `BUILD` file from scratch.

```bzl
cc_library(
    # Name of our module
    name = "matrix",
    # Source files are those with `.cc` extension
    srcs = glob("[*.cc]"),
    # Includes all header files in `hdrs`
    hdrs = glob(["*.h"]),
    # Visible to other packages (public)
    visibility = ["//visibility:public"],
)
```

Now we have successfully added a module to our library, it's time to add some tests to examine if the module works as expected.
For now, all the tests reside in the `tests` directory. Since `tests` is a package managed by `bazel`, it contains a `BUILD` file. Let's check out its contents:

```bzl
cc_test(
    name = "tests",
    srcs = glob(["*.cc"]),
    deps = [
        # We need to import the module we want to test
        "//src/collections:vector",
        # New module, note "//src/matrix:matrix" is also acceptable
        "//src/matrix",

        // Import `googletest` to our tests.
        "@googletest//:gtest_main",
    ],
)
```

Then, we create a file named `test-matrix.cc`:

```c++
#include "src/matrix/matrix.h"
#include <gtest/gtest.h>

TEST(TestMatrix, TestAdd) {
    Matrix<int> lhs({{1, 2, 3});
    Matrix<int> rhs({{0, 0, 0}};
    Matrix<int> expected({1, 2, 3});

    EXPECT_EQ(expected, lhs + rhs);
}
```

Finally, we can run the test by executing `bazel test` and check if the function(s) of the module works as expected.

> [name=iosmanthus Teng] 下边这两节感觉可以整理一下? 好像可以合并.
## Contribution

> [name=iosmanthus Teng] 已经加了CI, 这里可以修改一下, 说我们已经有了CI, 贡献开PR之后要注意CI的状态, 看有没有通过测试, 如果确定是CI的问题请联系members, @~~坤~~昆霖和我

> [name=Alex Hsieh] 基本改好了，你们查漏补缺一下
### General information
> [name=iosmanthus Teng] 这里写成表好一些 
> [name=iosmanthus Teng] 一步一步
> [name=Alex Hsieh] ok
> 
> [name=iosmanthus Teng] Flow 和 process 哪个好
> [name=Alex Hsieh] 那还是flow吧

As we've mentioned before, members are encouraged to participate in open source projects. Guidelines for contributing to this project are listed below.

### Adding or improving features
If you want to add new features or improve existing ones, please refer to this standard contribution flow:
1. Fork the repository
2. Make changes to your forked repository
3. Open pull requests for other members to review and discuss your changes

Before you open a pull request:
> [name=Alex Hsieh] 这里说的把文件加进哪里来着？未解决
1. Test your code on your machine.
2. Check which files you should commit by adding them to (where?) one by one. (Do NOT track unnecessary files and make corresponding changes to `.gitignore`)
3. Fill in the pull request template.

For pull request form template, please see [this link](#).

CI support is up and running. After creating a pull request, you should keep an eye on the current status of the CI and see if all of your changes pass the tests. If there is a problem with the CI, please contact @iosmanthus or @

> [name=Alex Hsieh] 昆霖你自己加下你的gay号
> [name=Karl Han]什么gay号？
> [name=Alex Hsieh] 给pr和issue的两个模板留了dummy link，你们看下有无必要放指向这两个模板的链接，有就改成真的链接，没有就直接把整句话删掉
### Reporting bugs and submitting feature requests.
If you have found a bug or have some ideas about what this project should do next, open an issue. Issue form template can be found [here](#).

We will categorize the issues by adding tags, including:

* Feature requests. There will detailed documentation about the proposals made by community members, sorted by three difficulties:
    * Easy
    * Medium
    * Hard
* Help wanted

We are considering expanding the variety of tags. If you have any suggestions, feel free to open an issue and propose new tags that could be added.


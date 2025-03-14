<h1>Prometheus bazel rules</h1>

Prometheus/Alertmanager rules for Bazel

- [Initial project setup](#initial-project-setup)
- [Rules](#rules)
  - [promtool_config_test](#promtool_config_test)
  - [promtool_rules_test](#promtool_rules_test)
  - [promtool_unit_test](#promtool_unit_test)
  - [prometheus](#prometheus)
  - [promtool](#promtool)
- [Examples](#examples)

TODO

- integrate alertmanager and amtool into rules and workspace binaries (todo with sergey)
- implement local backfiller (todo with alexander)
- start alertmanager with input configs
- unit test rules and toolchains
- add linux toolchain
- run some binary tests macros against prometheus server and alertmanager for smoke/integration/load testing
- make toolchains work in containers

# Initial project setup

You will need recent [Bazel](https://bazel.build) release, otherwise rules will download and discover dependent required tools

otherwise, refer to [release page](https://github.com/5h4d0w4rt/rules_prometheus/releases/)

# Rules

<!-- Generated with Stardoc: http://skydoc.bazel.build -->

<a name="#promtool_config_test"></a>

## promtool_config_test

<pre>
promtool_config_test(<a href="#promtool_config_test-name">name</a>, <a href="#promtool_config_test-srcs">srcs</a>)
</pre>


Run "promtool check config" against config targets

Example:
```
//examples:test_config_yml

load("@io_bazel_rules_prometheus//prometheus:defs.bzl", "promtool_config_test")
promtool_config_test(
    name = "test_config_yml",
    srcs = ["prometheus.yml"],
)
```

```bash
bazel test //examples:test_config_yml

//examples:test_config_yml                                      PASSED in 0.1s
```


**ATTRIBUTES**


| Name  | Description | Type | Mandatory | Default |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| name |  A unique name for this target.   | <a href="https://bazel.build/docs/build-ref.html#name">Name</a> | required |  |
| srcs |  List of prometheus configuration targets   | <a href="https://bazel.build/docs/build-ref.html#labels">List of labels</a> | required |  |


<a name="#promtool_rules_test"></a>

## promtool_rules_test

<pre>
promtool_rules_test(<a href="#promtool_rules_test-name">name</a>, <a href="#promtool_rules_test-srcs">srcs</a>)
</pre>


Run "promtool check rules" against rules targets

Example:
```
//examples:test_rules_yml
promtool_rules_test(
    name = "test_rules_yml",
    srcs = ["rules.yml"],
)
```

```bash
bazel test //examples:test_rules_yml

//examples:unit_test_rules_yml                                           PASSED in 0.3s
```


**ATTRIBUTES**


| Name  | Description | Type | Mandatory | Default |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| name |  A unique name for this target.   | <a href="https://bazel.build/docs/build-ref.html#name">Name</a> | required |  |
| srcs |  List of Prometheus rules file targets   | <a href="https://bazel.build/docs/build-ref.html#labels">List of labels</a> | required |  |


<a name="#promtool_unit_test"></a>

## promtool_unit_test

<pre>
promtool_unit_test(<a href="#promtool_unit_test-name">name</a>, <a href="#promtool_unit_test-rules">rules</a>, <a href="#promtool_unit_test-srcs">srcs</a>)
</pre>


Run "promtool test rules" against test targets and rules files

Example:
```
//examples:unit_test_rules_yml

load("@io_bazel_rules_prometheus//prometheus:defs.bzl", "promtool_unit_test")
promtool_unit_test(
name = "unit_test_rules_yml",
srcs = [
"tests.yml",
],
rules = ["rules.yml"],
)
```

```bash
bazel test //examples:unit_test_rules_yml

//examples:unit_test_rules_yml                                                PASSED in 0.1s
```



**ATTRIBUTES**


| Name  | Description | Type | Mandatory | Default |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| name |  A unique name for this target.   | <a href="https://bazel.build/docs/build-ref.html#name">Name</a> | required |  |
| rules |  List of Rules-under-Test file targets   | <a href="https://bazel.build/docs/build-ref.html#labels">List of labels</a> | required |  |
| srcs |  List of Prometheus Unit Test file targets   | <a href="https://bazel.build/docs/build-ref.html#labels">List of labels</a> | required |  |


<a name="#prometheus"></a>

## prometheus

<pre>
prometheus(<a href="#prometheus-name">name</a>, <a href="#prometheus-kwargs">kwargs</a>)
</pre>

Prometheus runner which will launch prometheus server

This will emit runnable sh_binary target which will invoke prometheus server with all arguments passed along.
Tool will have access to workspace. It is intended for convenient in-workspace usage by human and not to be invoked programmatically.

Example:
```
load("@io_bazel_rules_prometheus//prometheus:defs.bzl", "prometheus")

package(default_visibility = ["//visibility:public"])

prometheus(
    name = "prometheus",
)
```


**PARAMETERS**


| Name  | Description | Default Value |
| :-------------: | :-------------: | :-------------: |
| name |  A unique name for this target.   |  none |
| kwargs |  Attributes to be passed along   |  none |


<a name="#promtool"></a>

## promtool

<pre>
promtool(<a href="#promtool-name">name</a>, <a href="#promtool-kwargs">kwargs</a>)
</pre>

Promtool runner which will launch promtool

This rule will emit runnable sh_binary target which will invoke promtool binary and all passed arguments along.
    Tool will have access to workspace. It is intended for convenient in-workspace usage by human and not to be invoked programmatically.

Example:
```
//:promtool
load("@io_bazel_rules_prometheus//prometheus:defs.bzl", "promtool")

package(default_visibility = ["//visibility:public"])

promtool(
    name = "promtool",
)
```


**PARAMETERS**


| Name  | Description | Default Value |
| :-------------: | :-------------: | :-------------: |
| name |  A unique name for this target.   |  none |
| kwargs |  Attributes to be passed along   |  none |

# Examples

[Here](https://github.com/5h4d0w4rt/rules_prometheus/tree/main/examples/BUILD)
---
title: Intelligent Test Runner
kind: documentation
further_reading:
  - link: "https://www.datadoghq.com/blog/streamline-ci-testing-with-datadog-intelligent-test-runner/"
    tag: "Blog"
    text: "Streamline CI testing with Datadog Intelligent Test Runner"
  - link: "https://www.datadoghq.com/blog/monitor-ci-pipelines/"
    tag: "Blog"
    text: "Monitor all your CI pipelines with Datadog"
---

## Overview

Intelligent Test Runner is Datadog's test impact analysis solution. It automatically selects and runs only the relevant tests for a given commit based on the code being changed. Significantly reduce time spent testing and overall CI costs, while maintaining test coverage.

{{< img src="continuous_integration/itr_savings.png" alt="Intelligent test runner enabled in a test session showing its time savings.">}}

Intelligent Test Runner works by analyzing your test suite to determine the code each test covers, and then cross-referencing that coverage with the files impacted by a new code change. Datadog uses this information to run a selection of relevant, impacted tests, omitting the ones unaffected by the code change and reducing the overall testing duration.

By minimizing the number of tests run per commit, Intelligent Test Runner reduces the frequency of [flaky tests][1] disrupting your pipelines. This can be particularly frustrating when the test flaking is unrelated to the code change being tested. After enabling Intelligent Test Runner for your test services, you can limit each commit to its relevant tests to ensure that flaky tests unrelated to your code change don't end up arbitrarily breaking your build.

### Limitations

There are known limitations in the current implementation of Intelligent Test Runner that can cause it to skip tests that should be run under certain conditions. Specifically, Intelligent Test Runner is not able to detect changes in library dependencies, compiler options, external services or changes to data files in data-driven tests.

To override Intelligent Test Runner and run all tests, add `ITR:NoSkip` (case insensitive) anywhere in your Git commit message. You can also add a list of [excluded branches](#excluded-branches) and [tracked files](#tracked-files).

## Set up a Datadog library

Before setting up Intelligent Test Runner, you must configure [Test Visibility][2] for your particular language. If you are reporting data through the Agent, use v6.40 or 7.40 and later.

{{< whatsnext desc="Choose a language to set up Intelligent Test Runner in Datadog:" >}}
    {{< nextlink href="continuous_integration/intelligent_test_runner/setup/dotnet" >}}.NET{{< /nextlink >}}
    {{< nextlink href="continuous_integration/intelligent_test_runner/setup/java" >}}Java{{< /nextlink >}}
    {{< nextlink href="continuous_integration/intelligent_test_runner/setup/javascript" >}}JavaScript{{< /nextlink >}}
    {{< nextlink href="continuous_integration/intelligent_test_runner/setup/swift" >}}Swift{{< /nextlink >}}
    {{< nextlink href="continuous_integration/intelligent_test_runner/setup/python" >}}Python{{< /nextlink >}}
{{< /whatsnext >}}

## Configuration

Once you have set up your Datadog library for Intelligent Test Runner, configure it from the [Test Service Settings][3] page. Enabling Intelligent Test Runner requires the `Intelligent Test Runner Activation Write` permission.

{{< img src="continuous_integration/itr_overview.png" alt="Intelligent test runner enabled in test service settings in the CI section of Datadog." style="width:80%;">}}

### Excluded branches

Due to the [limitations](#limitations) described above, the default branch of your repository is automatically excluded from having Intelligent Test Runner enabled. Datadog recommends this configuration to ensure that all of your tests run prior to reaching production.

If there are other branches you want to exclude, add them on the Test Service Settings page. The query bar supports using the wildcard character `*` to exclude any branches that match, such as `release_*`.

### Tracked files

Additionally, you may specify a set of tracked files. Intelligent Test Runner runs all tests if any of these files change.

You may also use the `*` and `**` wildcard characters to match multiple files or directories. For instance, `**/*.mdx` matches any `.mdx` file in the repository.

We recommend that you use tracked files for your dependency files (for example, `package.json`, `requirements.txt`) and for data files from data-driven tests, such as `tests/data/**`.

{{< img src="continuous_integration/itr_configuration2.png" alt="Select branches to exclude and tracked files" style="width:80%;">}}

## Explore test sessions

You can explore the time savings you get from Intelligent Test Runner by looking at the test commit page and test sessions panel.

{{< img src="continuous_integration/itr_commit.png" alt="Test commit page with intelligent test runner" style="width:80%;">}}

{{< img src="continuous_integration/itr_savings.png" alt="Intelligent test runner enabled in a test session showing its time savings." style="width:80%;">}}

When Intelligent Test Runner is active and skipping tests, purple text displays the amount of time saved on each test session or on each commit. The duration bar also changes color to purple so you can quickly identify which test sessions are using Intelligent Test Runner on the [Test Runs][4] page.

## Explore adoption and global savings

Track your organization's savings and adoption of Intelligent Test Runner through the out-of-the-box [Intelligent Test Runner dashboard][5]. The dashboard includes widgets to track your overall savings as well as a per-repository, per-committer, and per-service view of the data. View the dashboard to understand which parts of your organization are using and getting the most out of Intelligent Test Runner.

{{< img src="continuous_integration/itr_dashboard1.png" alt="Intelligent Test Runner dashboard" style="width:80%;">}}

The dashboard also tracks adoption of Intelligent Test Runner throughout your organization.

{{< img src="continuous_integration/itr_dashboard2.png" alt="Intelligent Test Runner dashboard" style="width:80%;">}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /glossary/#flaky-test
[2]: /continuous_integration/tests/
[3]: https://app.datadoghq.com/ci/settings/test-service
[4]: https://app.datadoghq.com/ci/test-runs
[5]: https://app.datadoghq.com/dash/integration/30941/ci-visibility-intelligent-test-runner

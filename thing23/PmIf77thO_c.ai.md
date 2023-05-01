# Testing Your IPFS Gateway Implementation: A Step-by-Step Guide - @galargh

<https://youtube.com/watch?v=PmIf77thO_c>

![image for Testing Your IPFS Gateway Implementation: A Step-by-Step Guide - @galargh](/thing23/PmIf77thO_c.jpg)

## Overview

In this video, Peter from the IPDX team introduces their work on a gateway conformance testing tool aimed at ensuring compliance with the IPFS specification. The talk covers the tool's goals, structure, how to run the tests, and future plans for the project.

## Content

### Introduction

The IPDX team focuses on improving the developer experience within the IPFS ecosystem. They noticed an increase in new HTTP gateway implementations and saw an opportunity to help with a dedicated testing tool to ensure compliance with the IPFS specification.

### Goals and Structure of the Testing Tool

The testing tool is written in Go but designed to be readable and accessible for developers coming from different language backgrounds. Test cases are designed to be easy to understand, with natural language descriptions and hints to help with debugging.

### Running Tests

Tests are run by providing a gateway URL and any additional arguments to the gateway-conformance CLI tool. The test output provides detailed information about any failures, including the name of the failed test, a description of the issue, and any associated error messages. The tool can also generate reports in various formats, such as JSON, XML, HTML, and Markdown.

### Automation and Continuous Integration

The gateway conformance testing tool can be automated using GitHub Actions, running tests on every PR or push to a repository. Peter shows an example workflow definition to demonstrate how to set up the tool for automated testing.

### Current State and Future Plans

The testing tool is already used in three projects (Kubo Gateway, Car Gateway, Bifrost Gateway) and they aim to migrate 100% of Kubo's tests to the new tool. They plan to cover the entire IPFS spec and integrate the tool with the spec contribution process. A workshop is organized for Kubo maintainers to gather feedback on the testing tool and consider its use for testing the Service Worker Gateway.

## Key Takeaways

- The gateway conformance testing tool aims to ensure compliance with the IPFS specification.
- The tool is designed to be language-agnostic, offering an easy-to-understand structure for test cases.
- Running tests is straightforward using the gateway-conformance CLI tool and providing a gateway URL.
- The tool can be automated with GitHub Actions for continuous integration testing.
- The team plans to migrate all of Kubo's tests to the new tool, cover the entire IPFS spec, and integrate the tool with the spec contribution process.
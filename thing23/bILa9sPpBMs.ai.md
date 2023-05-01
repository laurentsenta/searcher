# Automating Kubo's Development Process - @galargh

<https://youtube.com/watch?v=bILa9sPpBMs>

![image for Automating Kubo's Development Process - @galargh](/thing23/bILa9sPpBMs.jpg)

## Overview

In this video, Piotr from the IPDX team at Protocol Labs discusses the automation of Kubo's development process, including the release process, migration from CircleCI to GitHub Actions, implementing self-hosted runners, and monitoring GitHub Actions. The aim is to enhance Kubo's developer experience and provide more time for important tasks.

## Content

### Automating Kubo's Release Process

- Kubo is a Go-based IPFS implementation maintained by IP Stewards.
- Release process was lengthy and had over 150 manual steps.
- New automated release process with Kubo Releaser reduces steps to only 15.
- Release duration significantly reduced with automation.

### Migration from CircleCI to GitHub Actions

- Improved developer experience with integrated and streamlined process.
- Ran CircleCI and GitHub Actions concurrently for two months for comparison.
- Results showed decreased costs and increased reliability with GitHub Actions.
- Implemented self-hosted runners for better performance, reduced wait times, and customization.

### Monitoring GitHub Actions

- Created a GitHub app and configured it to receive webhooks related to GitHub Actions.
- Store raw, unprocessed event data in PostgreSQL database for future analysis.
- Use Grafana to visualize data and gain insight into GitHub Actions performance.

## Key Takeaways

- Automating processes can greatly improve developer experience and reduce errors.
- GitHub Actions provides a more streamlined experience and better integration than CircleCI.
- Self-hosted runners allow for more powerful machines and better task availability.
- Collecting raw data can provide valuable insight into process performance.
- In the future: fully automate release process, implement dashboard alerts, open-source monitoring dashboards, and enhance monitoring for self-hosted instances.
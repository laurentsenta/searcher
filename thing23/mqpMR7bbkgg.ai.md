# Implementations Showcase: Helia - IPFS for the JS and Browser Environments - achingbrain

<https://youtube.com/watch?v=mqpMR7bbkgg>

![image for Implementations Showcase: Helia - IPFS for the JS and Browser Environments - achingbrain](/thing23/mqpMR7bbkgg.jpg)

## Overview

In this video, Alex (achingbrain) from IP Stewards team at Protocol Labs introduced a new implementation of IPFS called Helia. The aim is to create a more usable and better-designed IPFS solution, specifically for JavaScript. While it shares common components with existing JSIPFS, Helia offers a redesigned, top-level API with more flexibility and functionality. Alex encouraged users to discuss their use cases and open issues about Helia and its integration with various technologies.

## Content

Hello everyone! My name is Alex, and I am achingbrain on the internet. I am part of the IP Stewards team at Protocol Labs, and I am responsible for JSIPFS and JSLibP2P. Today, I want to talk to you about Helia, a new implementation of IPFS, entirely written in JavaScript.

Helia aims to apply the learnings from the past five years of IPFS development and reimagine what a new implementation could look like. It shares many components with the existing JSIPFS, such as libp2p, bitswap, UnixFS, but its top-level APIs have been entirely redesigned to be more ergonomic and usable, focusing on users' needs when integrating IPFS with their applications.

Some key points about Helia:

- It is smaller, with less duplication.
- More composable, allowing users to combine its parts to create different functionality.
- More observable, enabling users to dig deeper and understand what's happening if something isn't working.

Helia targets various environments, such as Node, browsers, Electron, and Deno (once certain APIs are implemented). If you want to learn more about Helia, I have two talks lined up in the IPFS on the web track:

1. "Hello Helia" at 1:30 PM
2. "Performance considerations" at 2:00 PM

These talks will be held in one of the rooms over there, and I encourage you to meet and discuss use cases, problems, and open issues about Helia. There are two repositories for you to explore:

- The [Helia repo](https://github.com/achingbrain/helia) contains the source code and documentation.
- The [examples repo](https://github.com/achingbrain/helia-examples) features examples on how to integrate Helia with different bundlers, technologies, and tasks.

Please open issues and share your thoughts about Helia. Thank you very much for your time!
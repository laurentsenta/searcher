# Overview

In this video, Alex, who is a part of the IP Stewards team at Protocol Labs, talks about Helia, a new implementation of IPFS, all written in JavaScript. The main aim of this implementation is to take the learnings of the last five years and create a more ergonomic, usable, and smaller implementation. Helia shares many components with the existing JSIPFS, but the top-level APIs have been completely redesigned to make integration with applications easier. The implementation is targeting Node, browsers, Electron, and hopefully Deno, making it versatile. Alex invites viewers to check out his talks, which will provide more information about Helia's benefits and performance considerations. Additionally, he encourages users to provide feedback and report issues to make Helia more usable.

# Helia - IPFS for the JS and Browser Environments

Helia is a new implementation of IPFS using JavaScript, which has been designed to integrate more seamlessly with applications. The primary goal of Helia is to provide a more ergonomic, more usable, and smaller implementation of IPFS. 

The implementation shares many components with the existing JSIPFS, including libp2p, bitswap, UnixFS, and others. Still, the top-level APIs have been completely redesigned and optimized for use cases specific to JavaScript and browser environments. The result is that Helia is much more composable, more observable, and more focused on usability, making integration with applications straightforward.

One of the main challenges for previous JSIPFS implementations was trying to mimic the features and implementation details of the existing IPFS implementation, without considering its suitability for JavaScript and browser environments. This is where Helia comes in - as a completely new implementation which has been designed from scratch to optimize JavaScript and browser environments.

In addition, Helia targets a wide range of environments, including Node, browsers, Electron, and Deno. The implementation is also smaller and more focused on integration, which makes it ideal for use cases where size and integration are crucial factors.

Alex invites viewers to attend his talks about Helia for more details on the benefits and performance considerations of the implementation. He encourages users to give their feedback and report issues to make Helia more usable. There are two repositories associated with Helia: the Helia repository, where all the source code is available, and the examples repository, with a few pre-set examples available.
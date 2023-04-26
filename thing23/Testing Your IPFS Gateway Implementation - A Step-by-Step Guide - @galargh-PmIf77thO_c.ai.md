# Overview

In this video, Peter from Protocol Apps introduces the Gateway Conformance Testing Tool developed by the IPDX team to help developers ensure their HTTP gateway implementation conforms to the IPFS specification. He explains the purpose of HTTP gateways, the challenges in testing them, and the features of the testing tool. Peter also demonstrates how to run the tool in a step-by-step guide, highlights some of the benefits of the tool, and shares plans for future improvements.

# Testing Your IPFS Gateway Implementation: A Step-by-Step Guide

Peter from Protocol Apps explains the purpose of HTTP gateways and how the IPDX team has developed the Gateway Conformance Testing Tool to test the implementation of HTTP gateways conforming to the IPFS specification. One of the main goals of the tool is to ensure compliance with the specification. Initially, developers had to implement their own tests, but the tool automates the testing process.

The testing tool is written in Go and has static fixtures to define data to be requested from the gateway. Peter demonstrates a particular test case that takes car support in gateways as an example. The tool has features to help with debugging, such as assigning hints to a check
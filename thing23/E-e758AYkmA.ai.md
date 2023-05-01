# Delta Chat and IRO: Combining Email-based Messaging App and a Content-addressable System

<https://youtube.com/watch?v=E-e758AYkmA>

![Delta Chat and IRO](/thing23/E-e758AYkmA.jpg)

## Overview

In this video, Floris Bruynooghe discusses the integration of Delta Chat, an email-based messaging application, with IRO, a content-addressable system using verified streaming. He goes over the process of integrating IRO with Delta Chat, while mentioning the challenges faced throughout the implementation and the winning solutions to overcome them.

## Content

Delta Chat is an email-based messenger that utilizes email infrastructure, while IRO is a content-addressable system focused on transmitted data using verified streaming. Combining Delta Chat and IRO paves the way to develop a solution for transferring private keys on devices with the former. The solution involved the use of IRO to create a collection of data to be transferred as local networks were being set up.

The challenges faced throughout the implementation process included:

- Running into issues with newer dependencies and mitigating them by patching upstreams.
- The need for cross-network connectivity.
- The necessity of sending multiple addresses in QR codes.
- Ensuring smooth progress bars for users.

The eventual product resolved many of these issues, though questions about the advantages of content-addressing remained.

## Key Takeaways

- Delta Chat and IRO are both designed with compatible goals in mind, using technologies such as Rust, Java, and Node.js.
- Combining the two establishes a content-addressable system for secure messaging in IMAP-supported email servers.
- Though implementation faced several challenges, solutions were found, resulting in a system that is both content-addressable and authenticated.
- Room for improvement includes network connectivity across devices, user comfort with QR code-based authentication, and justifying content-addressing for the overall user experience.
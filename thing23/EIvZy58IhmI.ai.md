# UCAN Too - A Workshop on User-Controlled Authorization Networks (UCAN)

<https://youtube.com/watch?v=EIvZy58IhmI>

![image for UCAN Too - Irakli Gozalishvili & Alan Shaw](/thing23/EIvZy58IhmI.jpg)

## Overview

In this workshop, Irakli Gozalishvili and Alan Shaw provide an introduction to User-Controlled Authorization Networks (UCAN) and demonstrate how to utilize UCAN within a hands-on coding environment.

## What is UCAN?

UCAN stands for User-Controlled Authorization Networks. They are an extension of JSON Web Tokens (JWT) and are focused on providing the user complete control over their authorization. Unlike traditional API keys or authorization servers, UCANs include all the information about what a user is allowed to do directly within the token itself.

## UCAN Components

A UCAN token consists of the following fields:

- `Issuer`: Identity of the UCAN creator.
- `Audience`: Identity of the recipient.
- `Expiry`: Timestamp specifying validity.
- `Attenuations`: List of delegated capabilities.
- `With`: Resource associated with the delegated capabilities.
- `Proof`: List of nested UCANs proving the issuer's capability to delegate.

## Key Takeaways

- UCANs allow for delegation of specific capabilities, wildcard capabilities, or multiple capabilities.
- These tokens utilize Audience and Caveats fields for added security and to narrow the scope of what a delegated capability entails.
- UCAN tokens support both short-lived and future-dated expiry situations.
- UCAN RPC (Remote Procedure Call) is being used for UCAN authorization and invocations in various applications.
- The UCAN Working Group on GitHub provides resources, libraries, and important information about UCAN.
- UCANto is a useful library for working with the UCAN invocation spec and is used in Web3 Storage applications.

## Workshop

Participants worked with observable notebooks to make invocations to UCAN-based servers and gain points for correctly utilizing and navigating UCAN tokens and processes. The leaderboard tracked their progress and determined the winners.
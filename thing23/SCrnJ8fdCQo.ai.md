# Are We Interplanetary Yet? - Building an IPFS Implementation for Space

<https://youtube.com/watch?v=SCrnJ8fdCQo>

![image for Are We Interplanetary Yet? - Ryan Plauche](/thing23/SCrnJ8fdCQo.jpg)

## Overview

In this video, Ryan Plauche discusses the project of building an IPFS (InterPlanetary File System) implementation for space. There are two main aspects: the development of a demonstration mission and the long-term goal of an SDK for using IPFS in space. The video covers the overall project, the progress, challenges and future plans.

## Project Timeline and Goals

- May 2021: Filecoin announced plans to work with Lockheed Martin to put IPFS in space.
- November 2021: Development of the software side began.
- 2022: Mission selected (LM400 demonstration mission) and announced in January 2023.
- Summer 2023: Ground testing of the software.
- Late 2023: Hardware launched into space for actual tests.

## Demonstration Mission

The demonstration mission aims to prove the feasibility of using IPFS to transfer data to and from space. The high-level process involves:

1. Sending a CID (Content Identifier) from the ground to the IPFS running on a satellite.
2. The satellite recognizes the CID, retrieves the data associated with it, and sends it back to Earth.
3. The data reaches the ground and is sent to the broader IPFS network.

## Myceli, Radio Communication, and Hyphy

Myceli is a Rust-based IPFS implementation for space that can import and export files as DAGs (Directed Acyclic Graphs), send and receive blocks, and primarily communicates over UDP (User Datagram Protocol). The radio communication uses UDP as an abstraction layer to separate concerns between the IPsFs protocol and the hardware side.

Hyphy acts as a bridge between Myceli and Qubo (a local IPFS node) and syncs blocks between them.

## Current Challenges and Future Roadmap

Some of the current areas for improvement include:

- More robust data transfer and file handling.
- Adapting to satellite constellations and ground station networks for peer-to-peer communication in space.

The timeline for this project includes:

- Near-term focus: Incorporating pass data, baseline metrics and testing with larger files.
- Mid-term horizon: Expanding network robustness, working with hardware partners and developing peer-to-peer strategies.


## Key Takeaways

- The goal of the project is building an IPFS implementation for space and demonstrating its feasibility in the LM400 mission.
- Myceli, Radio Communication, and Hyphy are core components of the project.
- Ongoing challenges include enhancing data transfer and file handling and expanding network robustness for more complex satellite constellations.
- Collaborations, testing and contributions from the IPFS community are crucial in furthering the project.
# Are We Interplanetary Yet? Designing IPFS for Space - Ryan Plauche

<https://youtube.com/watch?v=v1W66lYBJB0>

![image for Are We Interplanetary Yet? Designing IPFS for Space - Ryan Plauche](/thing23/v1W66lYBJB0.jpg?raw=true)

## Overview
In this video, Ryan Plauche from Little Bear Labs introduces the new IPFS implementation, MySelli, and gives an insight into the challenges faced in deploying IPFS in space.

## Article
Ryan Plauche addresses the complexity involved in building an InterPlanetary File System suitable for space deployment. Unlike traditional cloud storage IPFS, a satellite's unreliable connection and hardware constraints require a fundamental reimagining of how data is transferred. Therefore, MySelli, the new IPFS implementation, has been built as a building block for deploying IPFS into space. 

MySelli is focused on content-addressable data, which currently consists of finding a way to move the data across a radio link from the satellite to the ground station. This promising beginning will see MySelli start running in a satellite in orbit around Earth later this year, with a demonstration mission to transmit data from a single satellite to a ground station. This mission will work towards seeing IPFS successfully integrated with space to big IPFS, a DAG and file that can be used here on earth.

Plauche explains his goal is to provide engineers with a platform for iteration, a better peer-to-peer solution, and data transfer protocol that imitates or approximates some of the satellite's hardware constraints. Space constraints and a lack of flexibility in cloud servers are the focus, so that a more general solution can be built that can be tested on the ground and in space.

MySelli is functionally complete with the ability to ship bytes or blocks from a satellite over a radio to a ground station and resolve that as a complete DAG and file. The project is currently available on GitHub as part of the IPFS shipyard organization and also in the space channel on the FogCoin Slack. 

Ryan Plauche is optimistic about the progress made thus far and encourages anyone interested to get in touch with him to discuss the challenges faced in creating an IPFS that is suitable for space deployment. 

## Key Takeaways
- IPFS needs a fundamental overhaul to operate in space deployment.
- Traditional IPFS will not work with unreliable satellite connections and hardware constraints.
- MySelli is the new IPFS implementation suitable for building blocks for deploying IPFS into space.
- Engineers can use MySelli as a platform for iteration without worrying about the satellite's hardware constraints.
- The satellite running MySelli will be in orbit around the Earth later this year.
- MySelli functional for shipping bytes or blocks from a satellite over a radio to a ground station.
- MySelli is currently available on GitHub and FogCoin Slack.
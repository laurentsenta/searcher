# Repco: Exchanging Community Media and Metadata Over IPLD - Franz Heinzmann

<https://youtube.com/watch?v=Qci5Fo_uwbk>

![Repco - Exchanging Community Media and Metadata Over IPLD - Franz Heinzmann](/thing23/Qci5Fo_uwbk.jpg)

## Overview

In this video, Franz Heinzmann presents the Repco project, a collaboration platform for independent community media outlets in Europe. The project aims to provide a shared repository for media content and metadata, allowing these outlets to exchange content, audiences, and technologies while preserving their autonomy and avoiding reliance on big tech platforms.

## Article

Hello everyone, my name is Franz Heinzmann, and I work with the Arso Collective in Freiburg, Germany. Today, I will be sharing with you our project called Repco, which we have been working on for the past few months. 

Community media refers to small media outlets such as community radio stations, independent journalistic publishers, DIY video sites, and archives of social movements. These outlets often face unique challenges, as they usually have limited resources and are reluctant to rely on large tech companies such as YouTube. 

The Repco project began a year ago when several independent community media outlets from across Europe came together at a conference in Austria. The participants expressed a desire to break out of their separate platforms and create a shared repository for their media to compete more effectively with big tech platforms.

In order to achieve this, Repco uses the following workflow:

1. Ingest existing content from various sources such as RSS, ActivityPub, and WordPress APIs.
2. Store the original content with content IDs assigned to them.
3. Map the original content to a shared data format, enabling standardized querying.

We also came up with a multi-writer system using entity URIs, allowing Repco to handle mutable identifiers and updates.

After following this workflow, the content is indexed and organized into repositories. These repositories are identified by key pairs, which we use decentralized identifiers (DIDs) for. An append-only log of commits is created for each repository, making it straightforward to replicate content from one node to another.

Repco also has two APIs for communication; the replication API, which is used to replicate content across nodes, and the GraphQL API, which is used in frontends to display content.

Looking ahead, we are considering splitting the query layer from the data transfer layer, allowing for more expressive and app-specific queries in the future. Additionally, we plan to introduce persistent subscriptions, allowing other services such as transcription or translation to integrate with Repco.

If you are interested in our project, feel free to [contact us](mailto:franz@arso.xyz).

## Key Takeaways

- Repco is a project designed to strengthen collaboration between independent community media outlets in Europe.
- The platform ingests content from various sources, assigns content IDs, and maps the content to a shared data format.
- Repco uses decentralized identifiers and an append-only log of commits to facilitate content replication across nodes.
- The project aims to separate the query layer from the data transfer layer in the future, allowing for more expressive queries and multi-party data transfer.
- Recent developments include work on persistent subscriptions and integration with transcription and translation services.
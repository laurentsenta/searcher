# Overview

In this video, Jorropo, a member of the IP Stewards team at Protocol Labs, talks about RAPID, a new tool he has built to help individuals and organizations download files faster using IPFS primitives. He discusses the current issues with peer-to-peer networks and how Torrent is still considered the fastest way to download files on the internet. He then goes on to explain how RAPID works and how it can make downloading files much faster, even with a low number of users.

## Content

Jorropo starts by introducing himself as a member of the IP Stewards team at Protocol Labs. He then talks about the main goal of Boxo, a library that spun out of Kubo, which aims to help people build applications using IPFS primitives. He then introduces his new tool, RAPID.

Jorropo explains that RAPID is a way to download files from multiple nodes at once, making the download process faster. He says that the current issue with peer-to-peer networks is that they often do not scale well, and the more users there are, the slower the download times become. He then demonstrates this by trying to download a file from the IPFS website and showing how slow it is.

Jorropo then explains how RAPID works and how it can make downloads much faster. He says that RAPID adds multiple nodes to the first block, giving faster times to the first byte. Then, it moves other nodes away from the root node, to other parts of the DAG, allowing for faster downloads even with a low number of users. He talks about the metric used by RAPID to decide where to download blocks from and how it maintains a balance between nodes downloading different parts of the file.

Jorropo then discusses the features that RAPID is missing and the work he plans to do in the future, such as adding content routing, block store caching and faster time
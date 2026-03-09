#uni
The video stream traffic is the major consumer of internet bandwidth.
Content Distribution services are responsible for 80% of residential ISP traffic (data from 2020).

Two challenges:
- how do we scale to ~1 billion users?
- heterogeneity: different users have different capabilities
The solution is **distributed, application-level infrastructure**.

# Coding
We can use redundancy within and between frames to decrease the number of bits used to encode the image.
- **spatial coding**: instead of sending $N$ values of the same color, just send the color and the number if times it repeats ($N$).
- **temporal coding**: instead of sending the complete $i+1$ frame, just send the differences from the $i$ frame
# Challenges of streaming
- server-to-client bandwidth will vary over time.
- packet loss will also vary over time

In an ideal world the servers send one frame every 1/30th of a second, the user receives one frame at the same interval, with a fixed network delay, and displays the frames at the correct framerate.
**Network delay though is variable**! Solution: clients stores the received frames and displays them at the correct framerate with a "video reproduction delay".
# Compression
An important characteristic of video is that it can be compressed, trading quality with lower file sizes.
Today we have algorithms that can compress videos to any desired bit-rate, the higher the bitrate the higher the quality and the overall user viewing experience.

Compressed video typically ranges between 100 kbps to over 10 Mbps for 4k streaming. This amounts to a huge amount of traffic and storage: a single 2 Mbps video of a duration of 67 minutes will consume 1 GB of storage and traffic ($\text{size}=\text{bitrate}*\text{time([s])}$).
In order to provide continuous playout, the network must provide an avera end-to-end throughput to the streaming application that is at least as large as the desired bitrate for the video.

We can use compression to create and store the same video at different bitrates, so that the streaming application (user) can choose which one to request based on the internet throughput it knows can achieve.
# HTTP streaming
The video is simply stored at an HTTP server as an ordinary file with its URL. When an user wants that video, it establishes a TCP connection with the server and issues an HTTP HGET request for that URL.
The server then sends the video file within an HTTP response message, as quickly as the network allows.
On the client side the bytes are **buffered**, once the number of collected bytes achieve a predetermined threshold, the client application begins playback:
the streaming application gets the bytes from the streaming buffer, decompresses them into frames, and plays them back at the right framerate on the user's screen, all while continuing to receive new information (the video) from the server.
This way of streaming has worked well but it has a shortcoming: every user receives the same video, independently from its network capability.
# Dynamic Adaptive Streaming over HTTP (DASH)
In DASH the video gets stored on the server at different bitrates (every version is a different file so it has a different URL), the streaming application dynamically chooses chunks of a few seconds of the video in whatever bitrates it deems adequate.
The HTTP server has a **manifest file**, which provides an URL for each version of the video along with its bitrate.
1. The client requests the manifest file
2. the client requests a chunk of the video at a desired bitrate
3. while downloading the chunk the client measures the received bandwidth and runs a rate determination algorithm to select the chunk to request next.
# Content Distribution Networks (CDNs)
For a content provider the simplest approach would be to just build a single capable server, but has many drawbacks (single point of failure, popular media sent many times over the same network, congestion ecc), thereby the applied strategy is to build servers all around the world, that sort of act like proxies, in which they store the most requested files in their area of operation.
The user ask the generale server where it can find the file and the server redirects it to a server near the client which has that piece of media stored. If there is no such server near the client CDNs utilize the *PULL* approach: the main server will quickly send the media to one of the servers accessible by the client, who will then download the file from the latter. When a server is full it removes the less frequently requested files.

There are two different approaches:
- **Enter deep**: building many servers and incorporate them near access ISPs, improving user perceived delay and overall user experience.
- **Bring home**: building many servers but less than the previous approach and employing them near IXPs. This results in lower maintenance needed compared to "enter deep".
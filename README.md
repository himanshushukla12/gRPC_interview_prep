# gRPC interview Questions.   
<img src="https://raw.githubusercontent.com/himanshushukla12/gRPC_interview_prep/main/image.png" width="500" height="500" alt="Image Description">

   
Certainly, here are concise answers to your questions:   
1. **What is gRPC, and how does it differ from traditional REST APIs?**   
    - gRPC is a high-performance remote procedure call (RPC) framework.   
    - Differs from REST with binary serialization, efficient HTTP/2 transport, and strong typing.   
2. **Core Components of a gRPC service:**   
    - Message types (Protobufs)   
    - Service definition (RPC methods)   
    - Stub (client-side)   
    - Implementation (server-side)   
3. **Protocol Buffers (protobufs) in gRPC:**   
    - Protobufs are a language-agnostic data serialization format.   
    - Used in gRPC to define message formats and service contracts.   
4. **Advantages/Disadvantages of gRPC:**   
    - Advantages: Efficiency, strong typing, multiplexing.   
    - Disadvantages: Complexity for non-binary systems.   
5. **Data Serialization/Deserialization:**   
    - gRPC uses Protocol Buffers for serialization, facilitating efficient binary encoding.   
6. **Types of RPC Methods:**   
    - Unary, Server Streaming, Client Streaming, Bidirectional Streaming.   
7. **Streaming RPCs:**   
    - Unary: Single request, single response.   
    - Server Streaming: Single request, multiple responses.   
    - Client Streaming: Multiple requests, single response.   
    - Bidirectional Streaming: Multiple requests, multiple responses.   
8. **HTTP/2 and gRPC Performance:**   
    - HTTP/2 is the underlying protocol, offering multiplexing and header compression for gRPC's efficiency.   
9. **Security Mechanisms in gRPC:**   
    - Provides transport security (TLS/SSL) and supports various authentication and authorization mechanisms.   
10. **Deadlines and Timeouts in gRPC:**   
    - Deadlines ensure timely responses, preventing indefinite waits.   
    - Timeouts set maximum waiting periods, critical for responsive services.   
11. **Error Handling in gRPC:**   
    - gRPC uses status codes for precise error reporting, helping clients react appropriately.   
12. **Authentication and Authorization in gRPC:**   
    - Authentication can be implemented using various mechanisms.   
    - Authorization is enforced via interceptors based on user roles or permissions.   
13. **Interceptors in gRPC:**   
    - Interceptors are middleware for common concerns (e.g., logging, authentication) in gRPC.   
14. **Versioning of gRPC APIs:**   
    - Use semantic versioning and maintain backward compatibility when evolving APIs.   
15. **Load Balancing and Service Discovery in gRPC:**   
    - gRPC supports client-side load balancing and integrates with service discovery tools.   
16. **Cancellation in gRPC:**   
    - Clients can cancel RPC requests, preventing unnecessary work and resource usage.   
17. **Best Practices for Designing gRPC Services:**   
    - Define clear service boundaries, keep messages small, plan for versioning.   
18. **Programming Languages Supporting gRPC:**   
    - gRPC is available in various languages like Python, Java, C++, and more.   
19. **Scenarios for Choosing gRPC:**   
    - Use gRPC for real-time, high-performance, or strict data format requirements.   
20. **Simple gRPC Chat Application:**   
    - Implement bidirectional streaming to enable real-time chat between clients and a server.   
   
These concise answers should provide you with a good overview of gRPC and its key concepts.   
# HTTP/3   
HTTP/3 aims to significantly improve HTTP/2 in terms of performance. HTTP/3 is based on the [QUIC Transport Protocol](https://tools.ietf.org/html/draft-ietf-quic-transport-29), which is built atop UDP. The idea behind using UDP is to remove the [head-of-line blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking) phenomenon present in TCP.   
In short, HTTP/3 is a relatively straightforward adaptation of HTTP/2 running over QUIC, which actually does the heavy lifting. QUIC is a remarkable protocol in two aspects. The first one is the low-latency connection establishment, e.g. that makes connecting to a website faster.   
At SafetyCulture, we have estimated that using HTTP/3 would save **thousands of hours** in latency to our combined user base. This is the conclusions we have come to based on some micro-benchmarks over WiFi internet connections. Overall, the latency improved by 20 to 40 ms when using HTTP/3.   
Our findings are actually aligned with the outcomes of Cloudflare research while [comparing](https://blog.cloudflare.com/http-3-vs-http-2/) HTTP/2 and HTTP/3. The interesting aspect of this extensive study is that the picture is not so bright, in fact HTTP/3 can perform worse than HTTP/2 depending on the size of the payload.   
However, these studies do not take into account substantial latency improvement when migrating between mobile and WiFi networks. This improvement could have a massive impact on our users since SafetyCulture’s main product, named [iAuditor](https://safetyculture.com/iauditor/), is used in a wide variety of places with disparate quality of internet access (e.g., Ethernet, WiFi, mobile networks). We are still assessing ways to gauge those benefits for our customers.   
The second major advantage of QUIC is the connection migration and resilience to NAT rebinding. As an example, WiFi to mobile network migration is seamless, i.e. there is no need to renegotiate a connection. This is called 0-RTT.   
# gRPC over HTTP/3   
Ultimately, we are trying to run both gRPC and gRPC-Web over HTTP/3 to benefit from QUIC’s performance. gRPC-Web over HTTP/3 is an easy win since modern Web browsers do support HTTP/3, whereas gRPC over HTTP/3 will require fundamental client-side adjustments. In this article, we want to leave doors open for both protocols.   

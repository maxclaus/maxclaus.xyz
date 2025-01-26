+++
title = "Web Real Time Apps"
description = "Implementing simple web real time apps"

[extra]
[extra.shared]
  LinkedIn = "https://www.linkedin.com/posts/maxclaus_web-real-time-apps-activity-7289387792852250626-o3Cu"
+++

At the end of last year I was looking up for something to implement in Rust and came up with the idea of building web real time applications.
It is actually not purely Rust, since most of the applications depend on the client side running in the browser, which is
implemented in Javascript and relies on browser APIs as well. However, it was a nice experience to review some of those web technologies while practicing a bit a of rust
development on the backend.

I am not diving into details on how these technologies work or how to implement each real time communication mentioned on this article. However, the referenced documents and app I built should be self-explanatory: [live app][app_url] (it may take a few seconds for the initial loading) and [source code][git_repository].

## Server-sent events

For unidirectional communication we have [Server-sent events (SSE)][sse_doc] where the browser opens a persistent connection to the server, which sends events
in `text/event-stream` format.

This is a good option when only the server needs to keep sending data to the client, and the client does not need
to send any data back to the server. An real-world example could be a news website sending breaking news updates to users on their homepage.

- [Backend implementation][sse_be].
- [Frontend implementation][sse_fe].

## Web Socket

For bidirectional communication we have [WebSocket][sse_doc] where the browser opens a persistent connection to the server, which events can be sent in both directions. The data also can have different couple of different formats such as string and blob for example.

This is a good option when both the server and client need to exchange data continuously. A real-world example could be a text message chat application.

- [Backend implementation][web_socket_be].
- [Frontend implementation][web_socket_fe].

## WebRTC

[WebRTC][webrtc_doc] is even more exciting because it enables peer-to-peer communication between browsers without requiring us to handle it
ourselves. It goes beyond arbitrary data exchange, by also supporting audio and video streaming, making it
straightforward to implement a basic video chat application. Additionally, WebRTC can be combined with WebSocket, where WebSocket handles
server-managed events, and WebRTC manages peer-to-peer communication.

This is a good option when the application requires media streaming between users. A real-world example could be a video chat application.

- No backend implementation was needed for peer-to-peer media streaming, as WebRTC handles that part for us. However, it does rely on the previous WebSocket
  implementation for data communication managed by our server, such as setting the current user name and notification
  about income calls.
- [Frontend implementation][webrtc_fe].

[app_url]: https://rust-web-real-time.onrender.com
[git_repository]: https://github.com/maxclaus/rust-web-real-time
[sse_doc]: https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events
[sse_be]: https://github.com/maxclaus/rust-web-real-time/blob/main/src/evenstream_handler.rs
[sse_fe]: https://github.com/maxclaus/rust-web-real-time/tree/main/app/src/pages/EventStream
[web_socket_doc]: https://developer.mozilla.org/en-US/docs/Web/API/WebSocket
[web_socket_fe]: https://github.com/maxclaus/rust-web-real-time/tree/main/app/src/pages/Chat
[web_socket_be]: https://github.com/maxclaus/rust-web-real-time/tree/main/src/ws
[webrtc_doc]: https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API
[webrtc_fe]: https://github.com/maxclaus/rust-web-real-time/tree/main/app/src/pages/VideoChat

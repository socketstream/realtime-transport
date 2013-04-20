# Realtime Transport Spec

**Note: This is an emerging idea. Feedback wanted!**

### The problem
There are several very solid WebSocket server modules for Node.js out there. However, despite all doing essentially the same thing (listening on a port, sending messages to a client, emitting an event when a client disconnects, etc) their APIs differ widely. This makes it very difficult to evaluate different transports without changing all your application code.

### The solution
Realtime Transports (RTTs) provide a tiny abstraction layer over existing transport libraries. They allow servers and frameworks to consume a transport without having to worry whether we should call `client.write('msg')` or `client.send('msg')`, or what event to listen out for when a client disconnects. Once your app is built around Realtime Transports, you can instantly move between Engine.IO, SockJS and native WebSockets by only changing one line of code.

### An open standard
The Realtime Transport interface was originally created for [Prism](https://github.com/socketstream/prism), the realtime server that powers SocketStream 0.4. However, RTTs deliberately have no dependencies (other than the underlying transport) so they can easily be used in your own app, realtime server or framework.


## Available Transports

The following modules will be officially maintained and supported:

Module Name | Description | Clients Supported
--- | --- | ---
[**rtt-ws**](https://github.com/socketstream/rtt-ws) | Raw WebSocket server using einaros's blazing-fast library | Browser, Node
[**rtt-engineio**](https://github.com/socketstream/rtt-engineio) | Engine.IO - a WebSocket transport with fallbacks (used in Socket.IO) | Browser, Node
[**rtt-sockjs**](https://github.com/socketstream/rtt-sockjs) | SockJS - a WebSocket transport with fallbacks (used in Meteor) | Browser

There's also a [mock transport](https://github.com/socketstream/rtt-mock) used exclusively for internal testing and benchmarks.


## Implementations

The following modules use Realtime Transports:

[**Prism**](https://github.com/socketstream/prism) a new Realtime Server (as used in SocketStream 0.4)


## What do Realtime Transports do?

* present a standard API: e.g. one way to start a server and send a message
* define the possible events a transport can emit (e.g. `disconnect`, `reconnect_failed`)
* make it easy to send ONLY the client-side code to the browser
* enforce a few conventions (e.g. a `socketId` must be a string)
* automatically reconnect based upon sensible defaults which can be overridden
* aim to offer a consistent API on the server and client where possible


## Server and Client libraries together

Realtime Transports include both the server and client libraries in the same module in a way which is very friendly to Browserify and other CommonJS module systems.

This gives app developers the uber-cool ability to completely switch transports with only one line. It does not prevent authors of Realtime Transports from creating separate modules for the server and client and requiring them both into one 'combo module'.


## API Spec

Coming soon! See one of the `rtt-` modules for the moment.


## FAQs

### Will Realtime Transports support binary data?

Yes - it's coming soon.


### Why not Streams?

I initially used Node Streams but the implementation felt cumbersome because

* many transports don't support streams natively
* you can't use Streams in the browser unless you Browserify the streams lib, adding considerable weight
* using Streams on the server but not in the browser would mean two ways of doing the same thing
* there is no easy way to do one-to-multiple streaming (broadcasting) in Node.js
* clients can't be trusted to gracefully back off, so high watermarks etc are somewhat pointless
* I want to keep everything as fast and simple as possible


## History

This is an evolution of an idea that first appeared in SocketStream 0.3.


## License

MIT
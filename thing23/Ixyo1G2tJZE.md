
# The Incredible Benefits of libp2p + HTTP - Marten Seemann & Marco Munizaga

<https://youtube.com/watch?v=Ixyo1G2tJZE>

![image for The Incredible Benefits of libp2p + HTTP - Marten Seemann & Marco Munizaga](/thing23/Ixyo1G2tJZE.jpg)

## Content

Yeah, first of all, a thanks to chatGPT for suggesting this title. Couldn't have come

up with that myself. But yes, we are talking about the decentralization heaven today. So,

why do people use HTTP? Well, there's a long list of reasons why people do that. HTTP has

been around for 30 years, 30-something years, and it's basically universally supported. Like,

when people talk about the internet, a lot of people just talk about HTTP, and they don't realize that there's a lot of other protocols that are also part of the internet stack.
Specifically, like, HTTP is supported by all browsers. There's command-line tools like curl

that are, like, really popular and built in basically everywhere. You can do HTTP from

cloud edge workers, and probably every programming language around has some kind of HTTP library.

In these 30 years of HTTP, a lot of companies have spun up and built infrastructure to make

that ecosystem better and to make it work more smoothly. There's CDNs, Cartoon Distribution

Networks, that make it super easy. Like, when you have a web server that's serving your website,
you just sign up, you get an account, you put it in front of your web server, and within,

like, five minutes, you have a globally distributed edge network that's serving your website very

close to the user, making your website very fast, because the edge is so close to wherever

the user might be located in the world. And it would be really nice if we could use this for IPFS and for Filecore, right? Because our data is usually content-addressed, which

means that once you have the hash of the data, the content behind it will never change. It's

infinitely cacheable. So, why has this been a problem for the libP2P and IPFS ecosystem

so far? Well, the reason is that you don't do plain HTTP anymore. Nowadays, you do HTTPS.

And to do HTTPS, you need a valid TLS certificate, meaning a TLS certificate signed by a certificate

authority like Let's Encrypt. And you can only get a TLS certificate if you also have

a domain name. Well, technically, it's possible to get IP certificates, like, in practice,

it's quite hard. This means that you can only set up an HTTPS server if you have at least

some level of control over the deployment, which means a domain name that you can get

a TLS certificate for. And that's really easy when you're setting up, like, a big service.

But when you just spin up a Kubo node on, like, a virtual server that you just clicked

five minutes ago, the server doesn't have a domain name. The server doesn't have a TLS
certificate. So, you can't do HTTPS on that server. So, here I have a table of what a

comparison between HTTP and LibP2P. So, we already went over the amazing caching infrastructure

that exists for HTTP. We don't have that for LibP2P. We don't have universal support for

LibP2P. We have implementations in Go and Rust and JavaScript and Nim and Zygnal as

well. But there are still a lot of programming languages where we don't have proper support
for LibP2P. If you want to run it in a cloud EdgeWorker environment or from your command

line, there's no tool for that either. On the other hand, LibP2P comes with some features

that HTTP doesn't have. Namely, net traversal. Because in HTTP, the assumption is that you

have the server on the internet that's publicly reachable, so you don't have to care about nets. Any client will just be able to connect through its firewall or through its net to

your server and it just works. But that's not the situation we have in a decentralised
network like IPFS. We have people who are running nodes in their home networks and they
want to be able to be participants of the network. They want to be able to offer files
to the network and have other nodes connect to them and download these files. So, we need
to do net traversal to get through these nets to have a truly decentralised network. So,

that's not possible with HTTP, but it's possible with LibP2P. And as we talked about in LibP2P,

we don't rely on TLS certificates or more precisely TLS certificates issued by certificate

authorities. So, realising this difference in features between HTTP and LibP2P, this

started a discussion within the LibP2P team about half a year ago. Why can't we have both?

Why can't we combine the benefits of HTTP with the benefits of LibP2P and make use of

all the amazing caching infrastructure that's out there and also make use of the net traversal

that LibP2P gives you? So, the idea would be that you write an application that you

would run on an HTTP server. So, that you would write some HTTP handlers when you get

a GET request for this path, and you do this HTTP response, and when you do a POST request,
then you get a different response. And all of this logic lives inside, and I'm using
Go terminology here, your HTTP surf marks. Once you've put all your logic into the surf

marks, you pass this to an HTTP handler and you run this on your server. This is how you would set up an HTTP server in Go, or basically any other programming language. So, the idea

now is what if you could take all of this logic that you already have, that you already

programmed, that you already tested and that works, and you just pass that to your LibP2P stack. And your LibP2P stack would run your HTTP handlers on top of LibP2P streams. So,

why is this interesting? This is interesting because of what the LibP2P team has been working

on over the last year. Over the last year, the main theme of our work was getting our

connectivity story straight. We added support for web transport and for web RTC, which now

allows users to connect to LibP2P nodes from their browsers without requiring a TLS certificate.

They can, any browser can now connect to any public LibP2P node using web transport. And

to private LibP2P nodes using web RTC. So, now, if we take that HTTP handler, put it

on top of LibP2P streams, now a browser will be able to connect to a LibP2P node that doesn't

have a certificate using web transport. And then it does HTTP over that LibP2P connection.

Some of you might have played around with the go LibP2P HTTP package. It's been around

for a very long time. And the integration that we wrote is actually compatible with
this package. So, we do have a specification for that. It's not merged yet. There's a lot

of discussion ongoing. If you're interested, please head over to our specs repo and participate

in that discussion. So, now, we have a demo for you. And as all of the demos that are

happening during this conference, Marco is doing this demo. Let's talk about service workers before I actually introduce the demo. So, just by a show of hands here, who knows what service workers are? Okay. Cool. That's quite a lot.

So, for most of you, this will be kind of straightforward. So, normally, we think of

service workers as something that can intercept HTTP requests from the browser and serve that

request from an offline cache. This enables, like, these offline apps to work just fine.

But hmm. We're intercepting a request, serving it from some offline cache. What if we could

intercept that and I don't know, something, something. There's a something here, right?
Like, okay, what if we intercept the request and now we ask some random peer in our network

to fulfill that request? We can do this. So, that's the demo. So, let's see. Let's see

if it works. Nope. That's the wrong website. Okay. Here we are. I lost my mouse. Okay.

Here we are. Okay. So, this is so, this is a website I made that loads a service worker

and that service worker intercepts HTTP request calls and then looks at the URL that you gave it. So, in this case, the URL has a hash at the end. And that hash is just a multi-adder
for a peer. And to make this a bit more interesting, this peer is actually Martin's laptop. And

you see a little lock icon there? This is, like, all HTTPS. We're not doing, like, any trust, I ignore the certificate. This is all, like, totally fine by the browser. And so,

I'm gonna make this HTTPS request. You'll see here in the URL that we have, like, this
slash IPFS and this CID. And I'm using this notes because it's a little bigger. So, we're

gonna refresh the page here. And here on the left, you see, okay, it's IPFS docs. Yeah.

So what? Here on the right, you see normal HTTP requests from the Chrome network inspector.

And you're like, okay, yeah. So what? Okay. But look at this. The size here, this is a service worker. These requests were intercepted by the service worker to, like, this JS asset.

Instead of going to this IPFS gateway.io page or server, it's being intercepted, passed

through a web transport connection to Martin's laptop where he gives me his local IPFS docs

and then serves it to me. And then my service worker unpacks that response and gives it to the browser. So, the browser has no idea that we're doing libp2p here. It's just, like,
makes a request to, like, I don't know, give me the IPFS docs. And libp2p under the hood
is like, oh, I know Martin has that. Let me get it from him and gives it to the browser. Browser's like, cool. Looks like a normal HTTP request to me. And all the normal Chrome

debugging tools, dev tools, they all work. It looks like HTTP. Because it is.
All right. Any questions? Yeah. I mean, that works. I had a whole backup thing to do. But

just kidding. That was the only thing. I had no worries there.

One of the big benefits of pulling HTTP into libp2p is to be able to use the server side
infrastructure. You mentioned, Martin, you mentioned it in terms of caching and load balancers and so on. Is there kind of work towards that? Or kind of where are we on that
landscape? Like, can you right now put libp2p connectivity and services behind that infrastructure?

Or is that still this is less HTTP inside of a libp2p stream, but it means turn a libp2p

stream into just using vanilla HTTP as its transport? Yeah. We don't have any proxy that would open libp2p and then put it onto HTTP. The idea

is that you would just reuse the HTTP handler that you have to run it on a server. And you
can then with that thing that's running on top of HTTP, you can use all that caching
infrastructure. And then you use the same handler for peers that are behind NATs to

get the benefits of libp2p. But that doesn't work for middle boxes, right? So if you look at a large deployment on HTTP,

you usually have your server is running on some 10 different web server machines are
running behind some big load balancer. And then that's running behind some caching infrastructure.
And all of those work because you can use HTTP and all of those can introspect the request and can do all of the smart HTTP things. HTTP inside of another transport can't do that.

So I'm talking about the reverse of being able to run your libp2p applications over
HTTP so that you can leverage that. Yeah. So I would make the counter argument that if you're running a service that has

like 10 servers already and you're doing load balancing, then getting the TLS certificate
and the domain name is probably the least of your problems. So just use HTTP in that
case. But like you have libp2p networks like IPFS and Ethereum 2 and Falcon and so on that want

to use these infrastructures and they currently can't.

So I kind of like see this as like the HTTP servers are kind of their own peers as well.

So like the client would see like, oh, you know, I see these multi-adders have the content,

but I also see that this like HTTP server has the content as well. Like I can ask that HTTP server and okay, yes, this HTTP server now has no idea about

like it doesn't have libp2p, it just speaks HTTP, but that's fine because the client knows

that like it's still making the same HTTP request either to like a libp2p node or a

HTTP server. And so we reuse all of that by not changing it, if that makes sense.

But is it actually reusing it because you couldn't speak to those caches and to those
load balancers with your application, right? So like, because you're sort of wrapping the application in a libp2p stream, you're nuking out all of the HTTP logic. Like you're bypassing all that you can't use any of the caches because the libp2p connection will terminate from the source client all the way to your web server at the very end.
And it won't be able to like interface with anything in between. Right. So I guess if your application uses HTTP, right, then like under that HTTP hood, there's like, okay, can it use a normal HTTP server? Yes. It gets all the caching benefits, right? Can it not? Like is that server behind a NAT or does it not have a TLS cert? Okay. Then it has to go through libp2p. And then in that case, yes, it does not gain any of the caching benefits because like that
stuff doesn't exist yet. Yeah. Great. Thanks. Yeah. Yeah. Excellent.


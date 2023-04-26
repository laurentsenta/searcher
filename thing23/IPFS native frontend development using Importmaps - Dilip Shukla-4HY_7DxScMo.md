
# IPFS native frontend development using Importmaps - Dilip Shukla

<https://youtube.com/watch?v=4HY_7DxScMo>

![image for IPFS native frontend development using Importmaps - Dilip Shukla](./IPFS native frontend development using Importmaps - Dilip Shukla-4HY_7DxScMo.jpg)

## Content

Hello everyone. And it's really wonderful feeling being here and also delightful for

the major reason being that I see a lot of people sharing the same ideas, headed towards
same direction, and that's quite interesting and give me a lot of motivation to move forward.
About me, I'm Dilip from India and based in Stockholm, core member of JSPM, JavaScript

Package Manager, and currently trying to found a company called Logon. On the internet, I

can be found as a user of Fusion Strings. And the agenda today is we are going to talk

about import map, what they are, and what are the challenges in web development in general,

but particularly for front-end development. What are the pressing issues? How JSPM has

already addressed those challenges and how plans to do them in future? And what is Logon,

what it is set up to do? And conclusion, basically, what next?

So before we begin, I think it is important to get in right frame of mindset. And that

brings me to this vague but interesting initial proposal which Tim Berners-Lee proposed initially

when he proposed initial draft of web2b. That also gave birth to HTML, kind of origin story

of HTML begins there. But on wide or bird-eye view, it also gives kind of a lightweight

platform for people to build and share information and abstract away a lot of complications from

setting up server or how do something. But not that is not just enough. It is mainly

information management concept. That means how we share information by linking one piece

of data to another piece. Having said that, let's jump in.
What are import maps? So basically these are import maps are new web standards and recently

became available in all major browsers out of the box support. And one way to imagine

about import maps is that these are native, web native package.json. Package.json is something

which we use in Node.js to manage dependencies to pull in from NPM or any other similar public

registries to fetch JavaScript dependencies to build front-end applications for the web.

And how import maps work is that we can imagine them as plain JSON objects. Key is any module

identifier, specifier, and value is any resolvable URL for those modules.

And then how do we use import maps? But before we proceed, we need to compare them how we
build for front-end or web in general today. So as we see, it is kind of continuous cycle
between fetching your source code, building on local, pushing, and again, rebuilding for preview. Then once again, rebuilding again for production. That means every time we make

a change, we have to rebuild. Every time we make that application available to a new environment,
we have to rebuild. And the root cause behind that is like because we need to use NPM and Node.js, which is primarily a backend or server-side solution. And there

is a lack of web native solution which can run in browser in general. So we plan to get

rid of both of them, hopefully someday. And about JSPM, what is the context here?

JSPM is JavaScript Package Manager, has been around for quite some time. Lesser known,

but still it has a niche user base. It has pivoted quite a lot of time, but in current

shape and form, it is basically an organization, a foundation, where we are set out to support

front-end development, to be precise, standard compliant front-end development by creating

toolings which are missing in the landscape of users for several reasons.
One being standards are also picking up a lot of speed. A lot of new standards are getting

into the specs, and there are no concrete implementations for them. So there is a hard

need of having some kind of tooling or ecosystem which can support web or browser native tooling

that JSPM does. And it does it by providing a CDN which can kind of fetch dependencies,

prebuilt dependencies from NPM and make it available in the browser using different kinds

of solutions. It has a JSPM library, HTTP API, and a CLI as well. Using that, we can

kind of generate dependencies for any front-end applications.
It also has the ability to parse HTML as an input and then generate another HTML which

has all the dependencies scanned and injected. This is one example, and using this QR code,

we can go to the live page where we can kind of play around with this work in progress sandbox generator. Here we can see we have some code, but there is no reference to from

where it can fetch these dependencies. And using JSPM generator, we can see that it has

generated automatically the import map and also added them, including integrity hash

into the HTML. And this is ready to be deployed immediately, instantly.

How it works. It works by using, say, JSPM CLI, one option to do it, by providing simple

and very memorable options, just using command line. And there are a lot of variations. Probably

heading towards docs is a good idea. And when we run this previous command, here we are

basically what we are doing is that pointing JSPM towards one kind of skeleton, app.html,

and then installing lit, which is a front-end development library. And we want the output

to comply with environment browser and production. There are several other options available,
like Geno and development environment, for example. And it will generate this kind of

HTML. Where we can see that even though we just mentioned lit, but it scanned all the

sub-dependencies, inline them using one short URL. That means no more heavy node modules

on your system. Then how it works currently. So, when any

publisher publishes their JavaScript packages to NPM public registry, then JSPM builder,

which is a code base, which runs on Google Cloud, it watches the NPM couch base and immediately

builds using rollup. Basically, it is the same process which every developer does with
every change, but it needs to be done only once, if we think about it. So, that is what
precisely JSPM does, and then uploads it to JSPM CDN. And through that, it is available.

Then what is the context of IPFS here? Because we seem to have solved the problem. Because

IPFS, even though kind of tries to solve a lot of problems for decentralized world, but

somehow when it comes to front-end development, specifically for IPFS, we fall back once again
that loop of Git, NPM, Git, NPM. So, even though IPFS is totally capable of doing everything

natively, it still has to go there. And that we can kind of solve. We can get rid of tooling,

which we no longer need. And how we do it, because, say, in general,

I mean, we say three problems, two problems. But naming and caching, cache invalidation,

to be precise, has been very hard problems for so long. As far as it has become kind

of meme. CID is set out to solve both of the problem, because CIDs can be treated as name,

concrete names, URLs, and if you have concrete names and URLs, cache invalidation is not
a thing anymore. And that actually circles back to HTML philosophy. That means like getting
rid of things which we can get rid of. And by this, using CIDs as a concrete identifier

to things, we can get rid of these problems. How JSPM and IPFS works is that JSPM is basically

capable of importing dependency directly from any URL, HTTP or IPFS, both. And once we have

basically downloaded the dependencies, we do not need to actually build it until or
unless we do not have a syntax error. So if any frontend project, in general, there is

a chance JSX is used or several other kind of build optimization need to be done.
So first of all, they are not needed. But even if they are needed, they need to be done only once and deploy only once because it is IPFS and everything is deduplicated. And

also JSPM provides pre-built dependencies. So this is basically the convergence between

IPFS and JSPM. And for resolving IPFS dependencies, we need to use a JSPM library. And this is

how it works. We import it. There are several other options to configure it, but this one
is used so that the dependencies are resolved relative to the HTML page where things are being imported. And then we pass any CID and it will basically generate the import map.

I did not. OK, let's do this. So this is a demo. I'm not sure how much it will be a reflection

of the things, but I will show the running demo as well. So basically what this one did

was ran two commands, one very simple dependency resolution on IPFS and another one was using

React as a dependency. So if you see here, this is the same page and it is running on

IPFS and it has this import map generated and we can see this is generated natively

from IPFS. That means that folder which has package.json, it is hosted on IPFS. There

is no NPM in between and it can scan all of the dependencies from there and embed it here.

This is another one, but it does not have any NPM dependency, just the local native IPFS dependency. Then this brings us to LogOm. What is LogOm and what is the connection with

JSPM and so forth? So the idea is that LogOm will pick up from where JSPM left and the

exact meaning of LogOm is not too much, not too less, just the right amount. And that
once again circles back to HTML philosophy that abstract away all the cruft which is not needed and only let people interact with what they actually need and that provides
value by some mean. It is imagined as a new kind of digital experience

platform, which is a mouthful, but we can kind of reduce it to pins and pages. I think

that would be quite familiar in the audience here. Imagine, say, we have a DAG and at the

root we have HTML pages and all the dependencies connected to that page, they are also part

of the DAG, but then we can follow all the changes throughout the lifetime of our website

and just store them as DAGs, but use import maps to resolve the dependencies which is

needed at that moment of time, but also kind of give all other infrastructure, including

hosting and servers without people needing to know or understand what is IPFS, what are

CIDs. So many unmaintained or deprecated libraries, people do not need to worry about them. So
basically abstracting away the instability in ecosystem and providing some kind of a

convergence between web 3 and web 2. People want sometimes some authority should take
care of all their infrastructure needs, but also reap the benefit of innovations happening
in decentralized space. So that is the idea basically. Like in early period of web, one

could simply open up a text editor, create a HTML file, JavaScript, everything is interpreted,

and it works. No longer juggling around CICD specifically for static web.

Then there is more. So if we look at this, this is a very familiar looking package.json,

but these keys, these are like special and this exports key is also special. This part
of native Node.js specification, this is how it resolves. That means whenever there is
a requirement with this module specifier and you resolve it based on which runtime it is

specified with. So going back to this here. So we could, instead of browser, we can use

all of those other keys, any of those keys for different, different vendors, because

lately there has been kind of a lot of innovation or a lot of new JavaScript runtimes have emerged

and also kind of gaining a lot of popularity in an audience, not in general audience, but
developers. So these are like well-defined keys, part of web interoperable community

group or working group, we can say. And these are like say official keys, but in custom

implementation we can resolve to another one. These native key resolutions are natively
supported already in JSPM, but in Logon, what we want to do is convert these resolved URLs

to CIDs. That means getting rid of once again, something which may have a probability to

break that is like say resolved URLs, because this one still expects these files to be co-located

with package.json. But in this case, this may no longer be required. Having said that,

all these QRs are different. So please scan them if you want to play them in real time.

Although Logon is still in, say not open. So please show your interest.

And this brings us to concepts of pins. So if we compare these two, here we have exports

and here we have imports. This is also official node.js package.json API. But what is different

here is that we can take this home prefixed by hash. We can take it as a concrete human

facing value. That means if there is any human software change management need to be done

across the system, across the application, this is the value. But then we no longer need
to take care of how it resolves or where it resolves. Take that and use it as a custom

component. So basically, we can have these CIDs on top of some custom components. And

from the infrastructure wise, we can resolve just by if a user uploads an HTML page using
this kind of syntax, then we can resolve these URLs and hydrate or include client-side JavaScript

automatically to HTML page. Obviously, this is optional and it can be done by hand as well, but it is good to have it inbuilt in some kind of service.
I think I ran quite fast. So that is it. Any questions?

So great stuff on the IPFS side. I just selfishly have a terrible time building JavaScript libraries.

I can't figure out how to get good at it. Is Legom going to help me? Can I show it my
JavaScript library and it will say fix it like this and then I'm more eligible to participate in these builds? I'm not sure I got that question. If I don't get my dist right, will you give me hints to improve it? On my npm package?

My npm package currently, the different CDN hosts for making a browser compatible, they

don't like me and so it doesn't work. And so what do I do to fix that? Great question. So when JSTM builds and uploads to CDN here, so during this build process,

it automatically makes it compatible to the profile it was used. So when we use browser
profile and if there is not already, so when someone publishes, they have to use some kind
of package JSON where they have to define this kind of resolution. So this resolution is not already specified in the package.json, just npm kind of automatically
does it for you. But there can be some problems, but there is a solution to that as well. So

we have a custom override. If some packages don't work, we let users define their custom
overrides and then that can be resolved in turn. Okay. Yeah, that makes sense. I still need help making my package like that.

Anytime. Yeah, I guess this still ends up being like step one, it still needs to be npm compatible, right? Like that part has to still work, right?

Can we skip npm? That's not my actual question, but yes.
Yes, yes. That's the whole idea. And this is a demonstration also showed. Maybe I can

kind of come to the source code here. So here, this package, this is totally native.

to IPS and I was able to install from this. So there is no npm involved at all, only the conventions.

That means there is need to be a directory with associated package.json. But with logon, I'm trying

to get rid of this as well, trying to use as much IPS as possible. So my actual, which is awesome, and I think partially answered your question. And that is
great. My actual question is, who do you see as the first set of users that you're targeting? Who do

you want to use this? Who do you want to come and use logon? Basically, anyone who's comfortable in writing HTML, but not so much JavaScript. And on top of it,

anyone who doesn't like building JavaScript. I don't think there's anyone that likes building JavaScript. So anyone, everyone, basically. Everyone. Basically getting rid of build step from the front end development. That's the core of the... So theoretically, you could build an app that's hosted on IPFS that has a file system provided by

WinFS, where you author something and then publish. And so all of that stuff could happen directly in

IPFS in a browser. Totally. And now you see why I was so much interested in WinFS. Amazing. Thank you. So what does this mean for something like Yarn or any type of package manager that you run? To die, kind of. Kind of. I just wanted to make sure I was on the same page.


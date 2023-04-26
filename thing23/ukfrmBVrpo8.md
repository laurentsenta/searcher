
# Pail - DAG based sharded key/value store using Merkle Clocks and CRDTs - Alan Shaw

<https://youtube.com/watch?v=ukfrmBVrpo8>

![image for Pail - DAG based sharded key/value store using Merkle Clocks and CRDTs - Alan Shaw](/thing23/ukfrmBVrpo8.jpg)

## Content

CIDV2 So this is actually three presentations, two of which I already gave that I've just kind of smooshed together and it's like a history of how this thing came into being. So let's get straight into it. Payal, like I said, well, like Chris said, is a DAG-based key-value store. It uses Merkle Clocks and CRDTs. So let's learn a little bit about it. First of all, let's be super clear about what the scope of this is.
Payal actually manages the part of the DAG that's responsible for tracking user keys and their values. And values in Payal are always just CIDs and CIDs obviously link off to other data which may be stored somewhere. Payal doesn't care. It's only managing the bits that make up the structure of the bucket.

Cool. And so, yeah, you can mutate the Payal. You can add stuff to it and delete stuff from it. You put things in. You give it a root, the root CID of the Payal. If you ever used MFS before, you'll know that when you add something to MFS, the root of the whole MFS tree changes every time you add something in. It's a similar sort of idea here. But you give it the root, the current root of the Payal. You give it a new key and a new value to put in and put that in the bucket. And delete, obviously, similar sort of idea. We give it the root and the key you want to delete. And so that generates a diff and the diff is just the blocks that were added
and removed as part of that mutation, along with the new root CID which you need for doing additional operations.

And then what you can do is you can kind of take those blocks in that diff and do stuff like upload it to web3.storage so that they are available over like bitswap via like Elastic IPFS.
You can do that if you want. You can put them in your own block store, whatever you want to do. But that's basically what happens. But yeah, anyway, Payal is a library. It's also a little CLI tool that I built just so I could like see it in action.
So let's try and do a demo of that now.

It's so close. Cool. Here we go. I'm just going to try and do that.
There we go. I'm going to leave that open actually. Okay. Here we go. Is that big enough for people? Giving away stuff. Cool. So on this side I've got my like a Payal which I can just list out the contents of. There's nothing in this Payal at the moment. But we're going to add some things. And so what we can do, I've got this. Doesn't matter what the values are for the purposes of this demo. So I've just got a little script that will generate me a random CID for everything I want to add to a Payal. So I'm just going to grab that. And what I can do is do like Payal put and then give it a key name. I'm going to call it one. And the value is just a CID. Like I said, keys are just arbitrary strings. Values are CIDs to existing data. Put that in. And Payal generates a little diff. So this is kind of what happens. This is the CID of the Payal root before this thing was put in. And so this block has been removed. This block has been added. And that's the new root of the Payal. Below here, these are the blocks, other blocks that were changed in the process of updating the Payal.
So in this case, because there's only one thing, we've only got the root node that got altered and changed.
So it gets a new CID. But when Payals get too big, they start to shard. And so you might, if you change a value in a big Payal with lots of things in it, then you might see more things appear here in this diff,
which is all the blocks that changed, which kind of propagate up to the root. So that's kind of how it works. Let's put some more things in. Payal put. Add another CID for me. Here we go. Cool. And then I can do list out. And we can see that we've got three items in there. They're lexicographically ordered. So that's why they're not in there in the order that I put them in. But it does mean that I can do kind of fun stuff like list out with a particular value.
So I can do list out. And then I can do list out. And then I can do kind of fun stuff like list out with a particular prefix to just list the things that begin with T, for instance. And it can do that. One of the big things about Payal is it can do that sort of thing really fast. So it allows you to construct your data in a way that you can list out things really quickly.

Cool. So then what is that? I talked about the DAG structure of the Payal. What does it actually look like? So I use this fun command called tree, which shows me that I have a shard here, which is just a block.

Every time you see shard, you think that's a block. And then it's got the keys and values in that block. So this is kind of not super interesting. But it gets more interesting when it does actually do some sharding. And I can actually force Payal to do some sharding to make this a bit more interesting by tuning down the maximum shard size.

What it does typically is that you keep adding stuff to it, and it puts things in the block. And when it gets to a certain size, the next thing you put in it, it will shard based on the key that you're putting in.

So let's just demo that. It makes it more obvious. So Payal put, and if I put like 3, 2, get another CID.

And then I can use this handy command line option.

Max shard size. And I'm going to set it to be 142, the current size of the shard. So basically when it tries to put something in, it will have to do some sharding to accommodate the new value that I'm putting in.

Cool. And then this now gets more interesting because we've got three CIDs listed here now.

Because we're going to have two shards. We've got the root shard as well as another shard that we've just created.
So we've got two blocks that have been created and one block removed.
And then so Payal LS will do what it normally does.
But if we do Payal tree now.
It's more interesting. So we've got the root shard. We've got a key called re, which actually doesn't exist in the Payal.

There's like pre, to and a typo there. But you can see that what it's done is it's kind of found this common prefix to these two keys and sharded based on that prefix.

And if you concatenate the keys in this shard with the parent, then you get the keys that are in the bucket.

Cool. So that's how it works. I've got this in here. I made a Payal.

And I can list out these words in a dictionary. 235,000 of them.

It's the words from if you have a computer and it's like Linux based or Unix based in some way.

You've probably got user share dict in there. And it's basically an English dictionary from the 90s or something.

And what I've done is I've created a Payal where the keys are just the words and the values are just random CIDs like I thought.

So that's that. And you can do you can kind of do fun stuff like Payal LS dash p, the prefix, and then list out all the words that begin with bat, of which there are 179 of them.

And it does that really, really quickly because it's designed to do that.
Anyway, so b, b, b, b, b, b, b, b, b, including battyphone, which is not a superhero's phone.

It's actually like a musical instrument or something. Anyway, who knew?

That's the words. And then also in that same directory, there's this thing called proper names, which is just names of people, human names, people names, names of people.

Just names. So there's 1,308. At some point in history, there was only 1,308 names.

Whatever. Anyway, so the cool thing about this is a bit less data in this Payal.

So when I do a tree, it's nice and quick, but you can see how a Payal looks when it's got a bit more data in it, how it's done.

It's done a whole bunch of sharding here. And you can sort of see how it shards up stuff.

So we got Sabrina as a name, and like Rudolph and Rupert.

So anyway, shards and stuff. And that's Payal.

Let's head on back to just prefix queries at the moment.

At the moment, it's like someone needs to write some code.

It's ordered, so you decide what happens.
Yeah, yeah, yeah, yeah, exactly.

So hang on, let's just turn off...

Stop mirroring. Okay, got it. There we go. Okay, back. We're back.
Cool, okay, so that's Payal. So at the time that I did this presentation and stuff, these are the questions I had.

Why do we even need a bucket abstraction? Well, CIDs, turns out, not super recognizable to humans.

You can't instantly see a CID unless it's the empty directory CID, which is easy.
But other CIDs, you can't necessarily know what data is behind them by just looking at them.
So that kind of sucks. And ironically, humans are the people who actually need to look at these CIDs and identify the data.

So it's weird that we don't have this kind of way of naming data according to what it is.
And annoyingly, all data storage services basically allow you to give names to data that you upload.

So why have we just got CIDs? So we want parity with this.

So in the context of creating a DAG and uploading it to something like Web3.storage,

why don't we just store a name with the upload? Well, actually, with Web3.storage right now, you can do that.

You can give something a name. But that name is actually just stored in our Postgres database next to the CID of the thing that you uploaded.

And that kind of sucks because that name is separate from that data. And it's also centralized in our Postgres database, which sucks.

And it's also not scalable because as soon as you want to list stuff out, we get a lot of uploads.

That table is a big table and we can't list out stuff like that, especially not with a prefix query like that.

So, yeah, we kind of want that name to be stored with the data as part of the data.

So that's one thing that would be cool. So the other thing you're probably thinking about right now is like, why don't we just use UNIXFS sharded directories for something like this?

Good question. It's kind of complicated. It does involve you bringing in like DAGPB, protobufs, protobuf encoding stuff, UNIXFS, understanding how UNIXFS works and encoding and decoding that data.

And also, it means that you're storing stuff as UNIXFS. You might have application data that's not necessarily files.

So anyway, the primary reason, though, is that I wanted this prefix matching ability to be able to really quickly list out things without actually reading the whole shard.

And with Hampt in UNIXFS sharded directories, you can't really do that without reading the whole thing.

So that kind of sucks. I'd also like to be able to use forward slash in keys because that just wouldn't be possible with UNIXFS directories.

And then I wanted to minimize block traversals and dynamically shard when it's absolutely necessary.

We're kind of optimizing for reading, even though writing is still pretty quick anyway.

So that's why that. My other question I had was like, man, every time I update the PAIL, the root CID changes.

How do I even keep track of that and share that? And at the time I was like, well, I guess we could have like an IPNS thing.

Like maybe there's some central service. Maybe we don't need to keep track of that root CID.

Maybe like the peers who are operating on the bucket just sort of day track it and like send it to other people when they need to know.

Anyway, need to know, want to know that. Like how are we going to do that?

And so, yeah, these are the questions I had at the time. But the biggest kind of question I had was like, how do we resolve conflicts?

Because it's nice that I can play with my bucket on my own. But what if I wanted multiplayer buckets where lots of people can be adding,

contributing to the same bucket, deleting things? How does it work when two people are updating the same key?

Or how does it work when like they're across the Internet and like the order of events is different?

Like someone deletes something and then someone adds it when actually the order should have been the other way around.

Yeah, so yeah, that was the end of the first presentation.
But luckily, when I was kind of looking into this, I found that there's like one way is to like just don't have conflicts.

It sounds pretty wild, but like there's a thing called CRDTs, as I found out, called conflict-free replicated data types, which can kind of help with that.

So next presentation is like a few weeks later. Let's talk about Merkle clocks and CRDTs.

One of the outcomes of that, those previous slides was like this realization that like in a distributed multiplayer environment,

like the order of operations really, really matters. And as well as the conflicts.

I'm going to talk a little bit about why that is and then show you how Merkle clocks and CRDTs can address the issues.

So let's consider a fruit basket. It's just a pail with three players.

We've got Alice, Bob and Carol. And what they're going to do is they're all going to add fruits at approximately the same time.

And we're going to look at this from Alice's point of view first. So Alice will put an apple in the fruit basket, then a banana.

Then she'll see that Bob puts a kiwi in and then she sees that Carol put a mango in.
Then she sees Bob put an orange in and then finally Alice puts a pear in the fruit basket.

So there's lots of fruit in there. That's great. But Bob, maybe due to network latencies, sees a slightly different order of events.

So Bob sees Alice put the apple in first, but then he puts his kiwi in, then he puts his orange in,

then he sees Carol put a mango in, then he sees Alice put a banana in, and then finally he sees Alice put a pear in.

So we actually ended up with the same operations, but they arrived in different orders.
And so you can imagine how that might be problematic if Alice or Bob or Carol were to put or delete items with conflicting keys.

And these are not conflicting, but you get the idea.
But why is this a problem? Why don't we just have timestamps and just timestamp these events and then sort them when we receive them according to the timestamp?

Well, not every player can be perfectly synchronized to a global time.

It's also really easy to spoof. You could set a far future time and then always get your thing applied to the bucket regardless.

And that sort of thing can be problematic in a trustless distributed system.
So we don't really want to do that. There's also the problem of in PAIL, aside from the problem of key conflicts, we actually can't receive events in a random order for this other reason,

which is the order of applied mutations to a PAIL determines the structure of the DAG that is generated.

So we can't really have events going in any order, even if they don't conflict, because we might end up with a different root CID,

even though the PAIL contains the same keys and values. So you could get, because it shards as it gets bigger, if you get a new key, it will use that key as the sharding key that it shards off.

If a different key comes in in a different order and it reaches that same threshold, then it will create a different shard and you will end up with two different roots CIDs.

And that's bad. So we don't want that. Anyway, cool.
So there's a thing called Merkle clocks and they make use of the inherent properties of Merkle DAGs to encode event ordering information.

So let's have a look at how that works. So this is the same same thing that happened earlier, but what we're going to do is create our own Merkle clock from these events.

And so what happens is that each player creates a graph of the events that happen and they can create events themselves or they can receive events from others.

And so, again, this is Alice's view of events again. All right. So she's just created an event which is a DAG node.

And you can see it's saying that she added an apple to the empty pair.
That's good. And what happens is that each player in the system keeps track of the latest event.

So it might be more than one if two or more players performed an operation at almost the same time.

So just you'll see in a sec. So Alice is tracking this as the latest event.
So next up, Alice puts in her banana into the pair and she tracks this as her latest event and the banana actually links back to the previous latest event, which is the apple.

So then soon after, she receives an event from Bob and Bob puts in the kiwi.

He hadn't received that banana event from Alice at the time he added the kiwi.
So this kiwi points back to the apple and that's okay because that's okay.

Alice just keeps track of these two events now. That's fine.
Then Alice receives an event saying that Carol put a mango in the bucket and Carol's mango does not point to Alice's banana.

So what we know about that is that when Carol created her mango event, she hadn't received Alice's banana event because it's not pointing back to it.

And now Alice's two latest events are the apple and the kiwi.

ąćAnd Alice receives another event from Bob. He's put in an orange. קümüć

�커�moil� GORD�

y banana a'r mango. Iawn. Ac yna, Alice ymgeisydd arall o Bob. Mae'n rhoi oran. Nid wyf yn credu, mae'n ddod yn ddiddorol o ddiddorau. Roedd yn digwydd cyn i Bob gwybod am y banana neu y mango. Pa sylwadau? Ond, iawn, nawr Alice mae'n rhoi cyfrifau ar gyfer y cyfan nesaf, y banana, y oran a'r mango. Ac yna, a'r ddechrau Alice addas y trwyddiant, ei trwyddiant trwyddiant, y pêr, ac mae'r pêr yn ysgol i'r cyfan nesaf y gwnaethon nhw ei trafod. Ac felly nawr, mae'n dechrau trafod ei gyfan newydd fel cyfan nesaf. Felly, gallwch weld y grâff y gwnaethon ni ei greu bydd yn y sefyllfa, dydyn ni ddim yn bwysig y rhan fwyaf y gynnal ni'r cyfan hwn, byddwn yn dod â'r grâff hon. Gallwch hefyd ddod â'r cyfan nesaf, fel y gallwn hillion na ddŵr i bob un

sy'n gynnol. Felly, be allwn ni ddweud Am mewn gwirionedd y cynlluniateirion hon. Unig fel heddiw y peir a'r mango, ond dydyn ni ddim yn gwybod a oedd o'n i, neu o'n i, neu o'n i'n y cyfraith,
ond roedd yn dod yn ddiweddar o'r mânt, ond cyn y peir.

Y peir a'r mango oedd o'n i ar ôl y kiwi, ond dydyn ni ddim yn gwybod pa o'r rhain oedd o'n i'n arfer. Ac yna, yn ddiweddar, rydyn ni'n gwybod bod y kiwi ar ôl y mânt a'r peir
oedd ar ôl yr oran a'r mango ar y rhan honno, ac hefyd ar ôl y banana. Iawn, felly, y gafon ni gynllun o ddiddordeb?

Iawn, dwi'n meddwl, iawn, dim yn wir, ond mae gennym rhai rhwng ddiddordeb, mae gennym rhai

gynllun o ddiddordeb, felly mae hynny'n fwy o ddiddordeb. Sut y gallwn ni gynllunio'r ddiddordeb y dyddordeb hyn yn amlwg? Wel, un peth y gallwn ei wneud yw gallwn gynllunio trwy gael
y nodau, gallwn ddod o'r dda i weithio i'r anhester cyffredinol

o'r holl ddiddordebau hyn, a'u cymryd yn eich gydol, gallwn gael 0, 1, 2 a 3

wrth i chi ffwrddio ar y trwy. Yna, rydych chi'n digwydd â rhywbeth sy'n edrych fel hyn, mae gennym Apple sy'n digwydd yn gyntaf, Cywi sy'n digwydd yn cyntaf, Banana, Orange a Mango sy'n digwydd yn gyntaf,
ond nid, ond yna, a yna peir. Felly mae gennym ni'r rhai sydd i'w ddod o'r gwaith i'w gwybod, felly mae hynny'n sgwrs. Beth ydyn ni'n ei wneud yma? Rydyn ni wedi gweld rhai pethau, ond nid pob un.

Un peth y gallwn ei wneud i'w gynllunio yw, fel, rydyn ni'n cymryd, fel, gan y CID o'r nodau

o'r pêl cyflawni ar ôl ymddangos ymddangos ymddygiad hwn, ac yna gynnal ni gymryd

ymddangos sy'n byw yn yr un peth ar gyfer pob chwaraewr sy'n cael y cyfnodau hyn. Felly gallwn wneud hynny.

Iawn. Iawn, felly ar y cyfnod rydw i wedi ei ysgrifennu, roedd fy nghaith nesaf yn rhywbeth fel hyn.
Roedd y syniad o ddefnyddio'r clociau Merkle a'r CRDTs i gynnal pêl cyflawni ar ôl ymddangos.
Fe wnaethon ni greu cloc Merkle fel gwasanaeth, mech-a-as, fel y dywedais yn ddiddorol.

Ac hefyd, roedd gennym gysylltiadau i ddysgu am ymddangos, oherwydd dydyn ni ddim eisiau pêl cyflawni ar ôl ymddangos,

rydyn ni eisiau gallu cyflawni cymryd ymddangos i bobl i wneud ymddangos ar gyfer ysgrifennu, neu ysgrifennu'n iawn, neu'n iawn, etc.

Ac rydyn ni hefyd yn adeiladu, yn gyfartal, rydyn ni'n adeiladu'r API Web3.storage ar gyfer UCAN,

sy'n mynd i ddod allan, neu sydd wedi dod allan yn beta, ond rydyn ni'n dod â'r cyflawni'n ddiweddar.
Felly mae'r holl hyn yn cyfathrebu'n iawn gyda hynny. Ac yna, os oes gennym ymddangos Merkle fel gwasanaeth, gallwch, yn ystod y ffordd, adeiladu unrhyw ddysgu CRDT ar ôl ei gilydd.

Iawn. Felly, yn ffwrdd gyfartal, rydym yn dod â'r diwrnod ar gyfer y bryd. Rydyn ni wedi'i adeiladu'r peth y dwi'n ei ddweud yn w3-bucket. Mae'n ddiddorol iawn, ond mae'n cael ei adeiladu ar y MacOS, y gwasanaeth Merkle, y llyfrgell pêl,

sydd yn yr hyn rydyn ni wedi gweithio gyda nhw, a'r newid ymddangos Web3.storage ar gyfer UCAN, sy'n ein ddweud yn w3-up.

Y bryd w3-bucket yw'r dylunio i'r w3-cli.

W3-cli yw'r dylunio i w3-up o'ch cymorth. Felly dyma sut mae'n gweithio. Yn gyntaf, fel y gwelwch chi'n ei weld yn y pêl, mae rhoi newid gwerthoedd newydd yn eich pêl leol.

Mae'r newid yn cynnig ddiff, sy'n set o blociau a'r rwydwyr a'r rwydwyr newydd.

Mae'r blociau a'r rwydwyr newydd yn cael eu gofalu gan w3-up,

fel y gwelwch chi'n ei weld, yw'r newid ymddangos Web3.storage ar gyfer ddatganu data yn defnyddio UCAN.
Mae w3-up yn gwneud y swydd o gael y blociau hwn yn ymgyrch i'r IPFS elastig a'r Filecoin.

Roedd ymgyrch ar y camp IPFS y gafodd i mi. Gallwch chi edrych ar y swydd, oherwydd roedd yn dda, yn fy nghyfri. Yn ymgyrch i'r IPFS elastig, gall unrhyw un ddod o'r blociau sy'n cynnig y bucet ar gyfer bitswap neu'r gateways os ydynt eisiau.

Yn ymgyrch i'r W3-up, bydd y gwneud y peth yn mynd i'r bucet yn gwneud cyfnod o ddarlith Merkle Clock,

fel y gwelwch ni gyda Alice, Bob a Carol. Felly, rydyn ni wedi creu'r ddarlith Merkle Clock, ac gallwn ei ddangos i'r gwasanaeth Merkle Clock,
sy'n dda. Rydyn ni'n cynyddu'r ddarlith Merkle Clock. Yn ymgyrch i'r W3-up, gall y ddarlith Merkle Clock, pan fydd yn cael y gwybodaeth honno,

gael ei ddod o'r blociau sydd angen ar gyfer bitswap. Felly, os ydym ni wedi'i ddod o'r ffordd, gall unrhyw ddarlith Merkle Clock yn dod i'r ffocws

a ddefnyddio'r ddarlith Merkle Clock fel ysgolion ar gyfer ysgolion, ac gall ei ddysgu'r cyfarfod cyffredinol o'r ddarlith. Felly mae'r cyfarfod yn y set o ddarlithau sy'r cyfarfodau cyfanol yn y ddarlith Merkle.

Felly gallwch ddysgu'r ddarlithau hynny. Ac yna gall un arall ddod i'r ffocws a wneud yr un peth. Ond y bwysigrwydd yw bod y ddarlithau hynny hefyd yn ddarlith Merkle.

Ac pan fyddwch chi'n rhoi gwerth, mae'r holl ddarlith W3 yn cynyddu'r ddarlithau lleol ei hun.

Felly gallwch chi ddysgu'r cyfarfodau cyfanol eich hun yn ystod arall.
Ac mae hynny'n iawn oherwydd Merkle Clocks a CRDTs, mae'n gweithio iawn, oherwydd, yn rhan o beth, mae'r cyfarfodydd yn dod i mewn.
Felly gallwch ddysgu'n ystod arall i'r bwysau eraill. Ac yna dydynt ddim angen y pwynt rondebwyd yn unig, yn ysbytai eich bod chi'n gallu sôn yn ystod arall i'r bwysau eraill. Mae'n dda iawn, ond dwi'n meddwl. Yn ystod y byd, wrth fy mod i'n adeiladu hyn, roeddwn i'n meddwl am y gweithgareddau hyn.
Sut allaf i wneud hyn fel CLI ddefnyddiol sy'n deimlo i bobl? Roeddwn i'n meddwl amdano mewn cyfnod o weithgareddau git. Mae'r cyrff git yn ymwneud â chlock advance, ac mae'r cyrff git pull yn ymwneud â chlock head, cael y cyfan cyfnodol, y cyfan sydd ar y tip o'r chlock. Pan rydych chi'n pull, rydych chi'n cael y cyfan a'i wneud yn ymwneud â'r cyfan rydych chi wedi'i gael.

Ac yna'r cyfanau eraill yw'r cyfan remote y gallwch chi ei gyrru.
A efallai y cyfan o'r cyfan yw'r cyfan Merkle yn y gwasanaeth. Y peth y gallwch chi ei gyrru ar gyfer y defnyddiol fel github.com. Yn fawr iawn. Gadewch i ni wneud y demo hwn yn gyflym. Yn fawr iawn iawn. Gadewch i ni wneud y demo hwn yn gyflym. Gadewch i ni wneud y demo hwn yn gyflym. Yn fawr iawn iawn. Yn fawr iawn iawn. Beth yw'r peth? Mae gen i ddau cyfnodau CLI.

Gallwn wneud, W3CLI, gwnes i gael y cyfnod o'r CLI.

Gallwch weld bod y cyfnodau gwahanol y gydag ni yma.

Rwyf wedi gwneud y gwaith o ddewis ymwneud â'r un ar y ddra.

Alice ar y ddra a Bob ar y ddra. Rwyf wedi gwneud y gwaith o ddewis Bob, gallu cynyddu'r cloc ar un ardal, a chyfnodau i gysylltu'r pethau i un ardal neu byd. Rwyf wedi cael y cydnabod o hynny. Gallwn gysylltu'r ffaith bod gen i gysylltu'r cyfnod i'r adnodd. Mae'r byd y gwnes i'w ddweud amdano. Mae Bob yn gallu gwneud hynny ar y cloc Alice.

Mae'n dda iawn. Mae gen i gysylltiad o cloc,

ac rwyf hefyd wedi sefydlu Bob fel gysylltiad cyfnodol.

Felly dyma'r gwneud y gynhyrch hon ddim yn cyd-dod yn ôl. Bob yw'r ffyrdd,

a'r gynllun Bob. Gallwch wneud cyfnod o fucat W3 a bydd yn dechrau cyflwyniad i'w ddod i mewn. Mae'r gynllunau'n cael eu sefydlu fel gysylltiadau. Ar y gynllun hon, mae Alice wedi'i sefydlu fel gysylltiadau.

Mae'r gynllunau'n cael eu sefydlu fel gysylltiadau. Mae'r gynllun hon yn un o'r CLI y gwnaethof i chi,
felly gallwn ni llwyno gynllunau yn y bucat. Mae gen i rai gweithiau i'w dystio. Gadewch i ni allu hynny. Gallwch weld... Mae ganddo 13 pethau. A dwi'n cael 12. Yn y bucat, a yw'r un pethau'n un peth?

Wel, byddai'n gwneud y peth o orfod cloc.

Nid yn y bwysigrwydd, ond yn y bwysigrwydd,

os yw'r pethau'n un peth, byddai'r pethau'n un peth.

Os ydyn nhw'n cyflwyno'r un cyfnod a'r gwerth, byddwch chi'n cael yr un cyfnod. Os ydyn nhw'n cael yr un cyfnod a gwerth, byddai'r pethau'n un peth. Yn y bwysigrwydd, mae'r cyfnod a'r gwerth yn cael ei chreu.
Mae'n dweud bod yn ddepennod ar y pwynt hwnnw fel gweithgaredd diwethaf.

Byddwn yn gwneud cwestiynau ar ôl.
Rydw i'n mynd i wneud cyfnod, a dwi'n mynd i ddod yn un arall.

Mae'r cyfnod hwnnw wedi mynd yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

Mae'r cyfnod hwn yn y bucket Alice.

[...]

It's like a block cache for the events that you've received.
Okay, so the events are, this is my current route, and this is the routes that are parents of this event, essentially. Yeah, yeah. Yeah, yeah. Because we're pushing events into Elastic IPFS, we don't have to keep hold of them locally. We can, you can just get them as and when they're needed from the IPFS network, because Elastic is always on. So you don't, like, you can literally delete your local block store and still have access to the bucket, because you've stored those blocks in web-free storage. And those are events that are signed by the DITKey. The, so the UCAN that says advance the clock
with this event is signed with my DITKey, yeah. Yeah. The events themselves are not, but they have CRDs. This is good enough. Sorry, I missed the reasoning for needing the shard mechanic, like, in general. Like, if you, like, just take a literal reading of, like, the Merkle CRD paper, like, where does the shard fit in into that model? The sharding of the bucket is, because it's one block, at some point, you can fill that block up and make that block too big for libp to be a transfer. So when you add a new item, it adds to the same block? Yeah. It replicates everything from the old object into a new block? Yeah. So it's like N from the old block, N plus one in a new block? Yeah, yeah, exactly. Yeah, you'd have one block with, like, one key in it. If you put another key, you'll get a new block with those two keys in it. And if that block gets too big, you put too many things in it, then it will create another block for one of the keys that is a shard, which reduces the size a little bit to accommodate other things. Is that a causal duplication, like, between blocks then? Or do you just, like, stop caring about the old ones? They're kind of small. The shard size doesn't have to be big, so, like, there's some duplication, yeah. Okay, cool. Yeah, but the key here is that, like, I don't really want to de-dupe everything because I want to minimize traversals, because fetching, like, blocks for each item is expensive,
and if I can just get one block with everything in it, then that's faster than getting a block, asking for a block, getting a block, decoding it, getting the next block, decoding it. Like, if I do that for every, like, if it's a deep shard, then it's not so good. All right, thanks, everyone. Thank you. Thank you. Thank you. Thank you. Thank you. Thank you.


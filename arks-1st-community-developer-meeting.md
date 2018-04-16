# Ark’s 1st community developer meeting
Here is a short and raw log of the community developer discussion with lead developer @fix of Ark. Enjoy!

Points discussed in the Meeting:
- Network forks. Explanation of types and causes.
- Log analysis via new log collector by @faustbrian
- AIP 11 & AIP 12
- Delegate resignation
- Multiple output types per transaction
- Fee reduction

Join our Developer channel: <https://gitter.im/ark-developers/Lobby>

**François-Xavier Thoorens @fix** 10:17

@/all i am thinking to set up a dev meeting here every monday at 2pm CET / 8am EST / 8pm China Time

purpose:

technical question requests (instead of answering all PM)

kick off the week development (target, needs for dev etc…)

**François-Xavier Thoorens @fix** 13:54

@/all starting aip discussion in 5min

**François-Xavier Thoorens @fix** 14:07

so agenda for this meeting:

**François-Xavier Thoorens @fix** 14:08

aip11 discussion

**@dafty-1** aip11

fork message meaning

Everybody ok?

**Alex Barnsley @alexbarnsley** 14:08

I was interested in IPFS but realise it’s reliant on this AIP so it can wait

**François-Xavier Thoorens @fix** 14:08

ok let’s start backward then. FORK messages

the easiest one :)

 **@jarunik** 14:09

Are there 4 and 5 error descriptions or do they not exist?

**François-Xavier Thoorens @fix** 14:09

so bottom line fork messages are a way to log events that are network related, when a node notice something that prevent him to be in sync in a broadway

so far there are only fork 1 to 6

let’s detail all numbers:

**François-Xavier Thoorens @fix** 14:12

fork 0: a tx already in blockchain is inserted in a new block (happening almost never)

fork 1: new block is referencing an unknown previous block (more precisely block.previousBlock id is NOT at block.height-1)

 **@jarunik** 14:16

Is that the orphaned blocks?

**François-Xavier Thoorens @fix** 14:16

fork 3: block is not forged by the delegate that it was supposed to be

**François-Xavier Thoorens @fix** 14:16

orphaned blocks are a different thing, getting to that after

**François-Xavier Thoorens @fix** 14:19

fork 6: the one happening more often:

before forging a block nodes is requesting network if “It” is okay for him to forge. It is doing this by polling randomly “good” peers, and they answer depends on

timesync

delegate public key

last block id of its blockchain

they if under 66% of polled peers answer “no” -> fork 6.

the node is trying to quickly reconfigure (like getting last block he might have missed) and retry again to poll the network

also there is a window of 4 seconds (blocktime/2) to forge the block, otherwise the nodes answer no, and the delegate cannot forge anymore

reason is that if the delegate forge too lately the block, the time the block propagate, it might conflict with the next block and get orphaned (or make the next block orphaned)

**François-Xavier Thoorens @fix** 14:24

bottom line if delegate node is not “fast” enough to react to get in sync with network, it forks 6 and is prevented from forging

so the best way to decrease fork 6 is to have more powerful and well connected node

as far for other forks (2, 4, 5) they are basically same as type 3, but slightly different so i removed them in favor of orphaned block concept

(sorry same as fork 1)

ie block.previousBlock is same height, block.height+1 is same block id etc…

any questions on this?

**Alex Barnsley @alexbarnsley** 14:27

makes sense to me

i guess the question is, can anything be done to help fork 6, and reduce how often it happens?

**pieface @samharperpittam_twitter** 14:28

As fork6 seems to be by far the most frequent. I’m guessing the minimum specs should be increased? Or at least get the specs of the machines frequently forking and see if there’s any correlation?

**François-Xavier Thoorens @fix** 14:28

more powerful and well connected node

**Alex Barnsley @alexbarnsley** 14:29

can this be something that is “forced” before being able to start up a node?

 **@jarunik** 14:29

How can we remove slow old dead pers?

**François-Xavier Thoorens @fix** 14:29

I am not running a mainnet delegate so it is difficult to estimate, but since fork 6 cases are increasing, it means specs should go up.

**Alex Barnsley @alexbarnsley** 14:30

in theory i could get Ark running on a Pi. it would be terrible, but if that ever tried for forge it’d potentially cause fork 6’s. This is why I asked about forcing a certain spec

to stop people using poor spec nodes

**François-Xavier Thoorens @fix** 14:30

**@jarunik** we can’t anybody can join. But point is that ONLY FORGING DELEGATES get fork 6

there is already something to prevent from forging on pi

 **@jarunik** 14:31

Voting out bad delegates does not work as u cant see specs

**Alex Barnsley @alexbarnsley** 14:31

could specs be determined by the time it took to do a certain task?

for example, mining is better on high end machines, therefore it’s good spec

**Mike Garuccio @mgaruccio** 14:32

What about a way to notify users when the delegate they are voting for misses blocks or causes a fork 6?

**François-Xavier Thoorens @fix** 14:33

here is a “basic” filter to prevent forging from pi <https://github.com/ArkEcosystem/ark-node/blob/c1df3762a4301deab54f7ab0f7935a7975497cd5/modules/nodeManager.js#L100>

**Alex Barnsley @alexbarnsley** 14:33

hm. perhaps if we could extend the filter?

 **@jarunik** 14:33

But delegate gets fork 6 if bad peers do prevent consensus… or not?

**François-Xavier Thoorens @fix** 14:34

@mgaruccio there is already a productivity info about efficiency of delegate

**@jarunik** not exactly, the node is selecting “good responsive peers” and have a timeout in the request

 **@jarunik** 14:35

Better specs will not help to get better responses from peers

**François-Xavier Thoorens @fix** 14:36

**@jarunik** here <https://github.com/ArkEcosystem/ark-node/blob/c1df3762a4301deab54f7ab0f7935a7975497cd5/modules/nodeManager.js#L100>

 **@jarunik** 14:36

How will better spec prevent fork 6?

**François-Xavier Thoorens @fix** 14:36

<https://github.com/ArkEcosystem/ark-node/blob/c1df3762a4301deab54f7ab0f7935a7975497cd5/modules/loader.js#L668>

**@dafty-1** 14:37

@mgaruccio monitoring scripts exist already. I do not think ark-node should be concerned about monitoring too imo

**François-Xavier Thoorens @fix** 14:37

**@jarunik** better spec will increase connectivity to network (blocks will propagate faster)

**Alex Barnsley @alexbarnsley** 14:38

would there need to be a way of checking specs of machine and the network they’re connected to as well?

boldninja @boldninja 14:38

Fix some people switched to dedicated and they still seem to “fork” so might be timedrift as well or latency (bad connection) ?

**François-Xavier Thoorens @fix** 14:38

also with time the number of transaction are increasing, making the nodes consuming more energy and thus better specs are needed. On this very aspect aip11 will decrease tx througput

yes good nodes to prevent from fork 6:

fast

well connected (good bandwith)

clock sync

 **@jarunik** 14:39

Sorry i dont see that

My node is never at limits

**François-Xavier Thoorens @fix** 14:39

only one parameter being bad and the node start to fork6

 **@jarunik** 14:40

We can have a look but most seem to have decent specs with a lot of unused performance

**François-Xavier Thoorens @fix** 14:41

one good thing to see how well connected is to look at nodeip:port/api/peers and check delay

if delay are over 200ms, something is getting delayed (bad firewall settings, hardware antivirus, proxy whatever)

 **@jarunik** 14:42

Arkstats.net shows it and still the good delegates have problems

**Alex Barnsley @alexbarnsley** 14:42

do we know where most the nodes are hosted at?

**François-Xavier Thoorens @fix** 14:43

for instance here <https://node1.arknet.cloud/api/peers>

**Brian Faust @faustbrian** 14:43

I tested like 10 logs total now and 6 seems to be the issue in a good 80% of cases

**François-Xavier Thoorens @fix** 14:43

no but for instance our nodes are hosted at OVH

**@dafty-1** 14:44

@fix ark node often fails to recover after a fork 6 and requires db rebuild. it tries to sync with forked nodes. could this be related to peer banning?

**François-Xavier Thoorens @fix** 14:44

also fork 6 could happen if over 33% of nodes are in bad sync

so the delegate wait to get in in better shape before starting again (that was the issue when network halted for a few minutes in the past)

 **@jarunik** 14:45

Looks like that 33% happens during some bad times

**François-Xavier Thoorens @fix** 14:45

so delegates could add a few spare relay nodes on the network to have it in better shape

and decrease this issue as well

 **@jarunik** 14:46

60% is bad peers

Chris wrote peer filter

**François-Xavier Thoorens @fix** 14:46

you need at 66% of polled peers

 **@jarunik** 14:47

Usually only 30% left which are good

we had exactly that problem with arkgo before we filtered much more

**François-Xavier Thoorens @fix** 14:48

well now there is an issue with filtering out “bad peers”, because then you don’t have real picture of a network, and it is definitely a vector of attack: an attacker would selectively take down some peers to try to make the consensus at his own advantage

 **@jarunik** 14:48

Yes but now we have kind of a bad peer attack

**François-Xavier Thoorens @fix** 14:49

imho this is not “an attack”

this is real life of synced DPoS consensus

that is why i was thinking to have blocktime increase adaptative algo (similar to difficulty) to account for this

 **@jarunik** 14:51

could we do a mandatory update… so at least unupdated old bad peers drop out ?

**François-Xavier Thoorens @fix** 14:52

because there are many reasons why connectivity of peers can worsen (an attack is a one of the reason but not the main one)

**Alex Barnsley @alexbarnsley** 14:52

I thought about suggesting increased block times but didn’t think that’d be a good option. Adaptive algorithm sounds like a much better way to go!

**François-Xavier Thoorens @fix** 14:52

**@jarunik** well i want to understand why you absolutely want to have 0 missed blocks

 **@jarunik** 14:52

I don’t … but delegates don’t recover from it

**François-Xavier Thoorens @fix** 14:52

this is a natural thing, like on PoW sometimes there is no block for 1 hour

 **@jarunik** 14:53

So you think there is no problem on the current network?

**François-Xavier Thoorens @fix** 14:53

**@jarunik** that is another issue, usually they should recover, otherwise it means the node has some issues

because on fork 6 there is no rebuilding, there is just trying to get las blocks

 **@jarunik** 14:54

yes … but currently it looks like they don’t recover

**Alex Barnsley @alexbarnsley** 14:54

Is there a reason for not rebuilding?

boldninja @boldninja 14:54

Not the case lately — they don’t recover on their own (like in the past)

 **@jarunik** 14:54

if they would recover … i would not see it as an issue

**François-Xavier Thoorens @fix** 14:54

mm then this is something else happening.

i understand better now

 **@jarunik** 14:55

and i think it creates kind of chain reaction

**François-Xavier Thoorens @fix** 14:55

there is currently no trial to rebuild when fork 6.

 **@jarunik** 14:55

as they weaken the network on top (if the don’t recover)

boldninja @boldninja 14:55

probably has to do something with peers imo when they try to sync to last block

**Brian Faust @faustbrian** 14:55

would there be something that speaks against an automatic rebuild on fork 6?

**François-Xavier Thoorens @fix** 14:55

because the event is due to network, not blockchain, so then i have to inquiry deeper about it

**Brian Faust @faustbrian** 14:56

at the moment people just try to get to their nodes asap to rebuild manually every single time so making that process automatic would solve that at least

**François-Xavier Thoorens @fix** 14:57

mm could be one of these <https://github.com/ArkEcosystem/ark-node/blob/c1df3762a4301deab54f7ab0f7935a7975497cd5/modules/delegates.js#L300>

 **@jarunik** 14:58

@faustbrian rebuild should not be the norm … you have to trust the snapshots

could turn into an attack vector

**François-Xavier Thoorens @fix** 14:58

yes this is a difficult part

**Brian Faust @faustbrian** 14:59

true but at the moment it is the norm as you just run to your node and rebuild

hope the logs this week will have some info

**Alex Barnsley @alexbarnsley** 14:59

How could over height block happen @fix?

**François-Xavier Thoorens @fix** 14:59

attack, or bad connectivity

that is documented in the code just below

**Alex Barnsley @alexbarnsley** 15:01

Oh sorry yes, I missed that. Presumed it was for further down

**François-Xavier Thoorens @fix** 15:01

yes

ok maybe we should move on the next item of the agenda

aip11 + dafty proposal

**Alex Barnsley @alexbarnsley** 15:03

:+1: I’ll just watch/read now as I’m back to working

**François-Xavier Thoorens @fix** 15:03

for the record: <https://github.com/ArkEcosystem/AIPs/blob/master/AIPS/aip-11.md> and <https://github.com/dafty-1/AIPs/blob/master/AIPS/aip-11.md>

**Brian Faust @faustbrian** 15:07

Could daftys AIP cause some issues for the network because the drop out would be so sudden for a delegate?

**François-Xavier Thoorens @fix** 15:07

ok so aip11: objective is to make transaction more lean to be exchanged, and improve transactions rules. I won’t go into details, but the transactions are now:

serialised and deserialised in an efficient way (was not in the past)

as small as possible

serialised version will be used as the default way to communicate between nodes (deprecating json)

@faustbrian no, the idea is that nodes are in consensus so the forging delegate list generated every 51 blocks are in sync

the implementation would be quite straight forward as well

for me this is a good proposal, so if everybody is ok we will:

move dafty aip11 to aip12

include this resignation tx on aip11

**Brian Faust @faustbrian** 15:11

would there be any negative side effects/downsides for the delegate wallet or does it just turn back into a “private” wallet

**François-Xavier Thoorens @fix** 15:11

yes will be normal wallet

**Alex Barnsley @alexbarnsley** 15:11

I like this. Could existing votes for the resigned delegate be made inactive, removing the need to unvote first?

**François-Xavier Thoorens @fix** 15:11

funny thing is that it could be a way for people to name their own wallet

**Brian Faust @faustbrian** 15:12

if people want to spend their ark like that :D

**Alex Barnsley @alexbarnsley** 15:12

that’s my only comment — back to work!

**François-Xavier Thoorens @fix** 15:13

@alexbarnsley no this is not a good idea, because:

can be computing expensive (vector of attack where people make fake votes to a delegate and then deactivate it)

boldninja @boldninja 15:13

Alex c c c

**Alex Barnsley @alexbarnsley** 15:13

@fix that’s fair enough. thought i’d ask the question :D

**François-Xavier Thoorens @fix** 15:13

would make a case for community reaction or notification for action, which could be good

**Brian Faust @faustbrian** 15:14

hmm, stupid and might never happen but what if a whale (2m ARK or so) wants to name his wallet by going delegate and then resigning but somehow voted for himself so he joins the top 51 for few minutes. Would the really fast join and dropout be an issue for the network?

boldninja @boldninja 15:15

Shouldn’t we’ve seen more drastic changes in span of one round than this.

**François-Xavier Thoorens @fix** 15:16

@faustbrian no

as bad as a delegate dropping naturally from 51 active

**Brian Faust @faustbrian** 15:17

yeah, was just wondering if the network could be somehow damaged/stressed by people quickly joining and leaving the top 51

**François-Xavier Thoorens @fix** 15:18

so for aip11 i have almost a complete implementation done on ark-js. I will push it this week

this will also raise the question of API versioning for ark-node but this will be for another aip

**Brian Faust @faustbrian** 15:19

API versioning should probably be done via headers to not start nasty stuff like module loading by url parameters

**@dafty-1** 15:19

@faustbrian this could be done now. delegation is an expensive tx which makes it infeasible to be an efficient attack

**François-Xavier Thoorens @fix** 15:19

@faustbrian no stress, basically the only thing is an extra indexed database column, so the delegate generation overhead will be at O(1)

@faustbrian yes that is why i included the version of protocol into transaction. I will do similar thing for block headers

also to will make easy to export blockchain in some data format to rebuild even faster if needed

or perform easy spv with no database

**Brian Faust @faustbrian** 15:24

when the resignation is completed

is there any way to let the voters know

maybe you have a shady delegate with weekly payouts and he disappears and people won’t notice for a full week

**@dafty-1** 15:26

you could filter on resignation tx. but really it is up to voters to notice. it’s the same scenario when a delegate gets voted out and people don’t realise for weeks

**Brian Faust @faustbrian** 15:26

true but resignation is more permanent and the choice of the delegate

you getting voted out is not a choice you made

but yeah, should be the voter that keeps an eye on it

**@dafty-1** 15:28

you could add a tag on the explorer and ark-desktop if you were inclined. resigned delegates can not accept new votes, so their vote count will reduce over time

**Brian Faust @faustbrian** 15:31

I think that there at least should be some kind of marking in the explorer

since that and the desktop client are where voters check

the forum would be somewhat useless if the delegate just disappears

without notice

**François-Xavier Thoorens @fix** 15:32

@faustbrian yes at “client level” ie some tools listening for this kind of tx and then sending message

@faustbrian also the api will answer that the delegate as resigned when queried, so the client could display it

and propose to unvote

**Brian Faust @faustbrian** 15:34

sounds good to me, like the idea of delegate resignation as long as the voters in one way or another will get informed about it and are not 100% left to their own devices to find out

would probably be useful to check the currently voted delegate when you start ark-desktop

and if the delegate is marked as resigned, show a big modal window that states that the delegate is no longer active

**François-Xavier Thoorens @fix** 15:35

yes something like ta

that

ok i am not sure you have more questions, but i have some

**Brian Faust @faustbrian** 15:37

my questions have been answered, was mostly concerned about the voters getting to know about the resignation

**François-Xavier Thoorens @fix** 15:37

i was thinking about multi payment tx: one tx could hold more outputs. the idea is to make it lighter and possibly include this into an automated payment tool at the node level

what do you think?

like type 0bis -> several (address-amount) pairs as payload

**@dafty-1** 15:38

would fee increase per output, or the same if you have 1 or 50 outputs?

**Brian Faust @faustbrian** 15:38

would it pretty much just be a collection of transactions?

**François-Xavier Thoorens @fix** 15:38

would decrease the fees for multi

boldninja @boldninja 15:39

That is an attack vector if you decrease no?

**François-Xavier Thoorens @fix** 15:39

because the ratio of tx/payment will decrease

well again up to delegates to find out the right ratio

boldninja @boldninja 15:39

those blocks will be “heavier” (bigger size) so someone could spam those multi tx fairly cheaply

**François-Xavier Thoorens @fix** 15:41

well the fees would increase with size of tx

just for instance it will make payment fees a lot lighter (vendorfield will be squashed to one tx for instance)

boldninja @boldninja 15:42

its genius idea (for pool payments) I’m just not sure about decreasing fee for this tx

fees will be fairly cheap with newest update and after adaptive maybe even reduced further

**@dafty-1** 15:47

could anything cause an output to be invalid? if yes, the transaction (and all outputs) would fail too right?

**François-Xavier Thoorens @fix** 15:48

yes

**Alex Barnsley @alexbarnsley** 15:48

I agree with @boldninja — lower fees are great for users but could result in an attack in this way, if the fees are further reduced when consolidating into single transaction. also, would there be a limit on transactions per multi-transaction?

**François-Xavier Thoorens @fix** 15:48

just as normal type 0 tx: if balance is not enough, the whole tx will be rejected

**@dafty-1** 15:49

got it

**François-Xavier Thoorens @fix** 15:50

yes was thinking like 32 or 64 tx (x29 bytes)

ok i propose to end this 1st dev meetup, maybe someone could make a blog with main results or transcript?

 **@jarunik** 15:51

Did we already discuss the fee “minimum” ?

**François-Xavier Thoorens @fix** 15:51

not yet

 **@jarunik** 15:52

I would change the formala in some way

**François-Xavier Thoorens @fix** 15:52

but the aip11 propose it is setup at delegate level

 **@jarunik** 15:52

that it can never be 0

**François-Xavier Thoorens @fix** 15:52

well the rule will be anyway a tx can’t be accepted is fee is null

 **@jarunik** 15:52

it should at least be 0.00000001 x “transaction load”

**François-Xavier Thoorens @fix** 15:52

0

 **@jarunik** 15:53

ah ok

then it is fine

**Brian Faust @faustbrian** 15:53

what could happen if a fee is stupidly low? like 0.00000000000000001

 **@jarunik** 15:53

you can’t have lower than 0.00000001

boldninja @boldninja 15:53

only 8 decimals tho

 **@jarunik** 15:54

I can make a short transcription on a medium blog … if no one else wants to.

**François-Xavier Thoorens @fix** 15:55

moving foward this week:

release of final aip11 with mentioned integration (dafty proposal plus multipayment)

release of reference implementation on ark-js

next monday meetup agenda.

Ok?

**Brian Faust @faustbrian** 15:55

I am finishing up the log thingy so that I can deploy it tomorrow

 **@jarunik** 15:56

thanks a lot @fix … i think the meeting is a great idea

**François-Xavier Thoorens @fix** 15:56

nice @faustbrian maybe we can also gather all what is going on and make some milestone snapshot about all projects going on

**@dafty-1** 15:56

ok, thanks @fix

I will touch up 11, rename to 12 and create pr

**François-Xavier Thoorens @fix** 15:56

yes thanks **@dafty-1**

also for next meeting i will put some aip(13) for fork management at level of ark-code, something i have been working on the last weeks

ok anyway thanks everybody for being there

and have a good coding week

---

Join our Developer channel: <https://gitter.im/ark-developers/Lobby>
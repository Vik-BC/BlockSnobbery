
## 🤘 BlockSnobbery: GOLOS/STEEM blockchain redis-streamer. 


* **One blocklistener for 10000 bots!** Connect to the blockstreamer with any number of scripts without creating undesirable load. Use the power and bandwidth of a redis pub/sub
* **No block will be missed** Checkpoint in case of a pause
* **No intervals hell** there are no intervals such as silly 3000ms
* **Clean & Native Websockets** No libraries required such as golos-js || steemjs
* **Redis cache for fast synchronization**

***

### 💾 Install

You should already have installed nodejs.org & redis.io

* `git clone https://github.com/vikxx/BlockSnobbery`
* `cd BlockSnobbery`
* `npm install ws`
* `npm install redis`
* `node snob.js now`

***
### ⚙️ Configure

Edit variables if required in snob.js
```
const WS = 'ws://localhost:9090'
const CHANNEL = "blockschannel"
const redKey = "lastblocknumber"
```
**WS** 
If you have a steem node installed, use address `ws://localhost:9090` 

You can also use public nodes: `wss://steemd.steemit.com` for STEEM 

or `wss://ws.golos.io` for GOLOS

**CHANNEL** - custom name of a redis channel 

**redKey** - custom redis key for a checkpoint block
***

### 🚀 Run

Start from the specified block:
`node snob.js 1234565`

Start with the newest block:
`node snob.js now`                                          

Start with the Last remembered block:                          
`node snob.js`       

***

Realtime block listener: `node snob.js now`


|Processed block num  | Age last ops |Current on Steem  | State  | Ops count  | Ops lenght  |   
|---|---|---|---|---|---|

![Capture.JPG](https://steemitimages.com/DQmee4VDMWHAqi3dKACkWGHMq63XP3ba6NAbj6DVJ9QV8Yt/Capture.JPG)

Processing Block History after pause: `node snob.js`

|Processed block | Age block |Current on Steem|State|Remain|Ops count| Ops lenght  |   
|---|---|---|---|---|---|---|

![Capture2.JPG](https://steemitimages.com/DQmbsDcqjFZQX9AQUj9ob8qJCtYDoSRNSAT7iZk4H2HvELP/Capture2.JPG)

### Processing more than 1000 missed blocks less than a second
Quick return to real time and synchronization after the downtime of your applications

![snob.gif](https://steemitimages.com/DQmRr5F4Hzxyyd6UmT2248FLCniTFQiAD3CQg1YMjU6wjXS/snob.gif)

## Connecting to a channel with operations

Use a redis SUB to get the blocks operations https://redis.io/topics/pubsub

For example:

```
const FilterOps = (op) => {
const [type, o] = op

if(type === "limit_order_create") // Do some func with: Order on market
 if(type === "comment"&&!o.parent_author)// Do some func with: post    
 if(type === "comment"&&o.parent_author)// Do some func with: comment   
 if(type === "vote" && o.weight > 0) // Do some func with: vote  
 if(type === "vote" && o.weight < 0) // Do some func with:  flag 
 if(type === "vote" && o.weight === 0) // Do some func with: downvote
 if(type === "custom_json") // Do some func with: follow, reblog etc. 
 if(type === "transfer") // Do some func with: transfer funds  
 if(type === "account_create")  // Do some func with:  account create 
 if(type === "account_update")  // Do some func with: profile update
 if(type === "account_witness_vote") // Do some func with: witness voting
 if(type === "feed_publish")  // Do some func with:  price feed
 if(type === "curation_reward")  // Do some func with:   curation reward
 if(type === "author_reward")   // Do some func with:  author reward
 //... etc


}


sub.on("message", (channel, message) => {
let ops = JSON.parse(message)
return ops.forEach(FilterOps)

})

sub.subscribe(SubRedisChannelName); 

```

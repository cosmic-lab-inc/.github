<p align="center">
  <a href="https://epoch.fm">
    <img alt="Epoch" src="./assets/logo.png" width="250px" style="border-radius: 50%;"/>
  </a>
</p>


<h1 align="center" style="font-size: 50px">
    Epoch ⏳
</h1>

Solana RPC nodes only serve the current state of accounts. The balance of your token account or the status of your 
gaming performance in Star Atlas at any point in the past is completely inaccessible via Solana RPC.

Historical account state is critical for analyzing trading performance over time, market impact of product release,
trend analysis such as NFT popularity, data mining, machine learning, business analytics of 
cash flows, and more.

🔔 Practically every business needs historical account state

The current state of affairs is companies relying on Flipside, Dust, or other web2 databases that listen to 
Solana updates as they arrive and store them in a database.
This has a few problems:
* Web2 databases are not decentralized, secure, or censorship resistant.
* These databases have started listening to data, but have not backfilled back to genesis. Therefore, much of 
  Solana's history is missing.
* These web2 databases only serve indexed data, or data that the community manually provided. Therefore, many 
  programs and account types are missing.
* These databases require querying through SQL, whereas Solana RPC is a JSON API. Personally, I don't need the SQL 
  layer, it's easier and faster to get an account using the Solana RPC interface.

Case in point, the existing solutions have an incomplete picture of Solana's history and ecosystem and have pitfalls 
that either omit, exclude, or otherwise make the data difficult to access.


🧪 This is why we created **Epoch**. 
Epoch is a decentralized, censorship resistant, and secure database of Solana's historical account state all the 
back to genesis, for every program, and every account type, all in its raw state. 
This allows developers to get any data they need and do with it as they please.

Solana's RPC method to get an account's current state (the most recent slot):
```typescript
import { Connection, PublicKey } from '@solana/web3.js';
const connection = new Connection('https://api.mainnet-beta.solana.com');
const publicKey = new PublicKey('...');
const accountInfo: AccountInfo<Buffer> = await connection.getAccountInfo(publicKey);
```

This is lacking the ability to query at a historical slot. The `EpochClient` wraps the Solana RPC client and provides 
a method to get an account's state at a historical slot:
```typescript
import { EpochClient } from '@cosmic-lab/epoch';
import { Connection, PublicKey } from '@solana/web3.js';
const connection = new Connection('https://api.mainnet-beta.solana.com');
const publicKey = new PublicKey('...');
const client = new EpochClient(connection);
const currentSlot = await connection.getSlot(); // 250_000_000
// slot occurs every 400ms, so there are 2.5 slots/second, or 216_000 slots per day
const slot30DaysAgo = currentSlot - 216_000 * 30; // 243_520_000
const accountInfo: AccountInfo<Buffer> = await client.getAccount(publicKey, slot30DaysAgo);
```

🚀 It's as easy as that to get an account's state at any point in time!

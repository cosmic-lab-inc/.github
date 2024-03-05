<p align="center">
  <a href="https://cosmiclab.io">
    <img alt="Epoch" src="./cosmic_wallet_transparent_512x512.png" width="300px" style="border-radius: 50%;"/>
  </a>
  <a href="https://cosmiclab.io">
    <img alt="Epoch" src="./epoch_logo.png" width="300px" style="border-radius: 50%;"/>
  </a>
</p>


Welcome to Cosmic Lab. Each of our projects solves a wide array of issues and challenges.
We are former Star Atlas and have worked at the forefront of blockchain since 2021.
Let's dig it to the pain points and how we solved them.


## 💸 #1: Covesting, AKA "Prop Trading"

Wall Street big wigs, such as brokerages and hedge funds, commonly engage in **proprietary trading**. 
This is when an institution outsources trading of stocks, bonds, and currencies to a profitable trader.

The institution has the capital, but neither the time nor expertise to trade profitably.
The trader has the expertise, but not the large amount capital.

The arrangement is typically a revenue share, with the trader keep a large percentage of the profits they generate
with the institution's capital.
Typically, the institution gets 20-50% of the profits earned by the trader.

For a trader who routinely does 2x returns per month, and a firm with $100,000 to covest,
the trader and institution with will split $100,000 per month in profits.

🍿 Here's the magic, and the powerful invention: **scoped transaction permissions**.

Until now, *covesting* hasn't existed in DeFi because it required "scoped permissions" of a trader's wallet.
The current paradigm is too insecure to enable such a product.
A party who covests assets essentially gives the trader authority to move them as they need to in order to generate 
profit. This means the trader can sign for any transaction, good or bad.

Though you intend for the trader to only use the assets to trade, they could withdraw them to a personal account and 
run away at any time, or trade them in a memecoin that you don't want to touch.

With "scoped permissions" the party who covests capital in a trader defines specific permissions for their wallet to 
sign transactions.
For example you can scope a wallet's transaction permissions to only allow under the following conditions:
* The instruction signed is by a specific program ID, such as Drift V2, in order to restrict trading to a 
  program/DEX you trust.
* The token account with assets being transferred contains specific mints, such as SOL and USDC, in order to 
  restrict trading activities to a trading pair you feel confident in.
* The program instruction being signed is of a certain type, such as Drift's "place limit order" or "cancel open order",
  instructions that are safe and necessary for trading, and **not capable of withdrawing/stealing assets** in any way.


<p align="center">
  <a href="https://cosmiclab.io">
    <img alt="Epoch" src="./epoch_banner.png" width="900px" style="border-radius: 10px;"/>
  </a>
</p>

## ⏳ #2: Historical Account State

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

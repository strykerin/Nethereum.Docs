# Using Nodesmith with Nethereum

This sample walks through the process of getting connected to Ethereum’s mainnet or test network using [Nodesmith](https://nodesmith.io). Nodesmith runs Ethereum nodes and provides access to them via API to eliminate the need to run and update your own infrastructure.

The first step to use Nodesmith is to [sign up](https://nodesmith.io) and get an API key. The next step is to choose which Ethereum network to connect to — either the mainnet, or the Kovan, Goerli, Rinkeby, or Ropsten test networks. Both of these will be used in the url we use to initial Nethereum with the format:`https://ethereum.api.nodesmith.io/v1/{network_name}/jsonrpc?apiKey={api_key}`.

For this sample, we’ll use a special API key `NETHEREUM_SAMPLE`, but for your own project you’ll need your own key.

```csharp
using Nethereum.Web3;
```

Let’s first define which network we want to connect to and create our url. Then create an instance of Web3 using that url.

```csharp
var networkName = "kovan"; // This can be mainnet, kovan, goerli, rinkeby, or ropsten
var apiKey = "NETHEREUM_SAMPLE"; // For a real application, create your own API key
var url = string.Format("https://ethereum.api.nodesmith.io/v1/{0}/jsonrpc?apiKey={1}", networkName, apiKey);
var web3 = new Web3(url);
```

Using the Net API, we can call and find out which network we’re connected to. This will change depending on which network we chose to connect to previously. For example, Kovan will return 42 and mainnet would be 1.

```csharp
var networkId = await web3.Net.Version.SendRequestAsync();
```

Next, using the Eth API, we’ll call to get the latest block which has been mined in this network.

```csharp
var latestBlockNumber = await web3.Eth.Blocks.GetBlockNumber.SendRequestAsync();
var latestBlock = await web3.Eth.Blocks.GetBlockWithTransactionsHashesByNumber.SendRequestAsync(latestBlockNumber);
```

That’s a quick demo of using Nethereum with Nodesmith. One important thing to know when using hosted infrastructure like Nodesmith is it doesn’t store any private keys, so any signing must be done locally and then the raw transaction passed on to the service. Nethereum makes this easy with the `Account` object. See the [Using account objects](nethereum-using-account-objects/#sending-a-transaction) for more details.

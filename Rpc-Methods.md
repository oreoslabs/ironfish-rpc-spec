# 2. RPC Methods
A specification of the standard interface for Ironfish node on version [v0.1.51](https://github.com/iron-fish/ironfish/releases/tag/v0.1.51).

## Account
The Account method group contains methods for interacting with the Ironfish wallet.

1. **Create**`accounts:create` returns the name and public address of the new created account. 

   Inputs:
   ```typescript
   type CreateAccountRequest = { name: string; default?: boolean }
   ```
   Response:
   ```typescript
   type CreateAccountResponse = {
     name: string
     publicAddress: string
     isDefaultAccount: boolean
   }
   ```
2. **Export**`accounts:exportAccount` returns the detailed account information to export.
   
   Inputs:
   ```typescript
   type ExportAccountRequest = { account?: string }
   ```
   Response:
   ```typescript
   type ExportAccountResponse = {
     account: {
       name: string
       spendingKey: string
       incomingViewKey: string
       outgoingViewKey: string
       publicAddress: string
     }
   }
   ```
3. **GetAccounts**`accounts:getAccounts` returns the account list.

   Inputs:
   ```typescript
   type GetAccountsRequest = { default?: boolean; displayName?: boolean } | undefined
   ```
   Response:
   ```typescript
   type GetAccountsResponse = { accounts: string[] }
   ```
4. **GetBalance**`accounts:getBalance` returns balance information of the account.

   Inputs:
   ```typescript
   type GetBalanceRequest = { account?: string; minimumBlockConfirmations?: number }
   ```
   Response:
   ```typescript
   type GetBalanceResponse = {
     account: string
     confirmed: string
     pending: string
     pendingCount: number
     unconfirmed: string
     unconfirmedCount: number
     minimumBlockConfirmations: number
   }
   ```
5. **GetDefaultAccount**`accounts:getDefaultAccount` returns the default account.

   Inputs:
   ```typescript
   type GetDefaultAccountRequest = {} | undefined
   ```
   Response:
   ```typescript
   type GetDefaultAccountResponse = { account: { name: string } | null }
   ```
6. **GetAccountNotesStream**`accounts:getAccountNotesStream` returns all notes of the account.

   Inputs:
   ```typescript
   type GetAccountNotesStreamRequest = { account?: string }
   ```
   Response:
   ```typescript
   type GetAccountNotesStreamResponse = {
     amount: string
     memo: string
     transactionHash: string
     spent: boolean | undefined
   }
   ```
7. **GetPublicKey**`accounts:getPublicKey` returns public address of the account.

   Inputs:
   ```typescript
   type GetPublicKeyRequest = { account?: string; generate?: boolean }
   ```
   Response:
   ```typescript
   type GetPublicKeyResponse = { account: string; publicKey: string }
   ```
8. **GetAccountsStatus**`accounts:getAccountsStatus` returns status of the account.

   Inputs:
   ```typescript
   type GetAccountStatusRequest = { account?: string }
   ```
   Response:
   ```typescript
   type GetAccountStatusResponse = {
     accounts: Array<{
       name: string
       id: string
       headHash: string
       headInChain: boolean
       sequence: string | number
     }>
   }
   ```
9. **GetAccountTransaction**`accounts:getAccountTransaction` returns transaction information of a specified transaction of the account.

   Inputs:
   ```typescript
    type GetAccountTransactionRequest = { account?: string; hash: string }
   ```
   Response:
   ```typescript
   type GetAccountTransactionResponse = {
     account: string
     transaction: {
       hash: string
       status: string
       isMinersFee: boolean
       fee: string
       blockHash?: string
       blockSequence?: number
       notesCount: number
       spendsCount: number
       notes: {
         value: string
         memo: string
         spent: boolean
       }[]
      } | null
   }
   ```
10. **GetAccountTransactions**`accounts:getAccountTransactions` returns all transactions of the account.

    Inputs:
    ```typescript
    type GetAccountTransactionsRequest = { account?: string; hash?: string; limit?: number }
    ```
    Response:
    ```typescript
    type GetAccountTransactionsResponse = {
      creator: boolean
      status: string
      hash: string
      isMinersFee: boolean
      fee: string
      notesCount: number
      spendsCount: number
      expirationSequence: number
    }
    ```
11. **ImportAccount**`accounts:importAccount` returns name of the imported account.

    Inputs:
    ```typescript
    type ImportAccountRequest = {
      account: {
        name: string
        spendingKey: string
        incomingViewKey: string
        outgoingViewKey: string
        publicAddress: string
     }
     rescan?: boolean
    }
    ```
    Response:
    ```typescript
    type ImportAccountResponse = {
      name: string
      isDefaultAccount: boolean
    }
    ```
12. **Remove**`accounts:remove` removes the specified account.

    Inputs:
    ```typescript
    type RemoveAccountRequest = { name: string; confirm?: boolean }
    ```
    Response:
    ```typescript
    ```
13. **RescanAccount**`accounts:rescanAccount` returns rescan status of the account.

    Inputs:
    ```typescript
    type RescanAccountRequest = { follow?: boolean; reset?: boolean }
    ```
    Response:
    ```typescript
    type RescanAccountResponse = { sequence: number; startedAt: number; endSequence: number }
    ```
14. **Use**`accounts:use`sets the account as default.

    Inputs:
    ```typescript
    type UseAccountRequest = { name: string }
    ```
    Response:
    ```typescript
    ```
## Chain
The Chain method group contains methods for interacting with the Ironfish blockchain.

1. **ExportChainStream**`chain:exportChainStream` returns specified blocks within request range.

   Inputs:
   ```typescript
   type ExportChainStreamRequest =
   | {
      start?: number | null
      stop?: number | null
   }
   | undefined
   ```
   Response:
   ```typescript
   type ExportChainStreamResponse = {
     start: number
     stop: number
     block?: { 
       hash: string
       seq: number
       prev: string
       main: boolean
       graffiti: string
       timestamp: number
       work: string
       difficulty: string
       head: boolean
       latest: boolean
    }
   }
   ```
2. **FollowChainStream**`chain:followChainStream` returns specified blocks within request range.

   Inputs:
   ```typescript
   type FollowChainStreamRequest =
           | {
      head?: string | null
   }
           | undefined
   ```
   Response:
   ```typescript
   type FollowChainStreamResponse = {
      type: 'connected' | 'disconnected' | 'fork'
      head: {
         sequence: number
      }
      block: {
         hash: string
         sequence: number
         previous: string
         graffiti: string
         difficulty: string
         size: number
         timestamp: number
         work: string
         main: boolean
         transactions: Array<{
            hash: string
            size: number
            fee: number
            notes: Array<{ commitment: string }>
            spends: Array<{ nullifier: string }>
         }>
      }
   }
   ```
3. **GetBlock**`chain:getBlock` returns specified block.

   Inputs:
   ```typescript
   type GetBlockRequest = { index?: number; hash?: string }
   ```
   Response:
   ```typescript
   interface Block {
     blockIdentifier: { index: string; hash: string }
     parentBlockIdentifier: { index: string; hash: string }
     timestamp: number
     transactions: Array<Transaction>
     metadata: {
       size: number
       difficulty: number
     }
   }
   ```
4. **GetBlockInfo**`chain:getBlockInfo` returns the block of specified block hash or sequence.

   Inputs:
   ```typescript
   type GetBlockInfoRequest = {
      search?: string
      hash?: string
      sequence?: number
   }
   ```
   Response:
   ```typescript
   type GetBlockInfoResponse = {
     block: {
       graffiti: string
       difficulty: string
       hash: string
       previousBlockHash: string
       sequence: number
       timestamp: number
       transactions: Array<{
         fee: string
         hash: string
         signature: string
         notes: number
         spends: number
       }>
     }
     metadata: {
       main: boolean
     }
   }
   ```
5. **GetChainInfo**`chain:getChainInfo` returns the current, heaviest and genesis block identifiers.

   Inputs:
   ```typescript
   ```
   Response:
   ```typescript
   interface ChainInfo {
     currentBlockIdentifier: BlockIdentifier
     genesisBlockIdentifier: BlockIdentifier
     oldestBlockIdentifier: BlockIdentifier
     currentBlockTimestamp: number
   }
   ```
6. **GetTransaction**`chain:getTransaction` returns serialized transaction data.

   Inputs:
   ```typescript
   type GetTransactionRequest = { blockHash: string; transactionHash: string }
   ```
   Response:
   ```typescript
   type GetTransactionResponse = {
     fee: string
     expirationSequence: number
     notesCount: number
     spendsCount: number
     signature: string
     notesEncrypted: string[]
   }
   ```
7. **GetTransactionStream**`chain:getTransactionStream` returns all decrypted transactions with specified viewKey.

   Inputs:
   ```typescript
   type GetTransactionStreamRequest = { incomingViewKey: string; head?: string | null }
   ```
   Response:
   ```typescript
   type GetTransactionStreamResponse = {
     type: 'connected' | 'disconnected' | 'fork'
     head: {
       sequence: number
     }
     block: {
       hash: string
       previousBlockHash: string
       sequence: number
       timestamp: number
     }
     transactions: Transaction[]
   }
   ```
8. **ShowChain**`chain:showChain` returns blockchain status.

   Inputs:
   ```typescript
   type ShowChainRequest =
           | {
      start?: number | null
      stop?: number | null
   }
           | undefined
   ```
   Response:
   ```typescript
   type ShowChainResponse = {
     content: string[]
   }
   ```
## Config
The Config method group contains methods for interacting with the Ironfish node environment config.

1. **GetConfig**`config:getConfig` returns the value of an environment attribute.

   Inputs:
   ```typescript
   type GetConfigRequest = { user?: boolean; name?: string } | undefined
   ```
   Response:
   ```typescript
   type GetConfigResponse = Partial<ConfigOptions>
   ```
   Refer to [ConfigOptions](https://github.com/iron-fish/ironfish/blob/v0.1.51/ironfish/src/fileStores/config.ts)

2. **SetConfig**`config:setConfig` sets the value of specified environment attribute.

   Inputs:
   ```typescript
   type SetConfigRequest = { name: string; value: unknown }
   ```
   Response:
   ```typescript
   type SetConfigResponse = Partial<ConfigOptions>
   ```
3. **UploadConfig**`config:uploadConfig` updates the value of specified environment attribute.

   Inputs:
   ```typescript
   type UploadConfigRequest = { config: Record<string, unknown> }
   ```
   Response:
   ```typescript
   type UploadConfigResponse = Partial<ConfigOptions>
   ```
## Event
The Event method group contains methods for interacting with the Ironfish p2p network.

1. **OnGossip**`event:onGossip` returns the received block on gossip message.

   Inputs:
   ```typescript
   type OnGossipRequest = undefined
   ```
   Response:
   ```typescript
   type OnGossipResponse = { blockHeader: RpcBlockHeader }
   ```
## Faucet
The faucet method group contains methods for interacting with the Ironfish faucet.

1. **GetFunds**`faucet:getFunds` requests token from faucet.

   Inputs:
   ```typescript
   type GetFundsRequest = { accountName: string; email?: string }
   ```
   Response:
   ```typescript
   type GetFundsResponse = { id: string }
   ```
## Fees
The fees method group contains methods for interacting with the Ironfish gas fee module.

1. **EstimateFee**`fees:estimateFee` returns value of estimated fee for specified transaction.

   Inputs:
   ```typescript
   type EstimateFeeRequest = {
     fromAccountName: string
     receives: {
       publicAddress: string
       amount: string
       memo: string
     }[]
   }
   ```
   Response:
   ```typescript
   type EstimateFeeResponse = {
     low: string
     medium: string
     high: string
   }
   ```
2. **EstimateFeeRates**`fees:estimateFeeRates` returns an estimated fee rate as ore/kb.

   Inputs:
   ```typescript
   type EstimateFeeRatesRequest = { priority?: PriorityLevel }
   ```
   Response:
   ```typescript
   type EstimateFeeRatesResponse = {
     low?: string
     medium?: string
     high?: string
   }
   ```
## Miner
The miner method group contains methods for pool mining.

1. **BlockTemplateStream**`miner:blockTemplateStream` returns blockTemplate for pool mining.

   Inputs:
   ```typescript
   type BlockTemplateStreamRequest = Record<string, never> | undefined
   ```
   Response:
   ```typescript
   type BlockTemplateStreamResponse = SerializedBlockTemplate
   ```
2. **SubmitBlock**`miner:submitBlock` submits mined block to Ironfish node.

   Inputs:
   ```typescript
   type SubmitBlockRequest = SerializedBlockTemplate
   ```
   Response:
   ```typescript
   type SubmitBlockResponse = {
     added: boolean
     reason:
     | 'UNKNOWN_REQUEST'
     | 'CHAIN_CHANGED'
     | 'INVALID_BLOCK'
     | 'ADD_FAILED'
     | 'FORK'
     | 'SUCCESS'
   }
   ```
3. **ExportMinedStream**`miner:exportMinedStream` returns all mined blocks in stream.

   Inputs:
   ```typescript
   type ExportMinedStreamRequest =
           | {
      blockHash?: string | null
      start?: number | null
      stop?: number | null
      forks?: boolean | null
   }
           | undefined
   ```
   Response:
   ```typescript
   type ExportMinedStreamResponse = {
     start?: number
     stop?: number
     sequence?: number
     block?: {
       hash: string
       minersFee: string
       sequence: number
       main: boolean
       account: string
     }
   }
   ```
## Node
The node method group contains methods for interacting with the Ironfish node.

1. **GetLogStream**`node:getLogStream` returns log information of a specified Ironfish node.

   Inputs:
   ```typescript
   type GetLogStreamRequest = {} | undefined
   ```
   Response:
   ```typescript
   type GetLogStreamResponse = {
     level: string
     type: string
     tag: string
     args: string
     date: string
   }
   ```
2. **GetStatus**`node:getStatus` returns status information of a specified Ironfish node.

   Inputs:
   ```typescript
   type GetNodeStatusRequest =
           | undefined
           | {
      stream?: boolean
   }
   ```
   Response:
   ```typescript
   type GetNodeStatusResponse = {
      node: {
         status: 'started' | 'stopped' | 'error'
         version: string
         git: string
         nodeName: string
      }
      cpu: {
         cores: number
         percentRollingAvg: number
         percentCurrent: number
      }
      memory: {
         heapMax: number
         heapTotal: number
         heapUsed: number
         rss: number
         memFree: number
         memTotal: number
      }
      miningDirector: {
         status: 'started'
         miners: number
         blocks: number
         blockGraffiti: string
         newBlockTemplateSpeed: number
         newBlockTransactionsSpeed: number
      }
      memPool: {
         size: number
         sizeBytes: number
      }
      blockchain: {
         synced: boolean
         head: {
            hash: string
            sequence: number
         }
         headTimestamp: number
         newBlockSpeed: number
      }
      blockSyncer: {
         status: 'stopped' | 'idle' | 'stopping' | 'syncing'
         syncing?: {
            blockSpeed: number
            speed: number
            downloadSpeed: number
            progress: number
         }
      }
      peerNetwork: {
         peers: number
         isReady: boolean
         inboundTraffic: number
         outboundTraffic: number
      }
      telemetry: {
         status: 'started' | 'stopped'
         pending: number
         submitted: number
      }
      workers: {
         started: boolean
         workers: number
         queued: number
         capacity: number
         executing: number
         change: number
         speed: number
      }
      accounts: {
         scanning?: {
            sequence: number
            endSequence: number
            startedAt: number
         }
         head: {
            hash: string
            sequence: number
         }
      }
   }
   ```
3. **StopNode**`node:stopNode` stops Ironfish node.

   Inputs:
   ```typescript
   type StopNodeRequest = undefined
   ```
   Response:
   ```typescript
   type StopNodeResponse = undefined
   ```
## Peer
The peer method group contains methods for interacting with the Ironfish node for peer status.

1. **GetPeer**`peer:getLogStream` returns status information of a specified peer.

   Inputs:
   ```typescript
   type GetPeerRequest = {
     identity: string
     stream?: boolean
   }
   ```
   Response:
   ```typescript
   type GetPeerResponse = {
     peer: PeerResponse | null
   }
   ```
2. **GetPeers**`peer:getPeers` returns connected peer list of a specified Ironfish node.

   Inputs:
   ```typescript
   type GetPeersRequest =
   | undefined
   | {
   stream?: boolean
   }
   ```
   Response:
   ```typescript
   type GetPeersResponse = {
     peers: Array<PeerResponse>
   }
   type PeerResponse = {
     state: string
     identity: string | null
     version: number | null
     head: string | null
     sequence: number | null
     work: string | null
     agent: string | null
     name: string | null
     address: string | null
     port: number | null
     error: string | null
     connections: number
     connectionWebSocket: ConnectionState
     connectionWebSocketError: string
     connectionWebRTC: ConnectionState
     connectionWebRTCError: string
   }
   ```
3. **GetPeerMessages**`peer:getPeerMessages` returns p2p network message from a specified peer.

   Inputs:
   ```typescript
   type GetPeerMessagesRequest = {
     identity: string
     stream?: boolean
   }
   ```
   Response:
   ```typescript
   type GetPeerMessagesResponse = {
     messages: Array<PeerMessage>
   }
   type PeerMessage = {
     brokeringPeerDisplayName?: string
     direction: 'send' | 'receive'
     message: {
       payload: string
       type: string
     }
     timestamp: number
     type: Connection['type']
   }
   ```
## Rpc
The rpc method group contains methods for interacting with the Ironfish node for rpc status.

1. **GetStatus**`rpc:getStatus` returns status information of rpc layer.

   Inputs:
   ```typescript
   type GetRpcStatusRequest =
           | undefined
           | {
      stream?: boolean
   }
   ```
   Response:
   ```typescript
   type GetRpcStatusResponse = {
     started: boolean
     adapters: {
       name: string
       inbound: number
       outbound: number
       readableBytes: number
       writableBytes: number
       readBytes: number
       writtenBytes: number
       clients: number
       pending: string[]
     }[]
   }
   ```
## Transaction
The transaction method group contains methods for transaction.

1. **SendTransaction**`transaction:sendTransaction` sends token to receiver.

   Inputs:
   ```typescript
   type SendTransactionRequest = {
     fromAccountName: string
     receives: { 
       publicAddress: string
       amount: string
       memo: string
     }[]
     fee: string
     expirationSequence?: number | null
     expirationSequenceDelta?: number | null
     ifSpendAllNotes?: boolean | null
   }
   ```
   Response:
   ```typescript
   type SendTransactionResponse = {
     receives: {
       publicAddress: string
       amount: string
       memo: string
     }[]
     fromAccountName: string
     hash: string
   }
   ```
## Worker
The worker method group contains methods for Ironfish node worker pool.

1. **GetStatus**`worker:getStatus` returns worker pool status of a specified Ironfish node.

   Inputs:
   ```typescript
   type GetWorkersStatusRequest =
           | undefined
           | {
      stream?: boolean
   }
   ```
   Response:
   ```typescript
   type GetWorkersStatusResponse = {
     started: boolean
     workers: number
     queued: number
     capacity: number
     executing: number
     change: number
     speed: number
     jobs: Array<{
       name: string
       complete: number
       execute: number
       queue: number
       error: number
     }>
   }
   ```

# rt-view setup notes

## Two machines

- LOCAL - your local machine (ubuntu box)
- REMOTE - the remote server running the node


## Local setup

### 1 - Check the public IP of LOCAL - x.x.x.x
`curl ifconfig.me`

### 2 - Download rt-view to LOCAL
`curl -L https://github.com/input-output-hk/cardano-rt-view/releases/download/0.3.0/cardano-rt-view-0.3.0-linux-x86_64.tar.gz`

### 3 - Unpack the downloaded rt-view
`tar -xvf cardano-rt-view-0.3.0-linux-x86_64.tar.gz`

### 4 - Run rt-view on LOCAL
`./cardano-rt-view`

### 5 - All questions use default answers EXCEPT:
`Now, indicate a host of machine RTView will be launched on (default is 0.0.0.0): x.x.x.x`


---------------------------------------------------------------------------------------------------------

## Remote setup

### 1 - The end of the rt-view config dialogue will print some settings that need to be copied.

Here is an example:

```json
"traceForwardTo": {
     "tag": "RemoteSocket",
     "contents": [
       "12.13.14.15",
       "3000"
     ]
   }
```

### 2 - Find and edit the REMOTE config file

Here is an example:

```json
{
  "ApplicationName": "cardano-sl",
  "ApplicationVersion": 1,
  "ByronGenesisFile": "mainnet-byron-genesis.json",
  "ByronGenesisHash": "5f20df933584822601f9e3f8c024eb5eb252fe8cefb24d1317dc3d432e940ebb",
  "LastKnownBlockVersion-Alt": 0,
  "LastKnownBlockVersion-Major": 2,
  "LastKnownBlockVersion-Minor": 0,
  "MaxKnownMajorProtocolVersion": 2,
  "Protocol": "Cardano",
  "RequiresNetworkMagic": "RequiresNoMagic",
  "ShelleyGenesisFile": "mainnet-shelley-genesis.json",
  "ShelleyGenesisHash": "1a3be38bcbb7911969283716ad7aa550250226b76a61fc51cc9a9a35d9276d81",
  "TraceBlockFetchClient": false,
  "TraceBlockFetchDecisions": false,
  "TraceBlockFetchProtocol": false,
  "TraceBlockFetchProtocolSerialised": false,
  "TraceBlockFetchServer": false,
 
  // rest of config data will appear here

}
```

### 3 - Place the copied data at the root of this file (at the end, one level within the main brackets)

Here is an example of the final result:
```json
{
  "ApplicationName": "cardano-sl",
  "ApplicationVersion": 1,
  "ByronGenesisFile": "mainnet-byron-genesis.json",
  "ByronGenesisHash": "5f20df933584822601f9e3f8c024eb5eb252fe8cefb24d1317dc3d432e940ebb",
  "LastKnownBlockVersion-Alt": 0,
  "LastKnownBlockVersion-Major": 2,
  "LastKnownBlockVersion-Minor": 0,
  "MaxKnownMajorProtocolVersion": 2,
  "Protocol": "Cardano",
  "RequiresNetworkMagic": "RequiresNoMagic",
  "ShelleyGenesisFile": "mainnet-shelley-genesis.json",
  "ShelleyGenesisHash": "1a3be38bcbb7911969283716ad7aa550250226b76a61fc51cc9a9a35d9276d81",
  "TraceBlockFetchClient": false,
  "TraceBlockFetchDecisions": false,
  "TraceBlockFetchProtocol": false,
  "TraceBlockFetchProtocolSerialised": false,
  "TraceBlockFetchServer": false,
  "TraceChainDb": true,
  "TraceChainSyncBlockServer": false,
  "TraceChainSyncClient": false,
  "TraceChainSyncHeaderServer": false,
  "TraceChainSyncProtocol": false,
  "TraceDNSResolver": true,
  "TraceDNSSubscription": true,
  "TraceErrorPolicy": true,
  "TraceForge": true,
  "TraceHandshake": false,
  "TraceIpSubscription": true,
  "TraceLocalChainSyncProtocol": false,
  "TraceLocalErrorPolicy": true,
  "TraceLocalHandshake": false,
  "TraceLocalTxSubmissionProtocol": false,
  "TraceLocalTxSubmissionServer": false,
  "TraceMempool": true,
  "TraceMux": false,
  "TraceTxInbound": false,
  "TraceTxOutbound": false,
  "TraceTxSubmissionProtocol": false,
  "TracingVerbosity": "NormalVerbosity",
  "TurnOnLogMetrics": true, // < ---------------------- make sure this is set to true
  "TurnOnLogging": true,
  "ViewMode": "SimpleView",
  "defaultBackends": [
    "KatipBK"
  ],
  "defaultScribes": [
    [
      "StdoutSK",
      "stdout"
    ]
  ],
  "hasEKG": 12788,
  "hasPrometheus": [
    "127.0.0.1",
    12798
  ],
  "minSeverity": "Info",
  "options": {
    "mapBackends": {
      "cardano.node.metrics": [
        "EKGViewBK"
      ],
      "cardano.node.BlockFetchDecision.peers": [
        "EKGViewBK"
      ]
    },
    "mapSubtrace": {
      "#ekgview": {
        "contents": [
          [
            {
              "contents": "cardano.epoch-validation.benchmark",
              "tag": "Contains"
            },
            [
              {
                "contents": ".monoclock.basic.",
                "tag": "Contains"
              }
            ]
          ],
          [
            {
              "contents": "cardano.epoch-validation.benchmark",
              "tag": "Contains"
            },
            [
              {
                "contents": "diff.RTS.cpuNs.timed.",
                "tag": "Contains"
              }
            ]
          ],
          [
            {
              "contents": "#ekgview.#aggregation.cardano.epoch-validation.benchmark",
              "tag": "StartsWith"
            },
            [
              {
                "contents": "diff.RTS.gcNum.timed.",
                "tag": "Contains"
              }
            ]
          ]
        ],
        "subtrace": "FilterTrace"
      },
      "benchmark": {
        "contents": [
          "GhcRtsStats",
          "MonotonicClock"
        ],
        "subtrace": "ObservableTrace"
      },
      "cardano.epoch-validation.utxo-stats": {
        "subtrace": "NoTrace"
      },
      "cardano.node.metrics": {
        "subtrace": "Neutral"
      }
    }
  },
  "rotation": {
    "rpKeepFilesNum": 10,
    "rpLogLimitBytes": 5000000,
    "rpMaxAgeHours": 24
  },
  "setupBackends": [
    "KatipBK"
  ],
  "setupScribes": [
    {
      "scFormat": "ScText",
      "scKind": "StdoutSK",
      "scName": "stdout",
      "scRotation": null
    }
  ],
  //  \/  \/  \/  \/  \/  \/  \/ - paste the copied data here
  "traceForwardTo": {
    "tag": "RemoteSocket",
    "contents": [
      "x.x.x.x",
      "3000"
    ]
  }
  //  /\  /\  /\  /\  /\  /\  /\
}
```


---------------------------------------------------------------------------------------------------------


## Ready to launch

Start rt-view on LOCAL  
Start node on REMOTE  
View rt-view on http://x.x.x.x:8024  
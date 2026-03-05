# hyperlane实现跨链教程(已发行的公链)

## 1)安装nodejs
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 18
nvm --version
nvm use 18
node -v
```

## 2)安装foudry
```
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup
cast --version
```


## 3)安装cli工具
```
npm uninstall -g @hyperlane-xyz/cli
npm i -g @hyperlane-xyz/cli@latest
```

## 4)安装git与unzip
```
sudo apt update
sudo apt install -y git
sudo apt install -y unzip
unzip -v
git --version
```

## 5)测试 registry 访问
```
node -e "fetch('https://raw.githubusercontent.com/hyperlane-xyz/hyperlane-registry/main/chains.yaml').then(()=>console.log('ok')).catch(console.error)"
必须打印ok，才能下一步
```

## 6)克隆注册表并解压
```
git clone  https://github.com/hyperlane-xyz/hyperlane-registry
ps:如果git拉取报错,那么手动下载下来
mv  hyperlane-registry-main  hyperlane-registry
unzip hyperlane-registry-main
cd hyperlane-registry
```

## 7)正式安装依赖
```
root@ubuntu:~/hyperlane-registry# npm uninstall -g @hyperlane-xyz/cli
removed 1 package in 331ms

root@ubuntu:~/hyperlane-registry# npm install -g @hyperlane-xyz/cli
added 1 package in 4s
```

## 8)查看hyperlane版本
```
root@ubuntu:~/hyperlane-registry# hyperlane --version
Hyperlane CLI
25.0.0
```

## 9)创建链配置
```  
root@ubuntu:~/hyperlane-registry# hyperlane registry init
Hyperlane CLI
Creating a new chain config
? Enter http or https rpc url: https://bsc.drpc.org
? Enter chain name (one word, lower case) bsc
? Enter chain display name Bsc
? Using chain id as 56 from JSON RPC provider, is this correct? yes
? Is this chain a testnet (a chain used for testing & development)? no
? Select the chain technical stack other
? Do you want to add a block explorer config for this chain yes
? Enter a human readable name for the explorer: https://bscscan.com
? Enter the base URL for the explorer: https://bscscan.com
? Enter the base URL for requests to the explorer API: https://api.etherscan.io/v2/api?chainid=56
? Select the type (family) of block explorer: etherscan
? Optional: Provide an API key for the explorer, or press 'enter' to skip. Please be sure to remove this field if you intend to add your config to the Hyperlane registry: GP69JEAP2W7YFJT9ZJTEPGQT6Y6KMW44ZN
? Do you want to set block or gas properties for this chain config no
? Do you want to set native token properties for this chain config (defaults to ETH) yes
? Enter the native token's symbol: BNB
? Enter the native token's name: BNB
? Enter the native token's decimals: 18
Chain config is valid, writing unsorted to registry:
    blockExplorers:
      - apiKey: GP69JEAP2W7YFJT9ZJTEPGQT6Y6KMW44ZN
        apiUrl: https://api.etherscan.io/v2/api?chainid=56
        family: etherscan
        name: https://bscscan.com
        url: https://bscscan.com
    chainId: 56
    displayName: Bsc
    domainId: 56
    isTestnet: false
    name: bsc
    nativeToken:
      decimals: 18
      name: BNB
      symbol: BNB
    protocol: ethereum
    rpcUrls:
      - http: https://bsc.drpc.org
    technicalStack: other
    
Skipping updating chain bsc at github registry (not supported)
Now updating chain bsc at filesystem registry at /root/.hyperlane
Done updating chain bsc at filesystem registry
```
## 10）初始化核心配置
```
root@ubuntu:~/hyperlane-registry# export HYP_KEY='你的私钥不带0x'
root@ubuntu:~/hyperlane-registry# hyperlane core init 
Hyperlane CLI
Hyperlane Core Configure
________________________
Creating a new core deployment config...
? Using owner address as 0x574B09A63a6B8436CC3eEB23414b7f59d43B5883 from signer, is this correct? yes
Creating trustedRelayerIsm...
Created trustedRelayerIsm!
Creating merkleTreeHook...
Created merkleTreeHook!
Creating protocolFee...
? Use this same address (0x574B09A63a6B8436CC3eEB23414b7f59d43B5883) for the beneficiary? yes
Created protocolFee!
Core config is valid, writing to file ./configs/core-config.yaml:

    owner: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
    defaultIsm:
      type: trustedRelayerIsm
      relayer: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
    defaultHook:
      type: merkleTreeHook
    requiredHook:
      owner: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
      type: protocolFee
      beneficiary: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
      maxProtocolFee: "100000000000000000"
      protocolFee: "0"
    proxyAdmin:
      owner: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
    
✅ Successfully created new core deployment config.
```

## 11）部署核心合约
```
root@ubuntu:~/hyperlane-registry# hyperlane core deploy
Hyperlane CLI
? Select network type Mainnet
? Select chain to connect: 
✔ Select chain to connect: bsc
Hyperlane Core deployment
_________________________

Deployment plan
===============
Transaction signer and owner of new contracts: 0x574B09A63a6B8436CC3eEB23414b7f59d43B5883
Deploying core contracts to network: bsc
┌────────────────────────┬─────────────────────────────────────────────────────────────────────┐
│        (index)         │                               Values                                │
├────────────────────────┼─────────────────────────────────────────────────────────────────────┤
│          Name          │                                'bsc'                                │
│      Display Name      │                                'Bsc'                                │
│        Chain ID        │                                 56                                  │
│       Domain ID        │                                 56                                  │
│        Protocol        │                             'ethereum'                              │
│      JSON RPC URL      │ 'https://bsc-mainnet.infura.io/v3/e463f6ea90ed48c69b353530d89babb9' │
│  Native Token: Symbol  │                                'BNB'                                │
│   Native Token: Name   │                                'BNB'                                │
│ Native Token: Decimals │                                 18                                  │
└────────────────────────┴─────────────────────────────────────────────────────────────────────┘
Note: There are several contracts required for each chain, but contracts in your provided registries will be skipped.
? Mailbox already exists at 0x2971b9Aec44bE4eb673DF1B88cDB57b96eefe8a4. Are you sure you want to deploy a new mailbox and overwrite existing registry artifacts? no
Error: Deployment cancelled
    at confirmExistingMailbox (file:///root/.nvm/versions/node/v18.20.1/lib/node_modules/@hyperlane-xyz/cli/bundle/index.js:561943:19)
    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
    at async runDeployPlanStep (file:///root/.nvm/versions/node/v18.20.1/lib/node_modules/@hyperlane-xyz/cli/bundle/index.js:561928:5)
    at async runCoreDeploy (file:///root/.nvm/versions/node/v18.20.1/lib/node_modules/@hyperlane-xyz/cli/bundle/index.js:561620:5)
    at async Object.handler (file:///root/.nvm/versions/node/v18.20.1/lib/node_modules/@hyperlane-xyz/cli/bundle/index.js:557115:9)
ps：由于hyperlane已经部署了bsc与monad的核心，所以我们不用部署了,可参考https://github.com/hyperlane-xyz/hyperlane-registry/tree/main
```

## 12） 初始化 Warp Route 配置
```
root@ubuntu:~/hyperlane-registry# hyperlane warp init
Hyperlane CLI
Hyperlane Warp Configure
________________________
Creating a new warp route deployment config...
? Select network type Mainnet
? Select chains to connect bsc, monad
? Is this chain selection correct?: bsc, monad yes
bsc: Configuring warp route...
? Using owner address as 0x574B09A63a6B8436CC3eEB23414b7f59d43B5883 from signer, is this correct? yes
? Use an existing Proxy Admin contract for the warp route deployment on chain "bsc"? yes
? Please enter the address of the Proxy Admin contract to be used on chain "bsc": 0x65993Af9D0D3a64ec77590db7ba362D6eB78eF70
? Do you want to use a trusted ISM for warp route? yes
? Select bsc's token type collateral
? Enter the existing token address on chain bsc 0x55d398326f99059fF775485246999027B3197955
monad: Configuring warp route...
? Using owner address as 0x574B09A63a6B8436CC3eEB23414b7f59d43B5883 from signer, is this correct? yes
? Use an existing Proxy Admin contract for the warp route deployment on chain "monad"? yes
? Please enter the address of the Proxy Admin contract to be used on chain "monad": 0x2f2aFaE1139Ce54feFC03593FeE8AB2aDF4a85A7
? Do you want to use a trusted ISM for warp route? yes
? Select monad's token type synthetic
Warp Route config is valid, writing to file undefined:

    bsc:
      type: collateral
      token: "0x55d398326f99059fF775485246999027B3197955"
      owner: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
      interchainSecurityModule:
        type: staticAggregationIsm
        modules:
          - type: trustedRelayerIsm
            relayer: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
          - owner: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
            type: defaultFallbackRoutingIsm
            domains: {}
        threshold: 1
      proxyAdmin:
        owner: "0x7379D7bB2ccA68982E467632B6554fD4e72e9431"
        address: "0x65993Af9D0D3a64ec77590db7ba362D6eB78eF70"
    monad:
      isNft: false
      type: synthetic
      owner: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
      interchainSecurityModule:
        type: staticAggregationIsm
        modules:
          - type: trustedRelayerIsm
            relayer: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
          - owner: "0x574B09A63a6B8436CC3eEB23414b7f59d43B5883"
            type: defaultFallbackRoutingIsm
            domains: {}
        threshold: 1
      proxyAdmin:
        owner: "0x9126696d9C3c44dc1273352ce171E359b1802560"
        address: "0x2f2aFaE1139Ce54feFC03593FeE8AB2aDF4a85A7"
    
? Using warp route ID as USDT/monad from warp deployment config, is this correct? yes
Skipping adding warp route deploy config at github registry (not supported)
Now adding warp route deploy config at filesystem registry at /root/.hyperlane
Done adding warp route deploy config at filesystem registry
✅ Successfully created new warp route deployment config with warp route id: USDT/monad
```

## 13）部署 Warp Route
```
root@ubuntu:~/hyperlane-registry# hyperlane warp deploy
Hyperlane CLI
✔ Select a warp route: USDT/monad
Hyperlane Warp Route Deployment
_______________________________

Warp Route Deployment Plan
==========================
📋 Token Standard: ERC20
📋 Warp Route Config:
┌─────────┬───────┬──────────────┬──────────────────────────────────────────────┬──────────────────────────────────────────────┬───────────────────────┐
│ (index) │ NFT?  │     Type     │                    Owner                     │                   Mailbox                    │     ISM Config(s)     │
├─────────┼───────┼──────────────┼──────────────────────────────────────────────┼──────────────────────────────────────────────┼───────────────────────┤
│   bsc   │ false │ 'collateral' │ '0x574B09A63a6B8436CC3eEB23414b7f59d43B5883' │ '0x2971b9Aec44bE4eb673DF1B88cDB57b96eefe8a4' │ 'See table(s) below.' │
│  monad  │ false │ 'synthetic'  │ '0x574B09A63a6B8436CC3eEB23414b7f59d43B5883' │ '0x3a464f746D23Ab22155710f44dB16dcA53e0775E' │ 'See table(s) below.' │
└─────────┴───────┴──────────────┴──────────────────────────────────────────────┴──────────────────────────────────────────────┴───────────────────────┘
📋 bsc ISM Config(s):
┌───────────┬────────────────────────┐
│  (index)  │         Values         │
├───────────┼────────────────────────┤
│   Type    │ 'staticAggregationIsm' │
│ Threshold │           1            │
│  Modules  │ 'See table(s) below.'  │
└───────────┴────────────────────────┘
┌─────────┬──────────────────────────────────────────────┐
│ (index) │                    Values                    │
├─────────┼──────────────────────────────────────────────┤
│  Type   │             'trustedRelayerIsm'              │
│ Relayer │ '0x574B09A63a6B8436CC3eEB23414b7f59d43B5883' │
└─────────┴──────────────────────────────────────────────┘
┌─────────────────┬──────────────────────────────────────────────┐
│     (index)     │                    Values                    │
├─────────────────┼──────────────────────────────────────────────┤
│      Type       │         'defaultFallbackRoutingIsm'          │
│      Owner      │ '0x574B09A63a6B8436CC3eEB23414b7f59d43B5883' │
│ Owner Overrides │                 'Undefined'                  │
│     Domains     │ 'See warp config for domain specification.'  │
└─────────────────┴──────────────────────────────────────────────┘
📋 monad ISM Config(s):
┌───────────┬────────────────────────┐
│  (index)  │         Values         │
├───────────┼────────────────────────┤
│   Type    │ 'staticAggregationIsm' │
│ Threshold │           1            │
│  Modules  │ 'See table(s) below.'  │
└───────────┴────────────────────────┘
┌─────────┬──────────────────────────────────────────────┐
│ (index) │                    Values                    │
├─────────┼──────────────────────────────────────────────┤
│  Type   │             'trustedRelayerIsm'              │
│ Relayer │ '0x574B09A63a6B8436CC3eEB23414b7f59d43B5883' │
└─────────┴──────────────────────────────────────────────┘
┌─────────────────┬──────────────────────────────────────────────┐
│     (index)     │                    Values                    │
├─────────────────┼──────────────────────────────────────────────┤
│      Type       │         'defaultFallbackRoutingIsm'          │
│      Owner      │ '0x574B09A63a6B8436CC3eEB23414b7f59d43B5883' │
│ Owner Overrides │                 'Undefined'                  │
│     Domains     │ 'See warp config for domain specification.'  │
└─────────────────┴──────────────────────────────────────────────┘
? Is this deployment plan correct? yes
Running pre-flight checks for chains...
✅ Bsc signer is valid
✅ Monad signer is valid
✅ Chains are valid
✅ Balances are sufficient
🚀 All systems ready, captain! Beginning deployment...
Loading registry factory addresses for bsc...
Creating staticAggregationIsm ISM for token on bsc chain...
Finished creating staticAggregationIsm ISM for token on bsc chain.
Deploying trustedRelayerIsm on bsc with constructor args (0x2971b9Aec44bE4eb673DF1B88cDB57b96eefe8a4, 0x574B09A63a6B8436CC3eEB23414b7f59d43B5883)...
Loading registry factory addresses for monad...
Creating staticAggregationIsm ISM for token on monad chain...
Finished creating staticAggregationIsm ISM for token on monad chain.
Deploying trustedRelayerIsm on monad with constructor args (0x3a464f746D23Ab22155710f44dB16dcA53e0775E, 0x574B09A63a6B8436CC3eEB23414b7f59d43B5883)...
Pending https://mainnet-beta.monvision.io/tx/0x466b722ae63f84b3ee7c21530e63e41c660b5aa518590d4a78fc2a520de5c5a0 (waiting 1 blocks for confirmation)
Pending https://bscscan.com/tx/0xb1189bcfab72e1fc62d849db6b212d2c45606e2353ec5ce294bd20c069a9018a (waiting 1 blocks for confirmation)
Pending https://mainnet-beta.monvision.io/tx/0x1d5f90ef5ab80f9d89ff4bdd89a574aa83cb172e6b2fb8cf66062ccdb5cd7a3f (waiting 1 blocks for confirmation)
Pending https://mainnet-beta.monvision.io/tx/0xd4fdb85c662d29eb0298e784d1d7ff925839f6ce73df6462385b7951106451bc (waiting 1 blocks for confirmation)
Pending https://mainnet-beta.monvision.io/tx/0xdc1017e90eb12cb7bf57764104d72ce1ab527344c9cec13618eab0700b4df73f (waiting 1 blocks for confirmation)
Config Hook is empty, skipping deployment.
Pending https://bscscan.com/tx/0x018da2f41e34243a0658ae9f9cf94a03cdddbd1cce72bbfd6dd3df4c6bec3b65 (waiting 1 blocks for confirmation)
Pending https://bscscan.com/tx/0x90ab76b02dc4d40941773299b425fa03f5b5ea1bd6adcb0974e54f5f15b89ed6 (waiting 1 blocks for confirmation)
Pending https://bscscan.com/tx/0xcedebcaf34090aed971038684ebffac7e19363fa947c5cc119f16a8ec47ea534 (waiting 1 blocks for confirmation)
Config Hook is empty, skipping deployment.
Deploying to bsc from https://bscscan.com/address/0x574B09A63a6B8436CC3eEB23414b7f59d43B5883
Deploying to monad from https://mainnet-beta.monvision.io/address/0x574B09A63a6B8436CC3eEB23414b7f59d43B5883
Deploying HypERC20 on monad with constructor args (18, 1, 0x3a464f746D23Ab22155710f44dB16dcA53e0775E)...
Deploying HypERC20Collateral on bsc with constructor args (0x55d398326f99059fF775485246999027B3197955, 1, 0x2971b9Aec44bE4eb673DF1B88cDB57b96eefe8a4)...
Pending https://mainnet-beta.monvision.io/tx/0xa9383a62dbd056f8a2fbf6bdc8aef3538e2d3134ab4b6de9037fbf7ba72f72f0 (waiting 1 blocks for confirmation)
Pending https://bscscan.com/tx/0x7240ee00337ecc1df6acd7dac9cfa6ecd3663d230268e54b6c82e5b24bd70900 (waiting 1 blocks for confirmation)
Pending https://mainnet-beta.monvision.io/tx/0xb8d79d47f3c6df83c0f0a39f261cbf1b9dd7290c0ec5e5d96cdd06e1d9144d1b (waiting 1 blocks for confirmation)
Deploying TransparentUpgradeableProxy on monad with constructor args (0xf64993A0EDDef20eb9D6a92958442a8635DFEfBc, 0x2f2aFaE1139Ce54feFC03593FeE8AB2aDF4a85A7, 0xe80a7c79000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000f034a58d9f2156dd16bb5302ce5428343b19914000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883000000000000000000000000000000000000000000000000000000000000000a546574686572205553440000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000045553445400000000000000000000000000000000000000000000000000000000)...
Pending https://mainnet-beta.monvision.io/tx/0xa2ed83e4b1d48216e73cace732b703e9bb9492a5713b4ef99b623290812192dd (waiting 1 blocks for confirmation)
Pending https://bscscan.com/tx/0x3b6f2d97f3fba3ebe36cce5e1e19c54aae7fb7219be41b6c3a74e53d378cb9ed (waiting 1 blocks for confirmation)
Successfully deployed contracts on monad
Deploying TransparentUpgradeableProxy on bsc with constructor args (0xE9cbF5D3Becab5aB2A0CFc3052cE71560A700663, 0x65993Af9D0D3a64ec77590db7ba362D6eB78eF70, 0xc0c53b8b0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000caa6d4e09cddc432a7537c0bec232e8a8576a520000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883)...
Pending https://bscscan.com/tx/0xec5a12228f0bcd36ca446b6f82b7db4958fee8d8150a93c71bfda7455d50126d (waiting 1 blocks for confirmation)
Successfully deployed contracts on bsc
Pending https://mainnet-beta.monvision.io/tx/0x05e8f8ab2513bc0483b56c74a2decd3895dd1eed7278439b4b401bad89eee06d (waiting 1 blocks for confirmation)
Pending https://bscscan.com/tx/0x4fd8dfe626ad28117efd9d539cf73b74e0af400a311dce49a26e50e7dc3d62d1 (waiting 1 blocks for confirmation)
Pending https://mainnet-beta.monvision.io/tx/0x5a004031ab1400f5d64fda8575cfa1c3d31f845da1d42999dbad5ade2e90d7a4 (waiting 1 blocks for confirmation)
Pending https://bscscan.com/tx/0xc756c94cdbd61effc5020bc5f9728524c353b2d6b6ea24c6f436632135bad51b (waiting 1 blocks for confirmation)
✅ Warp contract deployments complete
Start enrolling cross chain routers
Comparing target ISM config with monad chain
Comparing target Hook config with current one for monad chain
Comparing target ISM config with bsc chain

Comparing target Hook config with current one for bsc chain
Writing deployment artifacts...
Skipping adding warp route at github registry (not supported)
Now adding warp route at filesystem registry at /root/.hyperlane
Done adding warp route at filesystem registry
    tokens:
      - chainName: monad
        standard: EvmHypSynthetic
        decimals: 18
        symbol: USDT
        name: Tether USD
        addressOrDenom: "0x67752c58D0592BB3b46d372341D85E7e8DF1b09B"
        connections:
          - token: ethereum|bsc|0x959bEa75eB8247917Ea4602B0bE0043885834f57
      - chainName: bsc
        standard: EvmHypCollateral
        decimals: 18
        symbol: USDT
        name: Tether USD
        addressOrDenom: "0x959bEa75eB8247917Ea4602B0bE0043885834f57"
        collateralAddressOrDenom: "0x55d398326f99059fF775485246999027B3197955"
        connections:
          - token: ethereum|monad|0x67752c58D0592BB3b46d372341D85E7e8DF1b09B
    
⛽️ Gas Usage Statistics
	- Gas required for warp deploy on bsc: 0.00829895 BNB
	- Gas required for warp deploy on monad: 0.7870422 MON
```
## ps:如果是原生的主币的话，源链选择native，目标链选synthetic

## 14）授权给warp_router
```
root@ubuntu:~/hyperlane-registry# export HYP_KEY='私钥不带0x'
root@ubuntu:~/hyperlane-registry# cast send 0x55d398326f99059fF775485246999027B3197955 \
  "approve(address,uint256)" \
  0x959bEa75eB8247917Ea4602B0bE0043885834f57 \
  115792089237316195423570985008687907853269984665640564039457584007913129639935 \
  --rpc-url https://bsc-mainnet.infura.io/v3/e463f6ea90ed48c69b353530d89babb9 \
  --private-key $HYP_KEY

blockHash            0xaa297a6a5e874bccf021ac02ca2a9d0db0fd07ead4bf2edf87673b3a0461d679
blockNumber          79592285
contractAddress      
cumulativeGasUsed    9895321
effectiveGasPrice    50000000
from                 0x574B09A63a6B8436CC3eEB23414b7f59d43B5883
gasUsed              46506
logs                 [{"address":"0x55d398326f99059ff775485246999027b3197955","topics":["0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925","0x000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883","0x000000000000000000000000959bea75eb8247917ea4602b0be0043885834f57"],"data":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","blockHash":"0xaa297a6a5e874bccf021ac02ca2a9d0db0fd07ead4bf2edf87673b3a0461d679","blockNumber":"0x4be7b5d","blockTimestamp":"0x698594ed","transactionHash":"0x9d93bebf77ea1834354661a9e6960196a3faf5ed868530053583f80aa0e81f07","transactionIndex":"0x4a","logIndex":"0x129","removed":false}]
logsBloom            0x00000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000001000000000010000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000001000000008000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000020000000000000000010000000000400000000000000000000000000000000000000000000000000
root                 
status               1 (success)
transactionHash      0x9d93bebf77ea1834354661a9e6960196a3faf5ed868530053583f80aa0e81f07
transactionIndex     74
type                 2
blobGasPrice         
blobGasUsed          
to                   0x55d398326f99059fF775485246999027B3197955
```
## ps:原生主币跨链不需要授权给warp_router，直接转账即可

## 15）执行erc20跨链转账，bsc=>monad
```
root@ubuntu:~/hyperlane-registry# hyperlane warp send \
  --symbol USDT \
  --origin bsc \
  --destination monad \
  --amount 100000000000000000 \
  --relay
Hyperlane CLI
Multiple warp routes found for symbol USDT
? Select from matching warp routes USDT/monad
🚀 Sending a message for chains: bsc ➡️ monad
Running pre-flight checks for chains...
✅ Bsc signer is valid
✅ Monad signer is valid
✅ Chains are valid
✅ Balances are sufficient
Sending a message from bsc to monad
Pending https://bscscan.com/tx/0x7b257d0559fecc0aa57879bd7c8f0798e13cad04f1ffc15acd6a71ddb9a081ac (waiting 1 blocks for confirmation)
Sent transfer from sender (0x574B09A63a6B8436CC3eEB23414b7f59d43B5883) on bsc to recipient (0x574B09A63a6B8436CC3eEB23414b7f59d43B5883) on monad.
Message ID: 0xcbd3885fb16c0c02d2b555bdb252eb2b3153bcce3e59456444f5185be7ddff93
Explorer Link: https://explorer.hyperlane.xyz/message/0xcbd3885fb16c0c02d2b555bdb252eb2b3153bcce3e59456444f5185be7ddff93
Message:
    id: "0xcbd3885fb16c0c02d2b555bdb252eb2b3153bcce3e59456444f5185be7ddff93"
    message: "0x0300058d0700000038000000000000000000000000959bea75eb8247917ea4602b0\
      be0043885834f570000008f00000000000000000000000067752c58d0592bb3b46d372341d85e\
      7e8df1b09b000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883000\
      000000000000000000000000000000000000000000000016345785d8a0000"
    parsed:
      version: 3
      nonce: 363783
      origin: 56
      sender: "0x000000000000000000000000959bea75eb8247917ea4602b0be0043885834f57"
      destination: 143
      recipient: "0x00000000000000000000000067752c58d0592bb3b46d372341d85e7e8df1b09b"
      body: "0x000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883000000\
        000000000000000000000000000000000000000000016345785d8a0000"
    
Body:
    recipient: "0x000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883"
    amount: 100000000000000000
    
Attempting self-relay of message...
Preparing to relay message 0xcbd3885fb16c0c02d2b555bdb252eb2b3153bcce3e59456444f5185be7ddff93
Failed to build for submodule 0: Merkle proofs are not yet supported
Relaying message 0xcbd3885fb16c0c02d2b555bdb252eb2b3153bcce3e59456444f5185be7ddff93
Pending https://mainnet-beta.monvision.io/tx/0xf5e484f2449cfb5fc06a35969b1ae230d09570e9b39d43f79f12787aec8a8067 (waiting 1 blocks for confirmation)
Transfer was self-relayed!
✅ Successfully sent messages for chains: bsc ➡️ monad
```

## 16) 执行erc20跨链转账，monad=>bsc
```
root@ubuntu:~/hyperlane-registry# hyperlane warp send \
  --symbol USDT \
  --origin monad \
  --destination bsc \
  --amount 100000000000000000 \
  --relay
Hyperlane CLI
Multiple warp routes found for symbol USDT
? Select from matching warp routes USDT/monad
🚀 Sending a message for chains: monad ➡️ bsc
Running pre-flight checks for chains...
✅ Monad signer is valid
✅ Bsc signer is valid
✅ Chains are valid
✅ Balances are sufficient
Sending a message from monad to bsc
Pending https://mainnet-beta.monvision.io/tx/0xc22265fab01dbe361063f45966c1364eb9afa48a2f2f39f1fab5a31e72bb8c67 (waiting 1 blocks for confirmation)
Sent transfer from sender (0x574B09A63a6B8436CC3eEB23414b7f59d43B5883) on monad to recipient (0x574B09A63a6B8436CC3eEB23414b7f59d43B5883) on bsc.
Message ID: 0x6698d858b99091eb140f855e4911123fbfd84d8021a1e7dd734214b2f3bda38f
Explorer Link: https://explorer.hyperlane.xyz/message/0x6698d858b99091eb140f855e4911123fbfd84d8021a1e7dd734214b2f3bda38f
Message:
    id: "0x6698d858b99091eb140f855e4911123fbfd84d8021a1e7dd734214b2f3bda38f"
    message: "0x03000000740000008f00000000000000000000000067752c58d0592bb3b46d37234\
      1d85e7e8df1b09b00000038000000000000000000000000959bea75eb8247917ea4602b0be004\
      3885834f57000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883000\
      000000000000000000000000000000000000000000000016345785d8a0000"
    parsed:
      version: 3
      nonce: 116
      origin: 143
      sender: "0x00000000000000000000000067752c58d0592bb3b46d372341d85e7e8df1b09b"
      destination: 56
      recipient: "0x000000000000000000000000959bea75eb8247917ea4602b0be0043885834f57"
      body: "0x000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883000000\
        000000000000000000000000000000000000000000016345785d8a0000"
    
Body:
    recipient: "0x000000000000000000000000574b09a63a6b8436cc3eeb23414b7f59d43b5883"
    amount: 100000000000000000
    
Attempting self-relay of message...
Preparing to relay message 0x6698d858b99091eb140f855e4911123fbfd84d8021a1e7dd734214b2f3bda38f
Failed to build for submodule 1: Merkle proofs are not yet supported
Relaying message 0x6698d858b99091eb140f855e4911123fbfd84d8021a1e7dd734214b2f3bda38f
```

## 17) 执行bnb跨链转账，bsc=>bee
```
root@ubuntu:~/hyperlane-registry# hyperlane warp send \
  --symbol BNB \
  --origin bsctestnet \
  --destination bee \
  --amount 10000000000000000 \
  --relay
Hyperlane CLI
Multiple warp routes found for symbol BNB
? Select from matching warp routes BNB/warp-route-bnb-deploy-config
🚀 Sending a message for chains: bsctestnet ➡️ bee
Running pre-flight checks for chains...
✅ BSC Testnet signer is valid
✅ Bee signer is valid
✅ Chains are valid
✅ Balances are sufficient
Sending a message from bsctestnet to bee
Pending https://testnet.bscscan.com/tx/0x86279d9666d6ad9e6165ada1dea20d0f1a4a62a48a02abd99d5bf47355b6e114 (waiting 1 blocks for confirmation)
Sent transfer from sender (0x5159eA8501d3746bB07c20B5D0406bD12844D7ec) on bsctestnet to recipient (0x5159eA8501d3746bB07c20B5D0406bD12844D7ec) on bee.
Message ID: 0xa2f0034ee8abd34cd2f2226149a03553230c93d7b9c3590b96e9e1d23a994c7e
Explorer Link: https://explorer.hyperlane.xyz/message/0xa2f0034ee8abd34cd2f2226149a03553230c93d7b9c3590b96e9e1d23a994c7e
Message:
    id: "0xa2f0034ee8abd34cd2f2226149a03553230c93d7b9c3590b96e9e1d23a994c7e"
    message: "0x030000000200000061000000000000000000000000637189c5c4027259e98c9eea6\
      a393aef1f3a4bcc00000c74000000000000000000000000897e914031c27f679ed54910015453\
      cdbb9ae1620000000000000000000000005159ea8501d3746bb07c20b5d0406bd12844d7ec000\
      000000000000000000000000000000000000000000000002386f26fc10000"
    parsed:
      version: 3
      nonce: 2
      origin: 97
      sender: "0x000000000000000000000000637189c5c4027259e98c9eea6a393aef1f3a4bcc"
      destination: 3188
      recipient: "0x000000000000000000000000897e914031c27f679ed54910015453cdbb9ae162"
      body: "0x0000000000000000000000005159ea8501d3746bb07c20b5d0406bd12844d7ec000000\
        000000000000000000000000000000000000000000002386f26fc10000"
    
Body:
    recipient: "0x0000000000000000000000005159ea8501d3746bb07c20b5d0406bd12844d7ec"
    amount: 10000000000000000
    
Attempting self-relay of message...
Preparing to relay message 0xa2f0034ee8abd34cd2f2226149a03553230c93d7b9c3590b96e9e1d23a994c7e
Relaying message 0xa2f0034ee8abd34cd2f2226149a03553230c93d7b9c3590b96e9e1d23a994c7e
Pending https://scan.beechain.ai/tx/0x441d444ca5cbcf68ff3867aa5ba03de12fd7621eb9b56c893ab53743c0421bd8 (waiting 1 blocks for confirmation)
Transfer was self-relayed!
✅ Successfully sent messages for chains: bsctestnet ➡️ bee
```

#  前端bsc <==> monad 跨链请看当前vue项目代码即可
# 已部署到线上, 网址  https://hyperlane-crosschain.vercel.app/

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```


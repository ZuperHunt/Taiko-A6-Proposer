![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/18f1e7df-1599-4725-93ab-559ca6cfac42)

Penulis: [Luthfi](https://x.com/luthfi0x)

> [!NOTE]
> **APA ITU TAIKO?**\
> Singkatnya [Taiko](https://taiko.xyz) adalah the only zkEVM L2 Tipe 1 karena saampai saat ini gua belum tau ada another zkEVM Tipe 1 selain Taiko. Lalu. what makes Taiko special? Karena dia Type 1 (Ethereum-equivalent) sehingga dApps yang ada di Ethereum bisa diporting ke Taiko dengan perubahan yang minimum!

# Tutorial Taiko Node

## Dependencies

### Install `docker`

Set up repo `apt` Docker.
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install the Docker packages terbaru.
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Menjalankan Taiko Node

### Download Taiko

```
git clone https://github.com/taikoxyz/simple-taiko-node.git
```

### Setup Environment

copy folder sample environent dan buka file copyan itu
```
cd simple-taiko-node
cp .env.sample .env
nano .env
```

### Setup RPC Holesky Ethereum

Login ke [block.io](https://blockpi.io), pergi ke `dashboard`, create `+New API Key`.

![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/d8593358-06b6-4e02-9c27-1e5a56fa9377)

Pilih `Ethereum` > `Holesky` dan berikan nama `TaikoProposer`

![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/79a9523d-067b-49ab-a635-0be0c1200fac)

Klik API Key yang sudah kamu buat.

![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/fb087a53-26f9-4f4e-9516-0034acc1bca1)

Copy `https://...`ke variable `L1_ENDPOINT_HTTP` dan `wss://...` ke variable `L1_ENDPOINT_WS`ke dalam file `.env`.\
Sehingga file `.env`kamu menjadi seperti:
```
.......

############################### REQUIRED #####################################
# L1 Holesky RPC endpoints (you will need an RPC provider such as BlockPi, or run a full Holesky node yourself)
# If you are using a local Holesky L1 node, you can refer to it as "http://host.docker.internal:8545" and "ws://host.docker.internal:8546", which refer to the default ports in the .env for an eth-docker L1 node.
# However, you may need to add this host to docker-compose.yml. If that does not work, you can try the private local ip address (e.g. http://192.168.1.15:8545). You can find that with `ip addr show` or a similar command.
L1_ENDPOINT_HTTP=https://ethereum-holesky.blockpi.network/v1/rpc/......
L1_ENDPOINT_WS=wss://ethereum-holesky.blockpi.network/v1/ws/.......

.......
```

Simpan file tersebut `Control + X`.\
Terakhir, scroll ke bawah dan aktifkan `Archieve Node`.

![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/fdef8cfc-7f17-48cc-b3a0-a3165cc63384)

Start Taiko Node

```
docker compose up -d
```

### Verifikasi Node Taiko

Buka link [**`http://alamat_vps_kamu:3001/d/L2ExecutionEngine/l2-execution-engine-overview`**](http://localhost:3001/d/L2ExecutionEngine/l2-execution-engine-overview) di browser.\
Tunggu beberapa menit, jika node kamu berhasil jalan maka akan mulai tahap synchornize yang ditandai dengan grafik `Chain Head` yang meningkat.

![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/682cab46-6aa8-4483-8f09-ff86fae01728)

> [!NOTE]
> Saat ini Total Block Taiko Katla berjumah `329,394`. Ketika `Latest Header` kamu === dengan `Total Blocks` yang ada di explorer, itu artinya node kamu sudah 100% sync.

![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/79fbff94-d82b-476b-98f5-01e7ff4d2ba1)

### Cek error

Jika ada masalah pada node kamu, cek logs node.
```
docker compose logs -f
```

Pastikan hasilnya seperti:
```
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:14:49.377] L2 execution engine sync progress        progress="&{StartingBlock:0 CurrentBlock:17144 HighestBlock:329394 PulledStates:0 KnownStates:0 SyncedAccounts:0 SyncedAccountBytes:0 SyncedBytecodes:0 SyncedBytecodeBytes:0 SyncedStorage:0 SyncedStorageBytes:0 HealedTrienodes:0 HealedTrienodeBytes:0 HealedBytecodes:0 HealedBytecodeBytes:0 HealingTrienodes:0 HealingBytecode:0}" lastProgressedTime=2023-12-09T15:14:39+0000 timeout=1h40m0s
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:14:49.573] "✅ Block proven"                         blockID=1,523,934 hash=126a88..28780a prover=0x2909Db987AA74120a15f743197c58bE1B8D5e83b
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:14:52.390] Served taiko_headL1Origin                conn=172.18.0.6:42754 reqid=17 duration="136.284µs" err="not found"
simple-taiko-node-taiko_client_proposer-1        | INFO [12-09|15:14:52.632] L2 execution engine is syncing           progress="&{SyncProgress:0xc0005181b0 CurrentBlockID:+0 HighestBlockID:+329394 }"
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:14:52.833] Latest verified block header retrieved   hash=b4727d..179c92
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:14:52.834] Ignoring payload with missing parent     number=1,522,614 hash=b4727d..179c92 parent=77a90f..edb7ee
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:14:52.835] Forkchoice requested sync to new head    number=1,522,614 hash=b4727d..179c92 finalized=1,522,614
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:14:52.835] Served engine_forkchoiceUpdatedV2        conn=172.18.0.4:37944 reqid=55 duration="465.549µs" err="beacon syncer reorging"
simple-taiko-node-taiko_client_driver-1          | ERROR[12-09|15:14:52.835] Process new L1 blocks error              error="trigger beacon sync error: beacon syncer reorging"
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:14:55.942] Imported new chain segment               number=17282     hash=d7d307..e44b18 blocks=162  txs=2385 mgas=879.097 elapsed=8.041s    mgasps=109.320 age=2mo2w5d  dirty=0.00B
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:14:59.368] L2 execution engine sync progress        progress="&{StartingBlock:0 CurrentBlock:17357 HighestBlock:1522547 PulledStates:0 KnownStates:0 SyncedAccounts:0 SyncedAccountBytes:0 SyncedBytecodes:0 SyncedBytecodeBytes:0 SyncedStorage:0 SyncedStorageBytes:0 HealedTrienodes:0 HealedTrienodeBytes:0 HealedBytecodes:0 HealedBytecodeBytes:0 HealingTrienodes:0 HealingBytecode:0}" lastProgressedTime=2023-12-09T15:14:49+0000 timeout=1h40m0s
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:01.319] "✅ Block proven"                         blockID=1,523,919 hash=a59247..2b4443 prover=0xb2079986EBEb294b3Fa713e9C14B7717ED85873C
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:15:03.960] Imported new chain segment               number=17450     hash=e5e8a1..e983e2 blocks=168  txs=2501 mgas=956.802 elapsed=8.017s    mgasps=119.334 age=2mo2w5d  dirty=0.00B
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:15:04.637] Served taiko_headL1Origin                conn=172.18.0.6:42756 reqid=19 duration="209.105µs" err="not found"
simple-taiko-node-taiko_client_proposer-1        | INFO [12-09|15:15:04.878] L2 execution engine is syncing           progress="&{SyncProgress:0xc0000207e0 CurrentBlockID:+0 HighestBlockID:+329394 }"
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:05.016] Latest verified block header retrieved   hash=b4727d..179c92
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:15:05.017] Ignoring payload with missing parent     number=1,522,614 hash=b4727d..179c92 parent=77a90f..edb7ee
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:15:05.018] Forkchoice requested sync to new head    number=1,522,614 hash=b4727d..179c92 finalized=1,522,614
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:15:05.018] Served engine_forkchoiceUpdatedV2        conn=172.18.0.4:37944 reqid=57 duration="510.653µs" err="beacon syncer reorging"
simple-taiko-node-taiko_client_driver-1          | ERROR[12-09|15:15:05.019] Process new L1 blocks error              error="trigger beacon sync error: beacon syncer reorging"
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:09.367] L2 execution engine sync progress        progress="&{StartingBlock:0 CurrentBlock:17572 HighestBlock:329394 PulledStates:0 KnownStates:0 SyncedAccounts:0 SyncedAccountBytes:0 SyncedBytecodes:0 SyncedBytecodeBytes:0 SyncedStorage:0 SyncedStorageBytes:0 HealedTrienodes:0 HealedTrienodeBytes:0 HealedBytecodes:0 HealedBytecodeBytes:0 HealingTrienodes:0 HealingBytecode:0}" lastProgressedTime=2023-12-09T15:14:59+0000 timeout=1h40m0s
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:09.859] "📖 Protocol status"                      lastVerifiedBlockId=1,522,614 pendingBlocks=1475 availableSlots=401,724
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:15:11.961] Imported new chain segment               number=17628     hash=6538a2..fd0d6b blocks=178  txs=2495 mgas=970.631 elapsed=8.001s    mgasps=121.310 age=2mo2w5d  dirty=0.00B
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:13.169] "✅ Block proven"                         blockID=1,523,887 hash=fb0a60..b47a06 prover=0x2909Db987AA74120a15f743197c58bE1B8D5e83b
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:13.169] "✅ Block proven"                         blockID=1,523,896 hash=b91c43..800902 prover=0xD275E84eb1967f6DA2c475BbA1D312775ECEC21C
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:15:16.880] Served taiko_headL1Origin                conn=172.18.0.6:42754 reqid=21 duration="136.769µs" err="not found"
simple-taiko-node-taiko_client_proposer-1        | INFO [12-09|15:15:17.151] L2 execution engine is syncing           progress="&{SyncProgress:0xc000020900 CurrentBlockID:+0 HighestBlockID:+1524091}"
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:17.199] Latest verified block header retrieved   hash=b4727d..179c92
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:15:17.201] Ignoring payload with missing parent     number=1,522,614 hash=b4727d..179c92 parent=77a90f..edb7ee
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:15:17.202] Forkchoice requested sync to new head    number=1,522,614 hash=b4727d..179c92 finalized=1,522,614
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:15:17.203] Served engine_forkchoiceUpdatedV2        conn=172.18.0.4:37944 reqid=59 duration="287.011µs" err="beacon syncer reorging"
simple-taiko-node-taiko_client_driver-1          | ERROR[12-09|15:15:17.203] Process new L1 blocks error              error="trigger beacon sync error: beacon syncer reorging"
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:19.367] L2 execution engine sync progress        progress="&{StartingBlock:0 CurrentBlock:17795 HighestBlock:329394 PulledStates:0 KnownStates:0 SyncedAccounts:0 SyncedAccountBytes:0 SyncedBytecodes:0 SyncedBytecodeBytes:0 SyncedStorage:0 SyncedStorageBytes:0 HealedTrienodes:0 HealedTrienodeBytes:0 HealedBytecodes:0 HealedBytecodeBytes:0 HealingTrienodes:0 HealingBytecode:0}" lastProgressedTime=2023-12-09T15:15:09+0000 timeout=1h40m0s
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:15:19.996] Imported new chain segment               number=17810     hash=af9b06..92eaa2 blocks=182  txs=2976 mgas=1041.757 elapsed=8.034s    mgasps=129.654 age=2mo2w5d  dirty=0.00B
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:25.924] "✅ Block proven"                         blockID=1,523,933 hash=ec53a4..b7ba79 prover=0x2909Db987AA74120a15f743197c58bE1B8D5e83b
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:25.924] "✅ Block proven"                         blockID=1,524,029 hash=0c6f83..0001f0 prover=0x1715498570f67D423b6FEC5361815013Fa172FB0
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:25.924] "✅ Block proven"                         blockID=1,524,022 hash=859c8d..3e6f8e prover=0xfB924883f1A1D77537eA1fbFF126DA27bDfadeFf
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:25.924] "✅ Block proven"                         blockID=1,524,011 hash=99d63e..85a488 prover=0xF1F465C2Cae39Acb631142401144Aa34217fEB25
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:25.924] "✅ Block proven"                         blockID=1,524,003 hash=eb2280..5cd446 prover=0x7fDf03540091f7fCeE1A9B6D564C08d03440a819
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:25.925] "✅ Block proven"                         blockID=1,524,013 hash=3f9c96..2177f7 prover=0x875924aDC5dAa63e348BF15eAdaC7323C193e752
simple-taiko-node-l2_execution_engine-1          | INFO [12-09|15:15:28.003] Imported new chain segment               number=17989     hash=9765c5..64815f blocks=179  txs=2994 mgas=1054.887 elapsed=8.006s    mgasps=131.748 age=2mo2w5d  dirty=0.00B
simple-taiko-node-l2_execution_engine-1          | WARN [12-09|15:15:29.155] Served taiko_headL1Origin                conn=172.18.0.6:42756 reqid=23 duration="157.675µs" err="not found"
simple-taiko-node-taiko_client_driver-1          | INFO [12-09|15:15:29.367] L2 execution engine sync progress        progress="&{StartingBlock:0 CurrentBlock:18024 HighestBlock:329394 PulledStates:0 KnownStates:0 SyncedAccounts:0 SyncedAccountBytes:0 SyncedBytecodes:0 SyncedBytecodeBytes:0 SyncedStorage:0 SyncedStorageBytes:0 HealedTrienodes:0 HealedTrienodeBytes:0 HealedBytecodes:0 HealedBytecodeBytes:0 HealingTrienodes:0 HealingBytecode:0}" lastProgressedTime=2023-12-09T15:15:19+0000 timeout=1h40m0s
```
Logs kamu normal jika `CurrentBlock:xxxx` nilainya bertambah. Error `beacon syncer reorging` abaikan aje.

# Tutorial Menjalankan Propopser Taiko

## Deploying Smart Contract

> [!WARNING]
> **Syarat Menjadi Proposer**
> 1) Taiko node kamu sudah 100% synchronized
> 2) Kamu memiliki Holeksy ETH (Holesky ETH bisa kamu dapatkan melalui berbagai faucet)

## Stop Taiko Node

Kalau nodenya masih on, perlu di stop.
```
docker compose stop
docker compose down
```

## Setup Address Proposer

Buat address baru di Metamask. Kirim Holesky ETH ke wallet tersebut.

## Setup Environment

Buka file envrionment.
```
nano .env
```

Enable Proposer dengan mengubah `false` menjadi `true`.
```
....

ENABLE_PROPOSER=true

...
```

Set `Proposer Address` dengan mengganti `L1_PROPOSER_PRIVATE_KEY=` dengan private key address Taiko Node yang sudah kita buat sebelumnya. 
```
# A L1 account (with balance) private key who will send TaikoL1.proposeBlock transactions
L1_PROPOSER_PRIVATE_KEY=ABCDEFGHIJKAUTHORGANTENG
```

Set `Prover Endpoint` dengan membuka https://docs.taiko.xyz/resources/prover-marketplace,`copy` semua `Prover Endpoint` yang tersedia, dan `paste`.\
Simpan file environment `Control + X`.
```
# Comma-delineated list (no spaces) of prover endpoints proposer should query when attempting to propose a block
# If you keep this default value you must also enable a prover by setting ENABLE_PROVER=true
PROVER_ENDPOINTS=endpoint_1,endpoint2,endpoint3,......
```

## Menjalankan Proposer

Steady lads.. running the proposer 🤓
```
docker compose up -d
```

Verifikasi proposer, tunggu minimal beberapa menit dan pastikan hasilnya seperti:
```
docker compose logs -f taiko_client_proposer | egrep "Propose transactions succeeded"
```
Saat tutorial ini ditulis, hanya ada `1 prover` yang tersedia sehingga mendapatkan transaksi `Propose Block` rebutan.

Berikut ada ilustrasi jika kamu berhasil mempropose block:
```
simple-taiko-node-taiko_client_proposer-1  | INFO [19-02|16:19:01.874] "📝 Propose transactions succeeded"       txs=5
simple-taiko-node-taiko_client_proposer-1  | INFO [19-02|17:40:01.874] "📝 Propose transactions succeeded"       txs=10
simple-taiko-node-taiko_client_proposer-1  | INFO [19-02|20:23:01.874] "📝 Propose transactions succeeded"       txs=17
```
Jika ada logs `📝 Propose transactions succeeded`, selamat proposer kamu berjalan dengan sempurna dan kamu berhasil mempropose block.

## Cek Error

Jika selama 24 jam tidak ada block yang terpropose satu pun, cek error di `logs`.
```
docker compose logs -f taiko_client_proposer
```

## Cek Total Proposed Blocks

Buka [**`https://dashboard.dojonode.xyz/`**](https://dashboard.dojonode.xyz/) > Klik `Dojo Node` > Pilih `Proposer` > Klik `icon Satelite`> `Paste Address` proposer kamu.\
Berikut ada ilustrasi jika kamu berhasil mempropose block:

![image](https://github.com/ZuperHunt/Taiko-A6-Proposer/assets/80172679/9a66a7f1-905c-426c-b893-3a95c9f7e36f)

# Help

If you have any questions, find us on:\
[ZuperHunt's Discord server](https://discord.gg/ZuperHunt)\
[ZuperHunt's X(Twitter)](https://twitter.com/ZuperHunt)

# Acknowledgements

* [Smart Contract Quickstart](https://docs.fuel.network/guides/contract-quickstart/)
* [Gitpod-based “Virtual Private Server” Guidebook](https://luthfi0x.notion.site/Gitpod-based-Virtual-Private-Server-Guidebook-a82c45e276ea436986959e83d26b32f8#6f2f73ec3658433b86cf7405b1819d28)

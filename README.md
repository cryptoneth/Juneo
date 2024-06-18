# Juneo
```
sudo apt-get update && sudo apt-get upgrade -y
timedatectl
timedatectl set-ntp on 
```
```
sudo apt install git docker.io docker-compose -y
```

```
cd && git clone https://github.com/Juneo-io/juneogo-binaries
chmod +x ~/juneogo-binaries/juneogo
chmod +x ~/juneogo-binaries/plugins/jevm
chmod +x ~/juneogo-binaries/plugins/srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e
mkdir -p ~/.juneogo/plugins && \
mv ~/juneogo-binaries/plugins/jevm ~/.juneogo/plugins && \
mv ~/juneogo-binaries/plugins/srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e ~/.juneogo/plugins
```

```
tee /etc/systemd/system/juneod.service > /dev/null <<EOF 
[Unit]
Description=JUNEO Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/root/juneogo-binaries/juneogo --config-file="$HOME/juneogo-binaries/config.json"
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

```

```
systemctl daemon-reload
systemctl enable juneod
systemctl start juneod
systemctl status juneod
```


Wait for synchronization.
با کامند زیر باید چک کنیم تا نود سینک شه

```
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.isBootstrapped",
    "params": {
        "chain":"JUNE"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

خروجی باید همچین چیزی باشه :
{“jsonrpc”:”2.0",”result”:{“isBootstrapped”:true},”id”:1}

برای چک کردن لاگ نود هم کامند زیر رو بزنید
```
journalctl -u juneod -f -o cat
```



بعد سینک شدن
```
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
}' -H 'content-type:application/json' 127.0.0.1:9650/ext/info

```
بزنید و اطلاعاتی که میده رو ذخیره کنید

Copy the value for the nodeID, publicKey, and proofOfPossession from the response.

Log into mcnwallet.io.

Go to Juneo Discord > fucet.
Create a ticket to receive JUNE token for self-staking.

Use JUNE-Chain wallet:
Go back to https://mcnwallet.io/stake and open Validate window.
Fill Node Id, BLS Key and BLS Signature which You’ve previously saved.
Add at least 1 JUNE token and set Validation Period for at least 30 days.


```
curl -H 'Content-Type: application/json' --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health",
    "params": {
        "tags": ["11111111111111111111111111111111LpoYY"]
    }
}' 'http://127.0.0.1:9650/ext/health' 
```


Create a Supernet

```
sudo apt-get remove nodejs
sudo apt-get purge nodejs
sudo apt-get autoremove
sudo rm /etc/apt/keyrings/nodesource.gpg
sudo rm /etc/apt/sources.list.d/nodesource.list
NODE_MAJOR=18
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_${NODE_MAJOR}.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install -y nodejs
node --version
npm install -g typescript && \
npm install -g ts-node && \
tsc -v && \
ts-node -v
```

۱. پروژه را با این دستور کلون (clone) کنید:
```
git clone https://github.com/Juneo-io/juneojs-examples
```
۲. به پوشه juneojs-examples که ساخته شده  بروید و نصب را انجام دهید:
```

cd juneojs-examples && npm install
```
۳. یک فایل جدید به نام .env  از روی فایل نمونه .env.example  ایجاد کنید:
```

cp .env.example .env
```
۴. فایل .env را برای ویرایش باز کنید:

```
nano .env
```

اینجا کلمات بازیابیتونو بنویسید 

Ctrl+C
Y
Enter

سیو کنید و خارج شید

Create Supernet:
```
npx ts-node ./src/supernet/createSupernet.ts
```
ایدی سوپر نت رو یادداشت کنید
```
nano ./src/supernet/addSupernetValidator.ts

```
مقادیر زیر رو جایگذاری کنید
const nodeId: string = 'YOUR_nodeID'
const supernetId: string = 'YOUR_supernetId'
const durationInDays: number = 20 // number of days you will validate your Supernet


Ctrl+C
Y
Enter

سیو کنید و خارج شید

```
npx ts-node ./src/supernet/addSupernetValidator.ts
```

```
cd && cd juneogo-binaries

nano config.json

```

{  
 "track-supernets":"ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP"  
}  

جای ادرس ایدی سوپر نت خودتون رو بذارید

Ctrl+C
Y
Enter

سیو کنید و خارج شید
```
systemctl restart juneod

```

```
nano ./juneojs-examples/src/supernet/createChain.ts
```
مقادیر زیر رو جایگذاری کنید

const supernetId: string = '...' // your supernet id

const chainName: string = 'Chain Name'

const chainId: number = 330333 // For the Socotra Testnet, please use a random number from 200,000 - 299,999

const genesisMintAddress: string = '0x44542FD7C3F096aE54Cc07833b1C0Dcf68B7790C' // your wallet address here


Ctrl+C
Y
Enter

```
cd juneojs-examples && npx ts-node ./src/supernet/createChain.ts
```
خروجی باید شبیه زیر باشه . سیو کنید یه جایی

Created chain with id: 2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D


جای Chain ID تو هر دو کد زیر خروجی رو جایگذاری کنید

```
cd && mkdir -p $HOME/.juneogo/configs/chains/<CHAIN_ID>
```

```
nano $HOME/.juneogo/configs/chains/<CHAIN_ID>/config.json
```
```
{
 "pruning-enabled": false,
   "eth-apis": ["public-eth","public-eth-filter","net","web3","internal-public-eth","internal-public-blockchain","internal-public-transaction-pool","internal-public-debug","debug-tracer"]
}

```


Ctrl+C
Y
Enter


سیو کنید و خارج شید

```
nano ./juneogo-binaries/config.json

```
جایگذاری کنید . حواستون باشه که یه کاما , به اخر لاین قبل اضافه کنید

 "http-host":"XXX.XXX.XXX.XXX"

6) Save and Exit:

Ctrl+C
Y
Enter

سیو کنید و خارج شید
```
systemctl restart juneod
```

تو دیسکورد یه تیکت باز کنید
Supernet
- Name:
- Description:
- id:

Blockchain
- Name :
- Description :
- Currency: (in ALL-CAPS format)
- Decimals:
- Host: (http://xxx.xxx.xxx.xxx:9650 or with domain name)
- RPC: (http://xxx.xxx.xxx.xxx:9650/ext/bc/id/rpc)
- Logo (min 273x273 or greater. In .png format. The width:height ratio should be 1:1)
- id:



مشخصات رو جایگذاری کنید و بفرستید

مثلا :

Supernet
- Name: Supernet ABC
- Description: This is a supernet created by
- id: ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP

Blockchain
- Name: Chain ABC
- Description: This is a Blockchain developed by
- Currency: ABC22
- Decimals: 18
- Host: http://XXX.XXX.XXX.XXX:9650 
- rpc: http://XXX.XXX.XXX.XXX:9650/ext/bc/2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D/rpc
- Logo: [your_logo_goes_here]
- id: 2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D


چک کردن سلامت نود :

```
curl -H 'Content-Type: application/json' --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health",
    "params": {
        "tags": ["11111111111111111111111111111111LpoYY"]
    }
}' 'http://IP:9650/ext/health' 
```

جای IP ایپی سرور رو بذارید




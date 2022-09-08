# Hardhat

---

[참고 문서](https://hardhat.org/docs)

[Alchemy](https://dashboard.alchemy.com/) -> 로그인 -> Create App(체인 및 네트워크 선택) -> VIEW KEY(API KEY, HTTPS)가져오기

---

SetData : 0x557F595048F93989e06e0bE8501e76f316ABF0Ef

JolamanToken : 0xE87D2696d64f14e4818Afbe96e3ad9c37e6F215B

randomJolaman : 0xaA6fFE6349D920a43e172B4DeDD51153326E23C7

saleJolaman : 0x4e4E2CF7Fe65a557C3150c3E13Aa267fec0AB084

Stake system : 0x76aa0A29A371142b9908016e93965e0Ed3001c21



TokenID : 0x7EF2e0048f5bAeDe046f6BF797943daF4ED8CB47

```bash
# .env 파일
# 연결한 주소에서 가져오기
OPTIMISM_GOERLI_PRIVATE_KEY=56da9b1c3f87870b3b44cbfb357304c8ef736850c24c5cb6829ff7c58abf126a

# Optimism Goerli API KEY for Alchemy
ALCHEMY_API_KEY=XKWU3wJUuIyv-7_pJYIxwjh3m4z5Arfg

# URL to access Optimism Goerli (if not using Alchemy)
OPTIMISM_GOERLI_URL=https://opt-goerli.g.alchemy.com/v2/XKWU3wJUuIyv-7_pJYIxwjh3m4z5Arfg
```

---

- 하드햇은 프로젝트 단위로 설치

```shell
cd 프로젝트
npm init -y
npm install --save-dev hardhat
npx hardhat

// 컴파일
npx hardhat compile
// 베포
npx hardhat run scripts/deploy.js
```

- 플러그인 설치

``` shell
npm install --save-dev @nomicfoundation/hardhat-toolbox

npm install --save-dev @nomiclabs/hardhat-waffle 'ethereum-waffle@^3.0.0' @nomiclabs/hardhat-ethers 'ethers@^5.0.0'
-require("@nomiclabs/hardhat-waffle");

npm install --save-dev @nomiclabs/hardhat-ethers 'ethers@^5.0.0'
- require("@nomiclabs/hardhat-ethers");

npm i @openzeppelin/hardhat-upgrades
- require('@openzeppelin/hardhat-upgrades');

npm install @openzeppelin/contracts-upgradeable

npm install @openzeppelin/contracts

npm install dotenv
```

- 베포

``` bash
// 하드햇에서 주는 로컬 네트워크 가나슈 같은거
npx hardhat node
npx hardhat run --network localhost ./scripts/베포할 컨트랙트 명

// 원격 네트워크
npx hardhat run --network optimism-goerli ./scripts/베포할 컨트랙트 명
```

- 컨트랙트 콘솔로 실행

``` bash
npx hardhat console --network optimism-goerli

const a = await ethers.getContractFactory("베포할 컨트랙트 명")
const ssu = await a.attach("베포한 컨트랙트 주소")
ssu.address
(await ssu.함수(매개변수)).toString()
```

- Goerli
  [참고 페이지](https://hardhat.org/tutorial/deploying-to-a-live-network)

``` js
require("@nomicfoundation/hardhat-toolbox");
require("@nomiclabs/hardhat-waffle");
require("@openzeppelin/hardhat-upgrades");
require("@nomiclabs/hardhat-ethers");

// This is a sample Hardhat task. To learn how to create your own go to
// https://hardhat.org/guides/create-task.html
task("accounts", "Prints the list of accounts", async () => {
  const accounts = await hre.ethers.getSigners();

  for (const account of accounts) {
    console.log(account.address);
  }
});

// Go to https://www.alchemyapi.io, sign up, create
// a new App in its dashboard, and replace "KEY" with its key
const ALCHEMY_API_KEY = "1k-UYT_uG1tp_CM3LHXrNllDw4Cr6KuX";

// Replace this private key with your Goerli account private key
// To export your private key from Metamask, open Metamask and
// go to Account Details > Export Private Key
// Beware: NEVER put real Ether into testing accounts
const GOERLI_PRIVATE_KEY =
  "56da9b1c3f87870b3b44cbfb357304c8ef736850c24c5cb6829ff7c58abf126a";

module.exports = {
  solidity: "0.8.9",
  networks: {
    goerli: {
      url: `https://eth-goerli.alchemyapi.io/v2/${ALCHEMY_API_KEY}`,
      accounts: [GOERLI_PRIVATE_KEY],
    },
  },
};

```

- Optimism Goerli
  [참고 페이지](https://github.com/ethereum-optimism/optimism-tutorial/tree/main/getting-started)

``` js
require("@nomicfoundation/hardhat-toolbox");
require("@nomiclabs/hardhat-waffle");
require("@openzeppelin/hardhat-upgrades");
require("@nomiclabs/hardhat-ethers");
require("dotenv").config();

task("accounts", "Prints the list of accounts", async () => {
  const accounts = await hre.ethers.getSigners();

  for (const account of accounts) {
    console.log(account.address);
  }
});

const optimismGoerliUrl = process.env.ALCHEMY_API_KEY
  ? `https://opt-goerli.g.alchemy.com/v2/${process.env.ALCHEMY_API_KEY}`
  : process.env.OPTIMISM_GOERLI_URL;

const PRIVATE_KEY = process.env.OPTIMISM_GOERLI_PRIVATE_KEY;

module.exports = {
  solidity: "0.8.9",
  networks: {
    "optimism-goerli": {
      url: optimismGoerliUrl,
      accounts: PRIVATE_KEY,
    },
  },
};


```


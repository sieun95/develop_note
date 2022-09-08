# Truffle 

---

### Truffle 세팅

[참고 문서](https://github.com/trufflesuite/truffle)

``` shell
npm install -g truffle

cd 작업폴더

truffle init
```



``` javascript
/* 
기본 sol, js 파일 생성
sol파일은 contracts 폴더 js파일은 migrations 폴더
배포한 컨트랙트 다시 배포 안함 
*/

// sol파일 - 컨트랙트 작성

// SPDX-License-Identifier: MIT

pragma solidity >=0.4.22 <0.9.0;

contract Migrations {
    address public owner = msg.sender;
    uint public last_completed_migration;

    modifier restricted() {
        require(msg.sender == owner, "This function is restricted to the contract"); _;
    }

    function setCompleted(uint completed) public restricted {
        last_completed_migration = completed;
    }
}

// 1_inial_migration.js  - 베포 
const Migrations = artifacts.require("Migrations");

module.exports = function (deployer) {
  deployer.deploy(Migrations);
};
```



``` shell
cd 작업 폴더
truffle migrate
```

---

#### abi파일 경로 설정

[참고 링크](https://trufflesuite.com/docs/truffle/reference/configuration/)

- Contracts_build_directory 부분 확인

​		Truffle-config.js파일에서 경로 시작
​		ex) contracts_build_directory: "./frontend/src/abis"

``` js
module.exports = {
  contracts_build_directory: "./frontend/src/abis",
  networks: {
    development: {
      host: "127.0.0.1", // Localhost (default: none)
      port: 7545, // Standard Ethereum port (default: none)
      network_id: "*", // Any network (default: none)
    },
```



---

#### Test

``` shell
cd 작업폴더
truffle test
```



[Mocha 공식 문서](https://mochajs.org/)

[Chai 공식 문서](https://www.chaijs.com/api/)

###### truffle은 Mocah, Chai를 참고하여 test한다

######  .only, .skip 한 파일, 함수만 테스트한다(Mocha 참고)

- [Chai - assert](https://www.chaijs.com/api/assert/)

```js
// path test/...

// 테스트할 컨트랙트 가져오기
const Simple = artifacts.require("Simple");

// 테스트 파일 명
contract.only("test1", (accounts) => {
    
  it("Should not have zero address", async () => {
    const simpleInstance = await Simple.deployed();
    await assert.notEqual(simpleInstance.address, 0x0);
  });
    
  it.skip("Should not have 5", async () => {
    const simpleInstance = await Simple.deployed();
    const result = await simpleInstance.return5();
    await assert.equal(result, 5);
  });

  it("Should not have 55", async () => {
    const simpleInstance = await Simple.deployed();
    const result = await simpleInstance.returnParameter(55);
    await assert.equal(result, 55);
  });
});
```



- [Chai - expect](https://www.chaijs.com/api/bdd/)

``` js
// path test/...

// 테스트할 컨트랙트 가져오기
const Simple = artifacts.require("Simple");

contract.only("test2", (accounts) => {
  it("Should not have zero address", async () => {
    const simpleInstance = await Simple.deployed();
    await expect(simpleInstance.address).to.not.equal(0x0);
  });
});
```



###### BigNumber

[참고 문서](https://www.npmjs.com/package/chai-bn)

JS는 2 ** 53 -1 을 넘는 수는 계산을 못한다 하지만 블록체인에서는 2 ** 256 -1 까지 사용하기 때문에 문제가 생길 수 있다



``` sh
npm i chai chai-bn
```



```js
const Simple = artifacts.require("Simple");
const chai = require("chai");
// bn.js는 web3에서 지원한다
const BN = web3.utils.BN;
chai.use(require("chai-bn")(BN));
const { expect } = chai;

contract.only("test2", (accounts) => {
  it("Should not have zero address", async () => {
    const simpleInstance = await Simple.deployed();
    await expect(simpleInstance.address).to.not.equal(0x0);
  });

  it("Should not have 5", async () => {
    const simpleInstance = await Simple.deployed();
    const result = await simpleInstance.return5();
    // BigNumber는 오브젝트 형식이다
    console.log(new BN(5));
    await expect(result).to.be.bignumber.equal(new BN(5));
  });
});
```

###### BigNumber 산술연산자

[참고 문서](https://github.com/indutny/bn.js/)

``` js
 it("Should not have 5", async () => {
    const simpleInstance = await Simple.deployed();
    const result = await simpleInstance.return5();

    console.log(new BN(5));
    await expect(result).to.be.bignumber.equal(new BN(5));

    const addNumber = new BN(4).add(new BN(1)); // 4 + 1 = 5
    await expect(result).to.be.bignumber.equal(addNumber);

    const subNumber = new BN(6).sub(new BN(1)); // 6 - 1 = 5
    await expect(result).to.be.bignumber.equal(subNumber);

    const mulNumber = new BN(5).mul(new BN(1)); // 5 * 1 = 5
    await expect(result).to.be.bignumber.equal(mulNumber);

    const divNumber = new BN(5).div(new BN(1)); // 5 / 1 = 5
    await expect(result).to.be.bignumber.equal(divNumber);
  });
```

---

#### sendTransaction

```js
const Simple2 = artifacts.require("Simple2");
const chai = require("chai");
// bn.js는 web3에서 지원한다
const BN = web3.utils.BN;
chai.use(require("chai-bn")(BN));
const { expect } = chai;

contract.only("test3", async (accounts) => {
  it("Should not have zero address", async () => {
    const simpleInstance = await Simple2.deployed();
    await expect(simpleInstance.address).to.not.equal(0x0);
  });

  it("Should have 1 ether", async () => {
    const simpleInstance = await Simple2.deployed();
    console.log(accounts);
    await web3.eth.sendTransaction({
      from: accounts[0],
      to: simpleInstance.address,
      value: web3.utils.toWei("1", "ether"),
    });
    let balance = await web3.eth.getBalance(simpleInstance.address);
    expect(balance).to.be.a.bignumber.equal(
      new BN(web3.utils.toWei("1", "ether"))
    );
  });
});

```

---

#### event

[참고 문서](https://web3js.readthedocs.io/en/v1.7.5/web3-eth-contract.html#getpastevents)



``` solidity
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0;

contract Simple2 {
	// indexed를 선언한 부분 즉 from을 이용하여 filter 기능을 사용할 수 있다
    event Receive(address indexed from, uint256 amount);
    event ReturnNumber(string description);

    receive() external payable {
        emit Receive(msg.sender, msg.value);
    }

    function return99() public {
        emit ReturnNumber("This is return99");
    }
}

```



``` js
// myContract.getPastEvents(event[, options][, callback])

 it("Should have 1 ether", async () => {
    const simpleInstance = await Simple2.deployed();
    console.log(accounts);

    await web3.eth.sendTransaction({
      from: accounts[0],
      to: simpleInstance.address,
      value: web3.utils.toWei("1", "ether"),
    });

    await web3.eth.sendTransaction({
      from: accounts[1],
      to: simpleInstance.address,
      value: web3.utils.toWei("2", "ether"),
    });

    await web3.eth.sendTransaction({
      from: accounts[2],
      to: simpleInstance.address,
      value: web3.utils.toWei("3", "ether"),
    });

    await web3.eth.sendTransaction({
      from: accounts[2],
      to: simpleInstance.address,
      value: web3.utils.toWei("4", "ether"),
    });

   let info = await simpleInstance.getPastEvents("Receive", {
       // filter : indexed를 선언한 부분으로 값을 찾을 수 있다
      filter: {from : accounmts[2]},
      fromBlock: 0,
      toBlock: "latest",
    });
```



- 이벤트에서 함수 데이터 추출하기

``` js
   let info = await simpleInstance.return99();

    console.log(info.logs[0].event);
    console.log(info.logs[0].args);
```



[Chai-as-promised](https://www.npmjs.com/package/chai-as-promised)

``` sh
npm i chai-as-promised
```



``` js
return promise.should.be.fulfilled; // 테스트가 성공하면 true
return promise.should.eventually.deep.equal("foo");	// async await 을 사용 안 해도 된다
return promise.should.become("foo"); // same as `.eventually.deep.equal`
return promise.should.be.rejected;	// 테스트가 성공하면 fales
return promise.should.be.rejectedWith(Error); // other variants of Chai's `throw` assertion work too.
```




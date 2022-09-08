# Ethers.js

[공식 문서](https://docs.ethers.io/v5/)

---

- 리액트에서 컨트랙트 접근 방법

``` js
// contract 접근 방법
import { ethers } from "ethers";
import setDataAbi from "../../contractABI/setData.json";
import { DATA_CONTRACT_ADDRESS } from "../../config";
import setDataAbi from "../../contractABI/setData.json";


const joinContract = async () => {
    // 메타마스크 연결
    const accounts = await window.ethereum.request({
      method: "eth_requestAccounts",
    });
    //  transaction 가능하게 하는 부분
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const signer = provider.getSigner();
    
    // 메타마스크에 연결한 주소
     const address = accounts[0];
    // 트랜잭션
      const tx = signer.sendTransaction({
        from: address, 
        to: "DATA_CONTRACT_ADDRESS", // 컨트랙트 주소
        value: ethers.utils.parseEther("0.2"),
        gasPrice: "3000000",
        data: contract._owner(), // 받아오고 싶은 데이터 함수도 가능
      });
    
    // 사용할 contract 설정
    const contract = new ethers.Contract(
      "DATA_CONTRACT_ADDRESS",
      setDataAbi.abi,
      signer
    );
    
// 컨트랙트 내부에 함수 실행
    try {
      const response = await contract.getOwner();
      console.log("response: ", response);
    } catch (err) {
      console.log("error: ", err);
    }
  };
```

 ``` js
 ```


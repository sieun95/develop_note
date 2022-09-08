# ERC-20

#### 간단한 토큰 만들어보기

```solidity
contract myToken {

    event _transfer(address from, address to, uint amount);

    string private tokenName;
    string private tokenSymbol;
    uint private tokenTotalSupply;
    uint private tokenDecimal;

    mapping(address => uint) private balances;

    constructor(string memory _name, string memory _symbol, uint _totalSupply, uint _decimal) {
        tokenName = _name;
        tokenSymbol = _symbol;
        tokenDecimal = _decimal;
        mint(msg.sender, _totalSupply);

        balances[msg.sender] = _totalSupply;
    }
    
    function name() public view returns(string memory) {
        return tokenName;
    }

    function symbol() public view returns(string memory) {
        return tokenSymbol;
    }

    function totalSupply() public view returns(uint) {
        return tokenTotalSupply;
    }

    function decimal() public view returns(uint) {
        return tokenDecimal;
    }

    function balanceOf(address _addr) public view returns(uint) {
        return balances[_addr];
    }

    function mint(address _addr, uint amount) internal virtual {
        balances[_addr] += amount;
        tokenTotalSupply += amount;
    }

    function transfer(address _to, uint _amount) public {
        require(balances[msg.sender] >= _amount, "Too much to send tokens");
        balances[msg.sender] -= _amount;
        balances[_to] += _amount;

        emit _transfer(msg.sender, _to, _amount);
    }
}
```

#### Front랑 Contract 연동

```js
// 베포한 컨트랙트 ABI, 주소
const contract = new Contract(jsonInterface, address);
```


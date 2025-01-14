# DIOCoin - ERC20 Token

## 📌 About the Project
DIOCoin (𝐓𝐢𝐞) is an ERC-20 token implemented in Solidity for educational purposes and blockchain development projects.

## 🚀 Technologies Used
- **Solidity** (v0.8.0)
- **Remix IDE** (for compilation and deployment)
- **MetaMask** (for blockchain interaction)
- **Ethereum Testnet (Goerli/Sepolia)**

## 📂 Project Structure
```
/
├── DioCoin.sol        # Smart contract in Solidity
├── README.md          # Project documentation
├── LICENSE            # MIT License
├── .gitignore         # Git ignore file
├── .github/
│   ├── workflows/
│   │   ├── deploy.yml  # GitHub Actions for deployment
│   ├── CODEOWNERS      # Define code reviewers
│   ├── PULL_REQUEST_TEMPLATE.md  # Template for PRs
│   ├── ISSUE_TEMPLATE.md  # Template for issues
```

## ⚙️ Installation and Deployment

### 🔹 1. Clone the Repository
```bash
git clone https://github.com/your-username/DIOCoin.git
cd DIOCoin
```

### 🔹 2. Compile in Remix
1. Go to [Remix Ethereum IDE](https://remix.ethereum.org/).
2. Upload the `DioCoin.sol` file.
3. Go to the **Solidity Compiler** tab and click **Compile DioCoin.sol**.

### 🔹 3. Deploy on Ethereum Testnet
1. **Connect your MetaMask wallet**.
2. Go to the **Deploy & Run Transactions** tab.
3. In **Environment**, select **Injected Provider - MetaMask**.
4. Click **Deploy**.

## 📌 Implemented Functions

### 🔹 Main Methods
```solidity
function totalSupply() public view returns (uint256);
function balanceOf(address account) public view returns (uint256);
function transfer(address recipient, uint256 amount) public returns (bool);
function approve(address spender, uint256 amount) public returns (bool);
function transferFrom(address sender, address recipient, uint256 amount) public returns (bool);
```

### 🔹 Events
```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
event Approval(address indexed owner, address indexed spender, uint256 value);
```

## 🔍 Code Explanation

### 1. **Contract Declaration**
```solidity
pragma solidity ^0.8.0;
```
- Defines the Solidity version (0.8.0 or later) for compatibility.

### 2. **ERC-20 Standard Implementation**
The contract follows the ERC-20 interface, implementing essential functions like:
- `totalSupply()`: Retrieves total token supply.
- `balanceOf()`: Fetches balance of a given address.
- `transfer()`: Moves tokens between accounts.
- `approve()`: Allows another account to spend tokens.
- `transferFrom()`: Transfers tokens based on an approved allowance.

### 3. **Token Storage Variables**
```solidity
string public constant name = "DIO Coin";
string public constant symbol = "DIO";
uint8 public constant decimals = 18;
uint256 private totalSupply_ = 10 ether;
```
- Defines token name, symbol, and decimal places.
- Sets an initial total supply of `10 ether` (10 * 10^18 tokens).

### 4. **Mappings for Balance Tracking**
```solidity
mapping (address => uint256) private balances;
mapping (address => mapping(address => uint256)) private allowed;
```
- `balances`: Stores the balance of each user.
- `allowed`: Tracks allowances for delegated transfers.

### 5. **Transfer Function**
```solidity
function transfer(address receiver, uint256 numTokens) public override returns (bool) {
    require(numTokens <= balances[msg.sender], "Insufficient balance");
    
    balances[msg.sender] -= numTokens;
    balances[receiver] += numTokens;

    emit Transfer(msg.sender, receiver, numTokens);
    return true;
}
```
- Ensures the sender has enough tokens before transferring.
- Updates balances and emits a `Transfer` event.

### 6. **Approval for Delegated Transfers**
```solidity
function approve(address delegate, uint256 numTokens) public override returns (bool) {
    allowed[msg.sender][delegate] = numTokens;
    emit Approval(msg.sender, delegate, numTokens);
    return true;
}
```
- Grants a spender permission to use tokens.
- Emits an `Approval` event.

### 7. **Delegated Transfer (`transferFrom`)**
```solidity
function transferFrom(address owner, address buyer, uint256 numTokens) public override returns (bool) {
    require(numTokens <= balances[owner], "Owner has insufficient balance");
    require(numTokens <= allowed[owner][msg.sender], "Allowance exceeded");

    balances[owner] -= numTokens;
    allowed[owner][msg.sender] -= numTokens;
    balances[buyer] += numTokens;

    emit Transfer(owner, buyer, numTokens);
    return true;
}
```
- Allows a spender to transfer tokens based on allowance.
- Ensures sufficient balance and allowance before proceeding.

## 🚀 GitHub Configuration
### 🔹 .gitignore
```
/node_modules/
/build/
.env
.DS_Store
```

### 🔹 GitHub Actions (Deploy to Ethereum Testnet)
```yaml
name: Deploy Contract
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Install dependencies
        run: npm install
      - name: Compile Contract
        run: npx hardhat compile
      - name: Deploy Contract
        run: npx hardhat run scripts/deploy.js --network goerli
```

## 🛠️ How to Contribute
1. **Fork** the project.
2. Create a **new branch**:
   ```bash
   git checkout -b my-feature
   ```
3. Make your changes and **commit**:
   ```bash
   git commit -m "Adding new functionality"
   ```
4. Push to the remote repository:
   ```bash
   git push origin my-feature
   ```
5. Open a **Pull Request** for review.

## 📄 License
This project is licensed under the **MIT License**. Feel free to use and modify it.



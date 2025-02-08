# Launching the Wrapped Token Bridge Contracts on Remix

## 1. Getting Started in Remix

- Open your browser and navigate to [Remix IDE](https://remix.ethereum.org).
- In the left sidebar, click on the "File Explorers" tab to create new files.
- Create four new Solidity files named:
  - `TokenLocker.sol`
  - `WrappedToken.sol`
  - `BridgeOracle.sol`
  - `IZkBridge.sol`
- Copy and paste the complete code for each contract (as provided in the final deployment code above) into the corresponding file.

## 2. Configuring the Compiler

- Click on the **Solidity Compiler** tab in Remix.
- Select the appropriate compiler version (for example, v0.8.17 or later) to match the pragma in the code.
- Click the **Compile** button for each file or compile all contracts together.

## 3. Setting Up the Deployment Environment

- In the **Deploy & Run Transactions** tab, choose the **Environment**.
  - For testing on a local network, select **JavaScript VM**.
  - For deploying on a test network or mainnet, select **Injected Web3** and connect your MetaMask wallet.
- Ensure your wallet is connected to the appropriate network (for instance, a test network if you are not deploying on mainnet).

## 4. Deploying the Contracts

### Deploying TokenLocker.sol

- In the **Deploy & Run Transactions** tab, select the `TokenLocker` contract from the dropdown list.
- **Constructor Requirement:** The constructor requires the address of an ERC20 token.
  - For testing purposes, you can deploy a simple ERC20 contract (or use a predeployed ERC20 on your chosen network) and copy its address.
- Enter the token address in the input field provided.
- Click **Deploy**.
- Once deployed, copy the deployed TokenLocker contract address for later use.

### Deploying WrappedToken.sol

- Next, select the `WrappedToken` contract from the dropdown.
- **Constructor Requirement:** The constructor requires the address of the deployed TokenLocker contract.
- Paste the TokenLocker address you obtained from the previous deployment.
- Click **Deploy**.
- Save the WrappedToken deployed address for configuring roles and other interactions.

### Deploying BridgeOracle.sol

- Now, select the `BridgeOracle` contract.
- **Constructor Requirements:** This contract requires two parameters: the address of the TokenLocker and the address of the WrappedToken.
- Paste in the respective addresses.
- Click **Deploy**.
- The BridgeOracle contract will now be available for invoking the minting process.

## 5. Post-Deployment Configuration

- **Granting Roles:**
  - After deploying, you need to grant the `MINTER_ROLE` in the WrappedToken contract to the BridgeOracle contract so that it can mint wrapped tokens.
  - In the **Deployed Contracts** list, select the WrappedToken instance.
  - Call the appropriate function (for example, if a custom role management function is provided or by using Remix's **transact** buttons) to grant the MINTER_ROLE to the BridgeOracle address.
- **Configuring Fees and zkBridge (Optional):**
  - Use the `setFeePercent` and `setFeeReceiver` functions on the WrappedToken contract to configure fees.
  - If you need the zkBridge functionality, call `setZkBridgeMode(true)` on the WrappedToken contract.

## 6. Interacting with the Contracts

- **Locking Tokens:**
  - In the TokenLocker contract, call the `lockTokens(amount)` function.
  - Ensure you have approved the TokenLocker contract to transfer your ERC20 tokens by calling the ERC20 token's `approve()` function.
  - After locking, a `TokenLocked` event will be emitted.

- **Minting Wrapped Tokens:**
  - Once tokens are locked, simulate or trigger the bridging process.
  - In the BridgeOracle contract, call `processBridge(user, amount)` to mint the corresponding wrapped tokens to a given user.

- **Emergency and Administrative Actions:**
  - Test pausing by calling `pause()` on any contract as an admin.
  - When paused, an admin can call `emergencyWithdraw()` from the TokenLocker contract if needed.
  - Unpause using the `unpause()` function on the appropriate contracts.

## 7. Testing and Verifying

- Check the **Logs** in Remix (from the Terminal) to monitor event outputs and transaction statuses.
- Interact with each contract methodically, ensuring that event logs—for example, `TokenLocked`, `TokenMinted`, and `BridgeProcessed`—appear as expected.
- Use Remix's built-in tools to simulate different scenarios such as pausing/unpausing and role-based function calls.

## Summary

This guide walks you through the Remix IDE process for compiling, deploying, configuring, and testing the wrapped token bridge contracts. By following these steps, you'll establish a secure and functional bridge between the ERC20 tokens on Chain 138 and the wrapped tokens on Polygon.

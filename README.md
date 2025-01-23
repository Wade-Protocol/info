<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Trust Protocol (An onchain trust primitive)](#trust-protocol-an-onchain-trust-primitive)
    - [**Welcome to Trust Protocol**](#welcome-to-trust-protocol)
  - [**Problem Statement**](#problem-statement)
  - [**Our Solution**](#our-solution)
    - [**On-Chain Trust Bonds**](#on-chain-trust-bonds)
    - [Challenges:](#challenges)
    - [**New Features**](#new-features)
    - [**Reputation Score**](#reputation-score)
      - [**Parameters for Reputation Calculation**:](#parameters-for-reputation-calculation)
    - [**Community Reward Pool**](#community-reward-pool)
      - [Sources of the Reward Pool:](#sources-of-the-reward-pool)
    - [Distribution:](#distribution)
  - [Architecture](#architecture)
    - [**How It Works**](#how-it-works)
    - [**Sequence Diagram**](#sequence-diagram)
    - [**Technical Architecture Overview**](#technical-architecture-overview)
    - [**Smart Contracts Architecture**](#smart-contracts-architecture)
    - [**Contracts Class Diagram**](#contracts-class-diagram)
  - [**Use Cases**](#use-cases)
  - [**Join Us**](#join-us)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Trust Protocol (An onchain trust primitive)

### **Welcome to Trust Protocol**
---
In the real world, trust has foundation in primitives like survival,social bonds, bloodlines etc. These trust primitives transform depending on time and place, the cavemen had a primitive of survival in order to form their trust.We might not be cavemen anymore, but humans crave connection and security, even in the digital ecosystems of today.
Hence what if there is a new way to establish on-chain trust and reduce fraud in decentralized environments. Trust Protocol introduces on-chain trust bonds that allow users to put their money where their mouth is, proving trustworthiness while earning yield on their bond funds.

## **Problem Statement**
**Trust** redefines different human interactions, be that individuals interacting on markets or party members running a state. However, in decentralized environments, ensuring trust and credibility is challenging due to anonymity, the lack of robust verification mechanisms and lack quantifiable credibility. 
Hence the positive externalities trust brings in human interactions is low or even absent on chain. That is something we wanna change.


---

## **Our Solution**
### **On-Chain Trust Bonds**
- Users create bonds by depositing funds into an escrow which gives to only 2 users, the person creating the bond and the other pair signaling trust in another user.
- The funds in the bonds generate yield through yield bearing protocols (like Aave), allowing users to not ideally lock funds while still participating in the protocol.
- A bond score is measuring on the following parameters of , value locked in bond, number of bonds, context of other users bond score, time of bond and range of a user's bond value


### Challenges:
- **Sybil in Web3**: The ability to create multiple anonymous identities enables bad actors to exploit decentralized platforms through scams, rug pulls, and phishing attacks. Sybil resistance is a huge challenge for `Trust protocol` currently.
- **Track of Credibility**: Honest users have other reliable way to demonstrate trustworthiness which may not be captured fully by the protocol, reducing collaboration and opportunities.


### **New Features**
1. **Social Trust Graph**: Reputation is calculated based on trust relationships and weighted metrics.
2. **Types of Bonds**: Support for one-way and regular bonds with different weights in reputation scoring.
3. **Transitive Trust**: A trusts B, B trusts C, so A indirectly trusts C, contributing to the social trust graph.
4. **Community Reward Pool**: Yield from collected withdrawal fees, grants, and funds is distributed to honest and loyal bondholders.

---



### **Reputation Score**
Reputation lies at the core of Trust Protocol, encouraging users to behave honestly by rewarding good actions and penalizing bad ones.

#### **Parameters for Reputation Calculation**:
1. **Time**: Duration for which someone maintains trust bonds without dissolving them.
2. **Amount**: Total bond money currently held by a user.
3. **Reputation**: The reputation score of users creating bonds with you (higher reputation connections carry more weight).
4. **Number**: Total number of active bonds.
5. **Type of Bond**:
   - **One-Way Bonds**: User A trusts User B and deposits money without requiring B to reciprocate.
   - **Regular Bonds**: Both users deposit money, demonstrating mutual trust. Regular bonds carry a higher weight in score calculations.
6. **Transitive Trust**: Trust connections through intermediaries enhance reputation, fostering a web of trust.




---

### **Community Reward Pool**
The Community Reward Pool incentivize loyalty and honesty within the ecosystem.

#### Sources of the Reward Pool:
1. **Withdrawal Fees**: Charged when users break bonds or dissolve them prematurely.
2. **Dissolving Fees**: Small fees charged when users take back their money by dissolving bonds.
3. **Grants and Funds**: Contributions from organizations supporting the protocol.

### Distribution:
- Yield from the pool is distributed to bondholders who:
  - Maintain bonds for longer durations.
  - Actively create trust and build the community.

---
## Architecture
### **How It Works**
1. **Create/Join a Bond**:
   - Deposit funds into a bond (one-way or mutual).
2. **Earn Reputation**:
   - Active bonds increase your reputation over time.
3. **Breaking Bonds**:
   - Incurs a **5% penalty** to the treasury, **reputation loss**, and an **SBT (Soulbound Token)** in your wallet marking you as a bond breaker.
4. **Build Credibility**:
   - On-chain reputation unlocks perks and trustworthiness across Web3 platforms.

```mermaid
    flowchart LR
        %% Starting point
        Start[Enter Into Protocol]
    
        %% User decision: Create, Join, or Quit
        Start --> A[Sign In]
        A --> B{What do you want to do?}
        B --> C[Create a Trust Bond]
        B --> D[Join an Existing Trust Bond]
        B --> E[Quit an Existing Bond]
    
        %% Create a Trust Bond
        C --> C2[Invite Other Party]
        C2 --> C3[Other Party Accepts Invitation?]
        C3 --> |Yes| C4[Both Parties Deposit Funds]
        C3 --> |No| C5[Bond Creation Fails]
    
        C4 --> C6[Bond is Active]
        C6 --> F[Funds Earn Yield]
        C6 --> G[Reputation Improves Over Time]
    
        %% Join an Existing Trust Bond
        D --> D1[Enter Bond ID]
        D1 --> D2[Verify Bond Details]
        D2 --> D3[Deposit Funds]
        D3 --> C6
    
        %% Quit an Existing Bond
        E --> E1[Select Bond to Quit]
        E1 --> E2{Bond Active or Broken?}
        E2 --> |Active| E3[Quit Bond]
        E3 --> E4[1% into treasury]
        E4 --> E5[Funds Withdrawn]
        E2 --> |Broken| E6[Broken or Not Found]
    
        %% Breaking the Bond
        C6 --> H{Break the Bond?}
        H --> |Yes| H1[Bond Breaks]
        H1 --> H2[5% Penalty Deducted to Treasury]
        H2 --> H3[Breaker's Reputation Decreases]
        H3 --> H4[Other Party's Reputation Intact]
        H --> |No| H5[Bond Remains Active]
    
        %% Ending points
        H4 --> End1[Bond Ends]
        H5 --> End2[Continue Earning Yield]
        E5 --> End4[Bond Ended by Quit]
    

```
### **Sequence Diagram**

```mermaid
sequenceDiagram
  actor User1 as User 1
  actor User2 as User 2
  actor Beneficiary as Beneficiary
  participant UserContract as User Contract
  participant BondContract as Bond Contract
  participant YieldContract as Yield Contract
  participant TreasuryContract as Treasury Contract

  User1 ->> UserContract: Deposit money
  User1 ->> UserContract: Create bond with User 2 
  UserContract ->> BondContract: Create Bond by depositing money
  BondContract ->> YieldContract: Stake deposited money
  YieldContract -->> BondContract: Return staked tokens

  BondContract -->> User1: Bond active
  BondContract -->> User2: Bond active

  BondContract ->> User2: Notify invitation
  User2 -->> BondContract: Accept bond invitation
  UserContract ->> BondContract: Deposit money
  BondContract ->> YieldContract: Stake deposited money
  YieldContract -->> BondContract: Return staked tokens

  alt Manage bond options
    UserContract ->> BondContract: Withdraw bond
    BondContract ->> YieldContract: Unstake money
    YieldContract -->> BondContract: Return funds
    BondContract ->> TreasuryContract: Transfer 1% fee
    BondContract -->> UserContract: Return deposited money (1% fee deducted)
  else
    UserContract ->> BondContract: Break bond
    BondContract ->> YieldContract: Unstake money
    YieldContract -->> BondContract: Return funds
    BondContract ->> TreasuryContract: Transfer 5% fee
    BondContract -->> UserContract: Return total bond money minus 5% fee
    BondContract -->> User2: Notify bond break
  end

  opt Claim Yield
    UserContract ->> BondContract: Claim yield
    BondContract ->> YieldContract: Unstake money
    YieldContract -->> BondContract: Return yield amount
    BondContract ->> TreasuryContract: Transfer flat fee from yield
    BondContract -->> UserContract: Return yield minus flat fee
  end

  opt Beneficiary Role
    UserContract ->> Beneficiary: Assign wallet address as beneficiary
    BondContract -->> Beneficiary: Permission to withdraw bond funds after 5 years of inactivity
    Beneficiary ->> BondContract: Withdraw bond funds and yield
    BondContract ->> YieldContract: Unstake remaining funds
    YieldContract -->> BondContract: Return funds and yield
    BondContract ->> TreasuryContract: Transfer flat fee from yield
    BondContract -->> Beneficiary: Transfer all funds and yield
  end

```
### **Technical Architecture Overview**
![image](https://github.com/user-attachments/assets/61c30423-2315-4909-965a-5484f998e5a7)


### **Smart Contracts Architecture**

```mermaid
    flowchart TD
        A[Factory Contract]
        A --> B[User Wallet Contract]
        B --> C[Bond Contract]
        D[Treasury Contract] --> B
        E[Settings Contract] --> C
```
### **Contracts Class Diagram**


```mermaid
classDiagram
  class UserContractFactory {
    <<Universal Upgradable Proxy Standard>>
    +event(address userContract, address owner)
    +initialize(factoryUserContractSettings : address, variable : address)
    +updateFactoryUserContractSettings(factoryUserContractSettings : address)
    +createUser(owner : address, beneficiary : address, userWalletSetting : address, identityService : address[], calldata : bytes32[], shouldVerify : bool, attestationServiceContract : address)
    -factoryUserContractSettings : address
  }

  class FactoryUserContractSettings {
    <<Universal Upgradable Proxy Standard>>
    +flatFeesUserCreate : uint
    +percentageFeesUserCreate : uint
    +treasuryAddress : address
  }

  class IdentityService {
    +verify(calldata : bytes32) : bool
  }

  class UserContract {
    +createBond(bondSettings : address, tokenAddress : address, yieldProviderInterface : address, amount : uint) : address
  }

  class UserWalletSettingContract {
    <<Universal Upgradable Proxy Standard>>
    +flatFeesBondCreate : uint
    +percentageFeesBondCreate : uint
    +flatFeesBondWithdraw : uint
    +percentageFeesBondWithdraw : uint
    +flatFeesBondBreak : uint
    +percentageFeesBondBreak : uint
    +treasuryAddress : address
  }

  class AttestationServiceContract {
    <<Universal Upgradable Proxy Standard>>
  }

  class EthereumAttestationService {
  }

  class UserCore {
    +totalValue : uint
    +totalBonds : uint
    +totalSlash : uint
    +totalBoundBroken : uint
    +highestBondValue : uint
    +lowestBoundValue : uint
    +withdrawBond(bond : address, amount : uint)
    +breakBond(bond : address, amount : uint)
    +withdrawOrBreakBond(bond : address, amount : uint)
  }

  class UserApprovalsAndHooks {
    +approveSlash()
    +approveAttestation()
  }

  class BondSettings {
    <<Universal Upgradable Proxy Standard>>
    +flatFeesBondCreate : uint
    +percentageFeesBondCreate : uint
    +flatFeesBondWithdraw : uint
    +percentageFeesBondWithdraw : uint
    +flatFeesBondBreak : uint
    +percentageFeesBondBreak : uint
    +treasuryAddress : address
  }

  class BondContract {
    +withdrawYield()
    +withdrawFunds(amount : uint)
    +breakBonds(amount : uint)
    -tokenAddress : address
    -yieldProviderAddress : address
  }

  class YieldProviderInterface {
    <<Universal Upgradable Proxy Standard>>
    +depositToken() external view returns (address)
    +balanceOfToken(address addr) external returns (uint256)
    +supplyTokenTo(uint256 amount, address to) external
    +redeemToken(uint256 amount) external returns (uint256)
  }

  class AaveLiquidStakingVault {
  }

  class TreasuryContract {
  }

  AttestationServiceContract --> EthereumAttestationService : one-to-one
  UserContract <|-- UserCore
  UserContract <|-- UserApprovalsAndHooks
  UserContract --> UserWalletSettingContract : one-to-one
  UserContractFactory --> FactoryUserContractSettings : one-to-one
  UserContractFactory --> IdentityService : "createUser (one-to-many)"
  UserContractFactory --> UserContract : "createUser (one-to-many)"
  UserContractFactory --> AttestationServiceContract : one-to-one
  UserContract --> BondContract : "createBond"
  BondSettings --> BondContract : one-to-one
  BondContract --> YieldProviderInterface : one-to-one
  YieldProviderInterface --> AaveLiquidStakingVault : one-to-one
  FactoryUserContractSettings --> TreasuryContract : one-to-one
  BondSettings --> TreasuryContract : one-to-one
  UserWalletSettingContract --> TreasuryContract : one-to-one

```





## **Use Cases**
- **Proof of Trustworthiness**: Show verifiable trust for perks in dApps.
- **Earn Yield**: Deposits generate passive income.
- **Interoperable Reputation**: Carry your credibility across multiple platforms.
- **Fraud Prevention**: Discourage bad actors with transparent penalties and rewards.

---

## **Join Us**

Trust Protocol is paving the way for a safer, more trustworthy decentralized future. By combining financial stakes, reputation scoring, and rewards, we aim to transform trust in Web3. TG: https://t.me/+AbV5oU_p50s2YjQ1



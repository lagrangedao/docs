# Data NFT

<!-- DataNFT for Dataset Licensing is a platform that allows dataset owners to tokenize their datasets uploaded on the Lagrange Platform and grant access to others users via NFTs. It utilizes blockchain technology to validate ownership and access rights, ensuring a secure and transparent ecosystem. -->

## What is a Data NFT? 🤔

A Data NFT represents the ownership and rights for a specific data asset on the blockchain. The owner has the claim on the base intellectual property and can distribute sub-licenses to other users, giving them permission to access the data.

## Use Case

Data NFTs establishes the ownership of data assets, which would allow for data transferability and data sales while maintaining a verifiable record of ownership. It also allows data owners to define who can access the data, for what purposes, and under what conditions, in order to protect their own intellectual property.

Data NFTs enable the tokenization of the base intellectual property, which maintains a traceable record of ownership on the blockchain, allows for easy transfer and trade of ownership, and provides opportunities for revenue generation through the creation and sale of datatokens associated with the underlying data.

## Workflow 🧩

<!--
```mermaid
sequenceDiagram;
  participant A as User
  participant B as Lagrange Platform
  participant C as Smart Contract
  participant D as DON
    A->>B: 1. Upload data asset;
    A->>B: 2. Request DataNFT Generation
    B->>C: 3. claimDataNFT(datasetName, metadataUri)
    C->>D: 4. Generate Metadata on IPFS
    D->>C: 5. Returns CID
    C->>A: 6. Creates New Data NFT
```
-->

<figure><img src=".gitbook/assets/datanft.svg" alt=""><figcaption></figcaption></figure>

1. Users onboard their data onto Lagrange Platform
2. Users click Generate dataNFT button on frontend
3. Lagrange will generate metadata on IPFS and Multichain.Storage
4. Frontend will call smart contract function to create dataNFT
   1. Contract will call Chainlink Oracle
   2. Oracle will call Lagrange API to perform a soft verification
   3. contract deploys new Data NFT contract
5. Backend will scan network for the transaction
6. Frontend displays information about the dataNFT
7. Users can also interact with the new contract to create licenses

## Smart Contracts 🛠

The Lagrange Platform creates new Data NFTs via the DataNFTFactory contract.

The Data NFTs are implemented using the ERC721 standard. Built on top of the OpenZeppelin contract library and implement the ChainlinkClient library. In the future we plan to migrate to Chainlink Functions, which is currently in BETA.

### Contract Addresses

Below are the contract addresses of the DataNFTFactory for each network. They are all using the same contract address!

| Network | Address                                    |
| ------- | ------------------------------------------ |
| Sepolia | 0xb7545455111a1cf7Cde72E8816bf21274D88aa81 |
| Mumbai  | 0xb7545455111a1cf7Cde72E8816bf21274D88aa81 |
| Polygon | 0xb7545455111a1cf7Cde72E8816bf21274D88aa81 |

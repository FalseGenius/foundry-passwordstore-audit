### [S-#] S-01 Storing password on chain makes it visible to anyone, regardless of the variable visibility. 

**Description:** All data stored on-chain is visible to anyone, and it can be read directly from the blockchain. The `PasswordStore::s_password` variable is intended to be a private variable, and only accessed through `PasswordStore::getPassword()` function, which is intended to be called by the owner of the contract.

We show one such method of reading any data off the chain below.

**Impact:** 

**Proof of Concept:**

**Recommended Mitigation:** 
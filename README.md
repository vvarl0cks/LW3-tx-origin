# 🧠 tx.origin vs msg.sender
## Why not to use tx.origin to determine the sender of a transaction

# What is tx.origin?

tx.origin is a global variable which returns the address of the account which sent the transaction. Now you might be wondering then what is msg.sender 🤔. The difference is that tx.origin refers to the original external account (which is the user) that started the transaction and msg.sender is the immediate account that called the function and it can be an external account or another contract calling the function.

So for example, if User calls Contract A, which then calls contract B within the same transaction, msg.sender will be equal to Contract A when checked from inside Contract B. However, tx.origin will be the User regardless of where you check it from.

```shell
gitpod /workspace/LW3-tx-origin (main) $ npx hardhat test
Compiled 3 Solidity files successfully


  tx.origin
Good Contract's Address: 0x8464135c8F25Da09e49BC8782676a84730C318bC
Attack Contract's Address 0x5FbDB2315678afecb367f032d93F642f64180aa3
    ✔ Attack.sol will be able to change the owner of Good.sol (1590ms)

  Lock
    Deployment
      ✔ Should set the right unlockTime (84ms)
      ✔ Should set the right owner
      ✔ Should receive and store the funds to lock
      ✔ Should fail if the unlockTime is not in the future (59ms)
    Withdrawals
      Validations
        ✔ Should revert with the right error if called too soon
        ✔ Should revert with the right error if called from another account
        ✔ Shouldn't fail if the unlockTime has arrived and the owner calls it (42ms)
      Events
        ✔ Should emit an event on withdrawals
      Transfers
        ✔ Should transfer the funds to the owner


  10 passing (2s)
```
# 👮 Prevention

You should use msg.sender instead of tx.origin to not let this happen. There is almost never a good use case for tx.origin except in very specific cases - and in those times, be VERY careful.

Example:
```shell
function setOwner(address newOwner) public {
    require(msg.sender == owner, "Not owner" );
    owner = newOwner;
}
```
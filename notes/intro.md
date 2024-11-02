### Program

- Piec eof code deployed
- Similar to regular rust binaries Instead `main()` --> `entryPoint()`
- Programs are stateless (No data stored)
- We use account to store both program + data

### Accounts

- everything stored in accounts
- It is like a key-value (Hash map) pair. Each public key have a account. Each public key is correspond to a private key
- This database is stored in validator
> Address is 32 bytes long public key

> Account = Data(Bytes) + Executable(bool) + Lamports(number) + Program Address

- Accounts can store upto 10MB of data
- Accounts require a refundable rent deposit proportional to amount of data
- Every account hasa program owner // @why ???
- Only program owns an account can modify its data and decrease lamports. Anyone can increase

- Data accounts
- Native program: buil-in programs which are part of solana validator eg: 
    - System Program for sol trans
    - BPF Loader: owner of al other programs on network
- Sysvar accounts: special accounts store network cluster state
    - Clock account store time


[Summary so far](https://youtu.be/SgpyL2coaPA?t=1222)


## Transaction
- User/App send Txn to solana validator to interactwith programs deployed in validator
- Txn contains instructions in order
- Txn is atomic

> Normal Account = Data(None) + Executable(False) + Lampots(Amount) + Owner(System Program)

When sending sol from one account to another system program increase/dcrease the amount of lamports in accountinfo stored in validator

[Simple Transfer code](https://youtu.be/SgpyL2coaPA?t=1834)

[Playground](https://beta.solpg.io/656a0ea7fb53fa325bfd0c3e)
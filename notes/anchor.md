# Anchor Framework

```
anchor init anchor-hello-world
```

Command Creates [Code](../anchor-hello-world/programs/anchor-hello-world/src/lib.rs)

[See](https://youtu.be/SgpyL2coaPA?t=3081)

```
anchor build
anchor test
```

An Anchor program consists of three parts. 

1. The program module, 
2. the Accounts struct which is marked with #[derive(Accounts)]
3. declare_id macro. 

The program module is where you write your business logic. The Accounts struct is where you validate accounts. The declare_id macro creates an ID field that stores the address of your program. Anchor uses this hardcoded ID for security checks and it also allows other crates to access your program's address.

```rs
// use this import to gain access to common anchor features
use anchor_lang::prelude::*;


// declare an id for your program
declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");


// write your business logic here
#[program]
mod hello_anchor {
    use super::*;
    pub fn initialize(_ctx: Context<Initialize>) -> Result<()> {
        Ok(())
    }
}


// validate incoming accounts here
#[derive(Accounts)]
pub struct Initialize {}
```


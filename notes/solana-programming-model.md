# Solana Programming Model

Resources: [1](https://www.helius.dev/blog/the-solana-programming-model-an-introduction-to-developing-on-solana#) [2](https://solana.com/docs/programs/lang-rust#program-entrypoint) [3](https://github.com/Ackee-Blockchain/school-of-solana/tree/master/3.lesson#accounts)

### Simple Program

```rs
use anchor_lang::prelude::*;

declare_id!("BB854xoW3PJKydEAs8mhnd8TnctPZiyMu5e8V9JfHug1");

#[program]
pub mod anchor_hello_world {
    use super::*;

    // Initialize the greeting account with a custom message
    pub fn initialize(ctx: Context<Initialize>, message: String) -> Result<()> {
        let greeting_account = &mut ctx.accounts.greeting_account;
        greeting_account.message = message;
        msg!("Greeting message set to: {:?}", greeting_account.message);
        Ok(())
    }

    // Update the greeting message in the greeting account
    pub fn update_message(ctx: Context<UpdateMessage>, new_message: String) -> Result<()> {
        let greeting_account = &mut ctx.accounts.greeting_account;
        greeting_account.message = new_message;
        msg!("Greeting message updated to: {:?}", greeting_account.message);
        Ok(())
    }
}

// Account structure to store the greeting message
#[account]
pub struct GreetingAccount {
    pub message: String,
}

// Context for initializing the GreetingAccount
#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = user, space = 8 + 64)]
    pub greeting_account: Account<'info, GreetingAccount>,
    #[account(mut)]
    pub user: Signer<'info>,
    pub system_program: Program<'info, System>,
}

// Context for updating the greeting message
#[derive(Accounts)]
pub struct UpdateMessage<'info> {
    #[account(mut)]
    pub greeting_account: Account<'info, GreetingAccount>,
}
```

# Explanation

1.  
    ```rust
    #[account] 
    pub struct GreetingAccount { pub message: String, }
    ```
    This struct defines the data structure stored on-chain.
    `#[account]`: The `#[account]` attribute tells Anchor that this struct will represent an on-chain account with data fields.
    `pub message`: String: A single field in GreetingAccount to store the greeting message as a String.
2.  `initialize` Function

    - Purpose: This function initializes a new GreetingAccount with a custom message.
    - Parameters 
        - `ctx`: Context<Initialize>: Context for the Initialize instruction, including all accounts needed. 
        - `message`: String: The greeting message to store in the GreetingAccount.
    - Logic:
        - `let greeting_account = &mut ctx.accounts.greeting_account;`: This line gets a mutable reference to `greeting_account` from the context.
        - `greeting_account.message = message;`: Sets the greeting message in the account.
        - The message is then logged on-chain using `msg!`.

    - Account Constraints:
        - The greeting_account account is initialized using `#[account(init, payer = user, space = 8 + 64)]`, which:
            - `init`: Indicates that this is a new account to be created.
            - `payer = user`: Specifies that user will pay for the account's creation.
            - `space = 8 + 64`: Allocates enough space for the account, with 8 bytes for account metadata and 64 bytes for the message.
3. `update_message` Function:
    - Purpose: Updates the existing message in the GreetingAccount.
    - Parameters:
        - `ctx`: `Context<UpdateMessage>`: Context for the UpdateMessage instruction, which requires `greeting_account`.
        - `new_message: String`: The new message to update in the GreetingAccount.
    - Logic:
        - `let greeting_account = &mut ctx.accounts.greeting_account;`: Gets a mutable reference to the greeting_account.
        - `greeting_account.message = new_message;`: Updates the message field with new_message.

    - Account Constraints:
        - `#[account(mut)]` for greeting_account allows it to be modified in this function, ensuring only the program can update this account.
4. `#[derive(Accounts)]` Structs: Initialize and UpdateMessage
    - Initialize:
        - Defines accounts needed to initialize GreetingAccount.
    - `#[account(init, payer = user, space = 8 + 64)]`: Creates the greeting_account with user covering rent costs.
    - `Signer<'info>`: Indicates that user is a signer and is required to approve creating this account.
    - `system_program`: Required by Solana to initialize new accounts.
    - `UpdateMessage`:
        - Requires `greeting_account` to be mutable (`#[account(mut)]`), so it can be updated.


## Whats ```'info``` ?

```rust
#[derive(Accounts)]
pub struct UpdateMessage<'info> {
    #[account(mut)]
    pub greeting_account: Account<'info, GreetingAccount>,
}
```

> ‘info is a custom lifetime specifier

By defining ‘info, Rust knows that the references (like Account<'info, GreetingAccount>) do not outlive the context in which they were borrowed, preventing invalid memory access.

## `#[account]` vs `#[derive(Accounts)]`

| Attribute            | Purpose                                   | Usage                                |
|----------------------|-------------------------------------------|--------------------------------------|
| `#[account]`         | Marks a struct as an on-chain data container | Used for persistent data storage in Solana accounts |
| `#[derive(Accounts)]` | Marks a struct as a context for instruction accounts | Used to define accounts needed by an instruction |

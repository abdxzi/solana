# Solana Programming Model - ll

[Resource](https://github.com/Ackee-Blockchain/school-of-solana/tree/master/4.lesson) for CPI, PDA


32 Bytes Public Key

# PDA
PDA doesn't have private key
Deterministic
Program Signing
intentionally falls of ED25519 curve (Means not in it)
Derived from seeds, program Id, bump (Bump used to make the address fall of the curve) [see](https://youtu.be/NcSj1t7_ack?t=958)

```ts
import { PublicKey } from "@solana/web3.js";

const programId = new PublicKey("11111111111111111111111111111111");
const string = "helloWorld";
const bump = 254;

const PDA = PublicKey.createProgramAddressSync(
	[Buffer.from(string), Buffer.from([bump])],
	programId
);

console.log(`PDA: ${PDA}`);
// https://beta.solpg.io/66031f8ecffcf4b13384cff1
// https://beta.solpg.io/66032009cffcf4b13384cff2
// tinyurl.com/program-derived-address2
```


# Program 
```
main() in rust is entrypoint() in contract
```

Program are stateless
Account used to store code and data


# Cross Program invocation

[see](https://youtu.be/NcSj1t7_ack?t=1507)

Programs can sign on behalf PDA that derived from its program ID
A program can make CPIs up to depth 4

## `invoke()`
used when making CPIs that **does not require PDA signers**
## `invoke_signed()`
used whe require PDA signers
seeds used to sign PDA are passed =  `signer_seeds`

When CPI proccessed solana runtime internally calls `create_program_address` using signer_seeds, programID to derive PDA

```ts
invoke(
	instruction, 
	&[from_pubkey, to_pubkey, program_id]
)?;

// https://beta.solpg.io/github.com/ZYJLiu/doc-examples/tree/main/cpi-invoke

invoke_signed(
	instruction,
	&[from_pubkey, to_pubkey, program_id],
	signer_seeds,
)?;
// https://beta.solpg.io/github.com/ZYJLiu/doc-examples/tree/main/cpi-invoke-signed
```


# Hands on PDA Example
[See](https://youtu.be/NcSj1t7_ack?t=3438)


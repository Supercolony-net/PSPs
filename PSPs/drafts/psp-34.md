# Non-Fungible Token Standard for Substrate's `contracts` pallet

- **PSP Number:** 34
- **Authors:** Pierre Ossun <pierre.ossun@supercolony.net>, Green Baneling <green.baneling@supercolony.net>, Markian <markian@supercolony.net>
- **Status:** Draft
- **Created:** 2021-11-22
- **Reference Implementation** [OpenBrush](https://github.com/Supercolony-net/openbrush-contracts/blob/main/contracts/token/psp721/psp721.rs)

## Summary

A standard for a Non-Fungible Token interface for WebAssembly smart contracts which run on Substrate's [`contracts` pallet](https://github.com/paritytech/substrate/tree/master/frame/contracts).

This proposal aims to define the standard Non-Fungible Token interface for WebAssembly smart contracts, just like [EIP-721](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md) for the Ethereum ecosystem.

## Motivation

Without a standard interface for Non-Fungible Token every contract will have different signature and types. Hence, no interoperability is possible.
This proposal aims to resolve that by defining one **interface** that shares the same **ABI** of **permissionless** methods between all implementations.

The goal is to have a standard contract interface that allows tokens deployed on Substrate's `contracts` pallet to be re-used by other applications: from wallets to decentralized exchanges.

## Implementations
Examples of implementations:

- [OpenBrush](https://github.com/Supercolony-net/openbrush-contracts/blob/main/contracts/token/psp721/psp721.rs), written in [ink!](https://github.com/paritytech/ink).

## Motivation for having a standard separate from ERC-721
Due to the different nature of WebAssembly smart contracts and the difference between EVM and the [`contracts` pallet](https://github.com/paritytech/substrate/tree/master/frame/contracts) in Substrate, this standard proposal has specific rules and methods,
therefore PSP-34 differs from ERC-721 in its implementation. Also the proposal contains new methods that should improve the usability of Non-Fungible Token.

# This standard is at ABI level

Substrate's [`contracts` pallet](https://github.com/paritytech/substrate/tree/master/frame/contracts) can execute any WebAssembly contract that implements its defined API; we do not want to restrain this standard to only Rust and [the ink! language](https://github.com/paritytech/ink), but make it possible to be implemented by any language/framework that compiles to WebAssembly.

## Specification
1. [Interfaces](#Interfaces)
2. [Extension](#Extension)
3. [Events](#Events)
4. [Types](#Types)
5. [Errors](#Errors)

### Interfaces

#### PSP-34 Interface

This section defines the required interface for this standard.

##### **collection_id**() ➔ Id
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [],
  "docs": [
    "Returns the collection `Id` of the NFT token.",
    "",
    "This can represents the relationship between tokens/contracts/pallets."
  ],
  "mutates": false,
  "name": [
    "PSP34",
    "collection_id"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Id"
    ],
    "type": "Id"
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **balance_of**(owner: AccountId) ➔ u32
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "owner",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    }
  ],
  "docs": [
    "Returns the balance of the owner.",
    "",
    "This represents the amount of unique tokens the owner has."
  ],
  "mutates": false,
  "name": [
    "PSP34",
    "balance_of"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "u32"
    ],
    "type": 1
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **owner_of**(id: Id) ➔ AccountId
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    }
  ],
  "docs": [
    "Returns the owner of the token."
  ],
  "mutates": false,
  "name": [
    "PSP34",
    "owner_of"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
       "AccountId"
    ],
    "type": "AccountId"
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **get_approved**(id: Id) ➔ Option<AccountId>
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    }
  ],
  "docs": [
    "Returns the approved account ID for this token if any."
  ],
  "mutates": false,
  "name": [
    "PSP34",
    "get_approved"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Option"
    ],
    "type": "Option<AccountId>"
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **is_approved_for_all**(owner: AccountId, operator: AccountId) ➔ bool
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "owner",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "operator",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    }
  ],
  "docs": [
    "Returns `true` if the operator is approved by the owner."
  ],
  "mutates": false,
  "name": [
    "PSP34",
    "is_approved_for_all"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "bool"
    ],
    "type": "bool"
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **set_approval_for_all**(operator: AccountId, approved: bool) ➔ Result<(), PSP34Error>
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "operator",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "approved",
      "type": {
        "displayName": [
        "bool"
        ],
        "type": "bool"
      }
    }
  ],
  "docs": [
    "Approves or disapproves the operator for all tokens of the caller.",
    "",
    "On success a `ApprovalForAll` event is emitted.",
    "",
    "# Errors",
    "",
    "Returns `SelfApprove` error if it is self approve."
  ],
  "mutates": true,
  "name": [
    "PSP34",
    "set_approval_for_all"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Result"
    ],
    "type": 1
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **approve**(to: AccountId, id: Id) ➔ Result<(), PSP34Error>
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "to",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    }
  ],
  "docs": [
    "Approves the account to transfer the specified token on behalf of the caller.",
    "",
    "On success a `Approval` event is emitted.",
    "",
    "# Errors",
    "",
    "Returns `SelfApprove` error if it is self approve.",
    "",
    "Returns `NotApproved` error if caller is not owner of `id`."
  ],
  "mutates": true,
  "name": [
    "PSP34",
    "approve"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Result"
    ],
    "type": 1
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **transfer**(to: AccountId, id: Id, data: [u8]) ➔ Result<(), PSP34Error>
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "to",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    },
    {
      "name": "data",
      "type": {
        "displayName": [
          "[u8]"
        ],
        "type": "[u8]"
      }
    }
  ],
  "docs": [
    "Transfer approved or owned token from caller.",
    "",
    "On success a `Transfer` event is emitted.",
    "",
    "# Errors",
    "",
    "Returns `TokenNotExists` error if `id` is not exist.",
    "",
    "Returns `SafeTransferCheckFailed` error if `to` doesn't accept transfer."
  ],
  "mutates": true,
  "name": [
    "PSP34",
    "transfer"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Result"
    ],
    "type": 1
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **transfer_from**(from: AccountId, to: AccountId, id: Id, data: [u8]) ➔ Result<(), PSP34Error>
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "from",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "to",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    },
    {
      "name": "data",
      "type": {
        "displayName": [
          "[u8]"
        ],
        "type": "[u8]"
      }
    }
  ],
  "docs": [
    "Transfer approved or owned token from `from`.",
    "",
    "On success a `Transfer` event is emitted.",
    "",
    "# Errors",
    "",
    "Returns `TokenNotExists` error if `id` does not exist.",
    "",
    "Returns `NotApproved` error if `from` doesn't have allowance for transferring.",
    "",
    "Returns `SafeTransferCheckFailed` error if `to` doesn't accept transfer."
  ],
  "mutates": true,
  "name": [
    "PSP34",
    "transfer_from"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Result"
    ],
    "type": 1
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

#### PSP34Receiver
`PSP34Receiver` is an interface for any contract that wants to support safe transfers from a PSP-30 token smart contract to avoid unexpected tokens in the balance of contract.
This method is called before a transfer to ensure the recipient of the tokens acknowledges the receipt.

##### **before_received**(operator: AccountId, from: AccountId, id: Id, data: [u8]) ➔ Result<(), PSP34ReceiverError>
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "operator",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "from",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    },
    {
      "name": "data",
      "type": {
        "displayName": [
          "[u8]"
        ],
        "type": "[u8]"
      }
    }
  ],
  "docs": [
    "Ensures that the smart contract allows reception of PSP34 token(s).",
    "Returns `Ok(())` if the contract allows the reception of the token(s) and Error `TransferRejected(String)` otherwise.",
    "",
    "This method will get called on every transfer to check whether the recipient in `transfer`",
    "or `transfer_from` is a contract, and if it is, does it accept tokens.",
    "This is done to prevent contracts from locking tokens forever.",
    "",
    "Returns `PSP34ReceiverError` if the contract does not accept the tokens."
  ],
  "mutates": true,
  "name": [
    "PSP34Receiver",
    "before_received"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Result"
    ],
    "type": 2
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

### Extension

#### PSP34Metadata

`PSP34Metadata` is a **recommended** extension for this Non-Fungible Token standard
because it is main feature of the NFT.

##### **total_supply**() ➔ Balance
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [],
  "docs": [
    "Returns the current total supply of the NFT."
  ],
  "mutates": false,
  "name": [
    "PSP34Metadata",
    "total_supply"
  ],
  "returnType": {
    "displayName": [
      "Balance"
    ],
    "type": "Balance"
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

##### **get_attribute**(id: Id, key: [u8]) ➔ Option<[u8]>
Selector: `` - first 4 bytes of `blake2b_256("")` // TO UPDATE WHEN PSP NUMBER
```json
{
  "args": [
    {
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    },
    {
      "name": "key",
      "type": {
        "displayName": [
          "[u8]"
        ],
        "type": "[u8]"
      }
    }
  ],
  "docs": [
    "Returns the attribute of `id` for the given `key`.",
    "",
    "If `id` is a collection id of the token, it returns attributes for collection."
  ],
  "mutates": false,
  "name": [
    "PSP34Metadata",
    "get_attribute"
  ],
  "payable": false,
  "returnType": {
    "displayName": [
      "Option"
    ],
    "type": "Option<[u8]>"
  },
  "selector": "" // TO UPDATE WHEN PSP NUMBER
}
```

The list of required attributes for NFT should be defined in a separate 
proposal based on the scope of the usage.

### Events

#### Transfer
When a contract creates (mints) new tokens, `from` will be `None`.
When a contract deletes (burns) tokens, `to` will be `None`.
```json
{
  "args": [
    {
      "docs": [],
      "indexed": true,
      "name": "from",
      "type": {
        "displayName": [
          "Option"
        ],
        "type": "Option<AccountId>"
      }
    },
    {
      "docs": [],
      "indexed": true,
      "name": "to",
      "type": {
        "displayName": [
          "Option"
        ],
        "type": "Option<AccountId>"
      }
    },
    {
      "docs": [],
      "indexed": true,
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    }
  ],
  "docs": [
    "Event emitted when a token transfer occurs."
  ],
  "name": "Transfer"
}
```

### Approval
```json
{
  "args": [
    {
      "docs": [],
      "indexed": true,
      "name": "from",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "docs": [],
      "indexed": true,
      "name": "to",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "docs": [],
      "indexed": true,
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    }
  ],
  "docs": [
    "Event emitted when a token approve occurs."
  ],
  "name": "Approval"
}
```

### ApprovalForAll
```json
{
  "args": [
    {
      "docs": [],
      "indexed": true,
      "name": "owner",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "docs": [],
      "indexed": true,
      "name": "operator",
      "type": {
        "displayName": [
          "AccountId"
        ],
        "type": "AccountId"
      }
    },
    {
      "docs": [],
      "indexed": false,
      "name": "approved",
      "type": {
        "displayName": [
          "bool"
        ],
        "type": "bool"
      }
    }
  ],
  "docs": [
    "Event emitted when an operator is enabled or disabled for an owner.",
    "The operator can manage all NFTs of the owner."
  ],
  "name": "ApprovalForAll"
}
```

### AttributeSet
```json
{
  "args": [
    {
      "docs": [],
      "indexed": false,
      "name": "id",
      "type": {
        "displayName": [
           "Id"
        ],
        "type": "Id"
      }
    },
    {
      "docs": [],
      "indexed": false,
      "name": "key",
      "type": {
        "displayName": [
          "[u8]"
        ],
        "type": "[u8]"
      }
    },
    {
      "docs": [],
      "indexed": false,
      "name": "data",
      "type": {
        "displayName": [
          "[u8]"
        ],
        "type": "[u8]"
      }
    }
  ],
  "docs": [
    "Event emitted when an attribute is set for a token.",
  ],
  "name": "AttributeSet"
}
```

### Types
```rust
// Id is an Enum and its variant are types
enum Id {
    U8(u8),
    U16(u16),
    U32(u32),
    U64(u64),
    U128(u128),
    Bytes(Vec<u8>),
}

// AccountId is a 32 bytes Array, like in Substrate-based blockchains.
type AccountId = [u8; 32];
```

#### Return types
```json
{
  "types": {
    "1": {
      "def": {
        "variant": {
          "variants": [
            {
              "fields": [
                {
                  "type": {
                    "def": {
                      "tuple": []
                    }
                  }
                }
              ],
              "name": "Ok"
            },
            {
              "fields": [
                {
                  "type": {
                    "def": {
                      "variant": {
                        "variants": [
                          {
                            "fields": [
                              {
                                "type": "string"
                              }
                            ],
                            "name": "Custom"
                          },
                          {
                            "name": "SelfApprove"
                          },
                          {
                            "name": "NotApproved"
                          },
                          {
                            "name": "TokenExists"
                          },
                          {
                            "name": "TokenNotExists"
                          },
                          {
                            "fields": [
                              {
                                "type": "string"
                              }
                            ],
                            "name": "SafeTransferCheckFailed"
                          }
                        ]
                      }
                    },
                    "path": [
                      "PSP34Error"
                    ]
                  }
                }
              ],
              "name": "Err"
            }
          ]
        }
      }
    },
    "2": {
      "def": {
        "variant": {
          "variants": [
            {
              "fields": [
                {
                  "type": {
                    "def": {
                      "tuple": []
                    }
                  }
                }
              ],
              "name": "Ok"
            },
            {
              "fields": [
                {
                  "type": {
                    "def": {
                      "variant": {
                        "variants": [
                          {
                            "fields": [
                              {
                                "type": "string"
                              }
                            ],
                            "name": "TransferRejected"
                          }
                        ]
                      }
                    },
                    "path": [
                      "PSP34ReceiverError"
                    ]
                  }
                }
              ],
              "name": "Err"
            }
          ]
        }
      }
    }
  }
}
```

### Errors
The suggested methods revert the transaction and return a [SCALE-encoded](https://github.com/paritytech/parity-scale-codec) `Result` type with one of the following `Error` enum variants:

```rust
enum PSP34Error {
    /// Custom error type for cases if writer of traits added own restrictions
    Custom(String),
    /// Returned if owner approves self
    SelfApprove,
    /// Returned if the caller doesn't have allowance for transferring.
    NotApproved,
    /// Returned if the owner already own the token.
    TokenExists,
    /// Returned if the token doesn't exist
    TokenNotExists,
    /// Returned if safe transfer check fails
    SafeTransferCheckFailed(String),
}

enum PSP34ReceiverError {
    /// Returned if transfer is rejected.
    TransferRejected(String),
}
```

## Copyright

This PSP is placed in the [public domain](https://creativecommons.org/publicdomain/zero/1.0/).

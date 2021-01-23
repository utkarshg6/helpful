# Facts in Solidity

- Each contract is a class and every node deploys it's instance.
- The Identical function name of a contract (class) is the constructor.
- Every Function that returns have either *view* or *constant* in their function declaration.
- We cannot return from functions that modify the contracts data.
- If we define a variable with public, then a function with identical name will get created that will return the variable.
- If the to value is empty in the contract that means it's the first contract.
- You can't use require statements for _.sol_ files because _npm_ doesn't recognize them as JS files.
- You can't use array of strings in Solidity because nested loops don't work.

## Function Types

| Type       | Property                                      |
| ---------- | --------------------------------------------- |
| `public`   | Anyone can call this function.                |
| `private`  | Only contract can call this function.         |
| `view`     | Returns Data, doesn't modify contract's data. |
| `constant` | Returns Data, doesn't modify contract's data. |
| `pure`     | Neither read nor modify contract's data.      |
| `payable`  | When someone sends ether.                     |



## Web3 with Contracts

|   Goal    |  ABI  | Bytecode | Address |
| :-------- | :---: | :------: | :-----: |
|  Creating a Contract  | :heavy_check_mark: |  :heavy_check_mark:   |  :x:  |
| Deploying Contract| :heavy_check_mark:  |  :heavy_check_mark:   |  :heavy_check_mark:  |
| Interacting with Deployed Contract | :heavy_check_mark:  |   :x:   |  :heavy_check_mark:  |

## Data Types

|   Name   |  Examples  |
| :-------- | :---- |
|   `string`   |  `"Hello, World!"`  |
|   `bool`   |  `true` `false`  |
|   `int` or `int256`   |  `-2^255` to `2^255 - 1`  |
|   `uint` or `uint256`   | `0`  to `2^256 -1` |
|   `fixed/ufixed`   |  `3.14` `-1.763`  |
|   `address`   |  `0x17403D9cb75F9A55232D10Ea0066821035c21852`  |

## The _msg_ global variable

- We have access to `msg` whenever we _create a transaction_ or _call a function_.

| Name | Functionality |
| ---- | ---- |
| `msg.data` | 'Data' field from the call or transaction that invoked the current function. |
| `msg.gas` | Amount of gas the current function invocation has available. |
| `msg.sender` | Address of the account that started the current function invocation. |
| `msg.value` | Amount of the ether (in wei) that was sent along with the function invocation. |

## Reference Types in Solidity

| Name          | Explanation                   |
| ------------- | ----------------------------- |
| fixed array   | One Data Type, Fixed Length   |
| dynamic array | One Data Type, Dynamic Length |
| mapping       | JSON or Python Dictionaries   |
| struct        | Similar to that of C/C++      |

## The function modifier

- It is used when functions have repeated code.

```sol
modifier codeThatMayRepeat {
	doThis();
	_;
}

function thaWillDoRepeatedTask codeThatMayRepeat {
	doSomething();
}

function thaWillAlsoDoRepeatedTask codeThatMayRepeat {
	doSomethingToo();
}
```

 ## Testing in JS with Mocha

- `assert.ok` only checks the truthiness.
- function throw helper for try-catch statements in assert modules doesn't work perfectly with the async. Hence, only use `assert()`.
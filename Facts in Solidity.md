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

## Mapping

- Keys are not stored.

- Values are not iterable.

- All values exist.

- Mapping is a reference type unlike variables.

- Lookup Process =>

  ‘orange’ – Hashing Function – Index i – Value

## Struct

### Defining 

```
struct Request {
    string description;
    uint value;
    address recipient;
    bool complete;
}
```

### Instance

```
Request memory newRequest = Request({
    description: description,
    value: value,
    recipient: recipient,
    complete: false
});

requests.push(newRequest);
```



## Storage vs. Memory

- Sometimes references to where our contract stores data.

- Sometimes references to where our solidity variables stores values.

  | Storage                                 | Memory                             |
  | --------------------------------------- | ---------------------------------- |
  | Holds data between function calls.      | Temporary place to store data.     |
  | Similar to Computer’s Hard Drive.       | Similar to Computer’s RAM.         |
  | It points at the same variable.         | It makes the copy of the variable. |
  | Same as Pass by Reference (Persistent). | Same as Pass by Value (No Change). |

```
contract Numbers {
	int[] public numbers;
	
	function Numbers() public {
		numbers.push(10);
		numbers.push(20);
		
		changeArray(numbers);
	}
	
	function changeArray(int[] myArray) private {
		myArray[0] = 5;
	}
}
```

The output of `numbers[0]` will be `10`, because `myArray` is by default defined as `function changeArray(int[] memory myArray)` . Thus `myArray` will act like a RAM and no updates will get reflected to `numbers` after the completion of the function. In case the user wants the contract to make changes in the `numbers` to be persistent then they should replace `memory` with `storage`.

- An instance of a struct must be used with `memory`.

## Dealing with JavaScript

### Installing Compiler

- Make sure to install the exact version.
- Consider the below for `pragma soldity ^0.4.17`

```bash
npm i solc@0.4.17
```

### Compiling 

```javascript
const pathToContract = path.resolve(__dirname, 'contractsFolder', 'Contract.sol');
const source = fs.readFileSync(pathToContract, 'utf8');
const output = solc.compile(source, 1).contracts;
```

### Writing JSON

- Below code is useful when multiple contracts are written in one file.

```javascript
for (let contract in output) {
    fs.outputJSONSync(
        path.resolve(buildPath, contract.slice(1, contract.length) + '.json'),
        output[contract]
    );
}
```

### Importing Contract

```javascript
const compiledContract = require('../ethereum/build/Contract.json');
```

### Ganache and Web3

```javascript
const ganache = require('ganache-cli');
const Web3 = require('web3');

const web3 = Web3(ganache.provider());
```

### Deploying using Ganache

```javascript
let accounts = await web3.eth.getAccounts();

let contract = await web3.eth
	.Contract(JSON.parse(compiledContract.interface))
	.deploy({
		data: compiledContract.bytecode,
	})
	.send({
		from: accounts[0],
		gas: '1000000'
	});

// Calling functions that need money (function that creates tx)
await contract.methods
	.functionThatNeedsMoney('100') // Money in wei
	.send({
		from: accounts[0],
		gas: '1000000'
	});

// Calling functions that do NOT need money (view function)
const data = await contract.methods
    .viewFunction()
    .call();
```

### Receiving deployed Contract

```javascript
const contract = await new web3.eth
    .Contract(
        JSON.parse(compiledContract.interface),
        contractAddress
    );
```


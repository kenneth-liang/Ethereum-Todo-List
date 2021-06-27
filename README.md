# Ethereum Blockchain Application - Todo List

Simple todo list powered by smart contracts. Understand how blockchain works and how to connect an application with a decentralized platform. Unlike traditionally todo list applications, there is no central database where data is located. The data, todo list items, are stored on a network distributed over the blockchain.

## Technology:
* [Solidity](https://docs.soliditylang.org/en/v0.5.3/) - High-level language for implementing Smart Contracts 
* [Truffle Framework](https://www.trufflesuite.com/truffle) - Ethereum DApps 
* [Ganache](https://www.trufflesuite.com/ganache) - Personal Ethereum blockchain on your local machine
* [Metamask](https://metamask.io/) Ethereum Wallet - Chrome Extension 
* [web3.js](https://web3js.readthedocs.io/en/v1.3.4/) - Library for interacting with Ethereum blockchain
* [Mocha](https://mochajs.org/) - Testing Framework
* [Chai](https://www.chaijs.com/) - Assertion Library 
* [Node.js](https://nodejs.org/en/)

## Deployment 
```bash
# Clone 
$ https://github.com/kenneth-liang/Ethereum-Todo-List-.git

# Install dependencies 
$ npm install -g truffle@5.0.2
$ npm install

# Migrate 
truffle migrate --reset

# Run the app
$ npm run dev
```

## Listing Tasks
Modeling task with struct and mapping state variable tasks. Allowing for look up on any task by id.

```solidity
// TodoList.sol
pragma solidity ^0.5.0;

contract TodoList {
  uint public taskCount = 0;

  struct Task {
    uint id;
    string content;
    bool completed;
  }

  mapping(uint => Task) public tasks;
}
```

## Creating Tasks 

createTask function accepts one argument, text for the task, and stores the new task on the blockchain by adding it to tasks. We want to trigger an event any time a new task is created. Solidity allows for the listening of these events inside the client-side application. TaskCreated() is triggered anytime a new task is created in createTask().

```solidity
// TodoList.sol
pragma solidity ^0.5.0;

contract TodoList {

  // ...

  event TaskCreated(
    uint id,
    string content,
    bool completed
  );

  // ...

  function createTask(string memory _content) public {
    taskCount ++;
    tasks[taskCount] = Task(taskCount, _content, false);
    emit TaskCreated(taskCount, _content, false);
  }
}
```

![new_task](https://user-images.githubusercontent.com/59374267/123529698-ca7cef80-d6a7-11eb-8cbb-07dab9350223.gif)

## Completing Tasks

Checking off tasks on the todo list will update the smart contract. 

```solidity
// TodoList.sol
pragma solidity ^0.5.0;

contract TodoList {

  // ...

  event TaskCompleted(
    uint id,
    bool completed
  );

  // ...

  function toggleCompleted(uint _id) public {
    Task memory _task = tasks[_id];
    _task.completed = !_task.completed;
    tasks[_id] = _task;
    emit TaskCompleted(_id, _task.completed);
  }
}
```
![complete_task](https://user-images.githubusercontent.com/59374267/123529708-da94cf00-d6a7-11eb-8970-4456cd45789f.gif)

## Testing
```bash
# Run Test 
$ truffle test 
```

```javascript
// TodoList.test.js
it ('toggles tasks completion', async () => {
  const result = await this.todoList.toggleCompleted(1)
  const task = await this.todoList.tasks(1)
  assert.equal(task.completed, true)
  const event = result.logs[0].args 
  assert.equal(event.id.toNumber(), 1)
  assert.equal(event.completed, true )
})
```
 
<img width="314" alt="Screen Shot 2021-06-26 at 5 55 31 PM" src="https://user-images.githubusercontent.com/59374267/123529689-b5a05c00-d6a7-11eb-9b65-eaa9ea521d3b.png">

## Ganache Personal Blockchain 
Local development blockchain used to mimic the behavior of a public blockchain. Allows for deploying smart contracts, develop applications, and run tests.

<img width="1195" alt="Screen Shot 2021-06-26 at 6 29 06 PM" src="https://user-images.githubusercontent.com/59374267/123530177-67418c00-d6ac-11eb-8b5e-3afd65fc48fc.png">

## Metamask

Google Chrome extension turning your browser into a blockchain browser. Metamask allows for managing our personal account when connecting to the blockchain, as well as manage ETH funds needed to pay for transactions. 

<img width="794" alt="9-metamask" src="https://user-images.githubusercontent.com/59374267/123530068-5cd2c280-d6ab-11eb-9e0b-85c6411f3602.png">
<img width="351" alt="Screen Shot 2021-06-26 at 6 23 32 PM" src="https://user-images.githubusercontent.com/59374267/123530096-a02d3100-d6ab-11eb-9a1f-e4e51233f1f6.png">


## Future
* Upgrade User interface 

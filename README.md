Ethereum Blockchain Application - Todo List

Simple todo list powered by smart contracts. Understand how blockchain works and how to connect an application with a decentralized platform. Unlike traditionaly todo list applcations, there is no central database where data is located. The data, your todo list items, are stored on a networked distributed over the blockchain.


Technology:
Solidity - High-level language for implementing Smart Contracts 
Ganache - Personal Ethereum blockchain on your local machine
Truffle Framework - Ethereum DApps 
Metamask Ethereum Wallet - Chrome Extention 
Node.js 
Mocha Testing Framework
Chai Assertion Library 


Deployment 
npm install -g truffle@5.0.2
npm install
truffle migrate --reset
npm run dev



Listing Tasks
Modeling task with struct and mapping state variable tasks. Allowing for look up on any task by id.

TodoList.sol
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


Creating Tasks 

createTask function accepts one argument, text for the task, and stores the new task on the blockchain by adding it to tasks. We want to trigger an event any time a new task is created. Solidity allows for listening of these events inside the client side application. TaskCreated() is triggered anytime a new task is created in createTask()

TodoList.sol
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



Completing Tasks

Checking off tasks on the todolist will update the smart contract. 

TodoList.sol
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


Testing 
truffle test 


Future
Upgrade User interface 
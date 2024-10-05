# transparency// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TransparentDeposit {

    // Structure to hold deposit details
    struct Deposit {
        address depositor;
        uint256 amount;
        uint256 timestamp;
    }

    // Array to hold all deposits
    Deposit[] public deposits;

    // Event emitted when a deposit is made
    event DepositMade(address indexed depositor, uint256 amount, uint256 timestamp);

    // Function to make a deposit
    function deposit() public payable {
        require(msg.value > 0, "Deposit amount must be greater than zero");

        // Record the deposit
        deposits.push(Deposit({
            depositor: msg.sender,
            amount: msg.value,
            timestamp: block.timestamp
        }));

        // Emit an event for transparency
        emit DepositMade(msg.sender, msg.value, block.timestamp);
    }

    // Function to view total number of deposits
    function getDepositCount() public view returns (uint256) {
        return deposits.length;
    }

    // Function to get deposit details by index
    function getDepositDetails(uint256 index) public view returns (address, uint256, uint256) {
        require(index < deposits.length, "Invalid deposit index");
        Deposit memory dep = deposits[index];
        return (dep.depositor, dep.amount, dep.timestamp);
    }

    // Function to get the total balance of the contract
    function getTotalBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

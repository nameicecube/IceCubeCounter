// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract IceCubeCounter {
    uint256 private count; // Keeping count private for additional security

    address public owner; // Owner of the contract, set at deployment

    event CountChanged(uint256 newCount); // Emitted when count changes
    event CountReset(address resetBy);

    constructor() {
        owner = msg.sender; // Set the deployer as the owner
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action.");
        _;
    }

    // Increment the counter
    function increment() public {
        count += 1;
        emit CountChanged(count); // Log the new count
    }

    // Decrement the counter, ensuring it doesn’t go negative
    function decrement() public {
        require(count > 0, "Count cannot be negative.");
        count -= 1;
        emit CountChanged(count); // Log the new count
    }

    // Reset the counter, but only the contract owner can do this
    function resetCount() public onlyOwner {
        count = 0;
        emit CountReset(msg.sender); // Log the reset
    }

    // Return the current count
    function getCount() public view returns (uint256) {
        return count;
    }

    // Allow the owner to transfer ownership
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "New owner cannot be the zero address.");
        owner = newOwner;
    }
}

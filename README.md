# web3dvlpr2
A code about smart contract for a blockchain-based identity verification system
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract IdentityVerification {
    address public owner;

    struct User {
        bool isVerified;
        mapping(string => string) attributes;
    }

    mapping(address => User) public users;

    event IdentityVerified(address indexed user);
    event AttributeUpdated(address indexed user, string key, string value);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function verifyIdentity() external {
        require(!users[msg.sender].isVerified, "Identity already verified");

        // Perform identity verification logic here (this is a placeholder)
        // In a real-world scenario, this would involve more complex verification processes.

        // For simplicity, we'll just mark the user as verified
        users[msg.sender].isVerified = true;

        emit IdentityVerified(msg.sender);
    }

    function updateAttribute(string memory key, string memory value) external {
        require(users[msg.sender].isVerified, "User identity not verified");
        require(bytes(key).length > 0 && bytes(value).length > 0, "Invalid attribute");

        users[msg.sender].attributes[key] = value;

        emit AttributeUpdated(msg.sender, key, value);
    }

    function getAttribute(address _user, string memory key) external view returns (string memory) {
        return users[_user].attributes[key];
    }

    function isVerified(address _user) external view returns (bool) {
        return users[_user].isVerified;
    }

    function destroy() external onlyOwner {
        selfdestruct(payable(owner));
    }
}

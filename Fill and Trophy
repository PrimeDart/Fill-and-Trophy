// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract ImpactChain {
    struct DonationCategory {
        string name;
        uint256 totalDonations;
    }

    mapping(address => uint256) public impactTokens;
    mapping(string => DonationCategory) public categories;
    mapping(address => mapping(string => bool)) public hasVoted;

    event Donation(address indexed donor, string category, uint256 amount);
    event Vote(address voter, string proposal);

    modifier validDonationAmount(uint256 _amount) {
        require(_amount > 0, "Donation amount should be greater than 0");
        _;
    }

    modifier hasImpactTokens() {
        require(impactTokens[msg.sender] > 0, "Must have Impact Tokens");
        _;
    }

    modifier hasNotVoted(string memory _proposal) {
        require(!hasVoted[msg.sender][_proposal], "Already voted for this proposal");
        _;
    }

    function donate(string memory _category, uint256 _amount)
        public
        validDonationAmount(_amount)
    {
        categories[_category].totalDonations += _amount;
        impactTokens[msg.sender] += _amount;

        emit Donation(msg.sender, _category, _amount);
    }

    function totalDonationsForCategory(string memory _category)
        public
        view
        returns (uint256)
    {
        return categories[_category].totalDonations;
    }

    function voteForProposal(string memory _proposal)
        public
        hasImpactTokens
        hasNotVoted(_proposal)
    {
        hasVoted[msg.sender][_proposal] = true;

        emit Vote(msg.sender, _proposal);
    }
}

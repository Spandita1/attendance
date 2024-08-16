// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract AttendanceTracker {
    mapping(address => uint256) public checkInTimestamps;
    mapping(address => mapping(uint256 => bool)) public attendanceRecords;
    
    event CheckIn(address indexed student, uint256 timestamp);
    event CheckOut(address indexed student, uint256 timestamp);
    
    function checkIn() public {
        uint256 timestamp = block.timestamp;
        require(checkInTimestamps[msg.sender] == 0, "Already checked in");

        checkInTimestamps[msg.sender] = timestamp;
        attendanceRecords[msg.sender][timestamp] = true;
        
        emit CheckIn(msg.sender, timestamp);
    }
    
    function checkOut() public {
        uint256 checkInTime = checkInTimestamps[msg.sender];
        require(checkInTime != 0, "Not checked in");

        uint256 timestamp = block.timestamp;
        attendanceRecords[msg.sender][timestamp] = false;
        checkInTimestamps[msg.sender] = 0;
        
        emit CheckOut(msg.sender, timestamp);
    }

    function isCheckedIn(address student) public view returns (bool) {
        return checkInTimestamps[student] != 0;
    }

    function getAttendanceForTimestamp(address student, uint256 timestamp) public view returns (bool) {
        return attendanceRecords[student][timestamp];
    }
}

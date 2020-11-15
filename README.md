# serenity 4 solidity

## building basic smart contracts with Solidity



## Level One: The AssociateProfitSplitter Contract

At the top of your contract, you will need to define the following public variables:

a. employee_one -- The address of the first employee. Make sure to set this to payable.


b. employee_two -- Another address payable that represents the second employee.


c. employee_three -- The third address payable that represents the third employee.


### Create a constructor function that accepts:


a. address payable _one


b. address payable _two


c. address payable _three


Within the constructor, set the employee addresses to equal the parameter values. This will allow you to avoid hardcoding the employee addresses.
Next, create the following functions:


balance -- This function should be set to public view returns(uint), and must return the contract's current balance. Since we should always be sending Ether to the beneficiaries, this function should always return 0. If it does not, the deposit function is not handling the remainders properly and should be fixed. This will serve as a test function of sorts.


deposit -- This function should set to public payable check, ensuring that only the owner can call the function.


### In this function, perform the following steps:


Set a uint amount to equal msg.value / 3; in order to calculate the split value of the Ether.


Transfer the amount to employee_one.


Repeat the steps for employee_two and employee_three.


Since uint only contains positive whole numbers, and Solidity does not fully support float/decimals, we must deal with a potential remainder at the end of this function since amount will discard the remainder during division.


We may either have 1 or 2 wei leftover, so transfer the msg.value - amount * 3 back to msg.sender. This will re-multiply the amount by 3, then subtract it from the msg.value to account for any leftover wei, and send it back to Human Resources.

Create a fallback function using function() external payable, and call the deposit function from within it. This will ensure that the logic in deposit executes if Ether is sent directly to the contract. This is important to prevent Ether from being locked in the contract since we don't have a withdraw function in this use-case.


## Test the contract

In the Deploy tab in Remix, deploy the contract to your local Ganache chain by connecting to Injected Web3 and ensuring MetaMask is pointed to localhost:8545.
You will need to fill in the constructor parameters with your designated employee addresses.
Test the deposit function by sending various values. Keep an eye on the employee balances as you send different amounts of Ether to the contract and ensure the logic is executing properly.

![alt text](http://url/to/img.png)
![alt text](http://url/to/img.png)
![alt text](http://url/to/img.png)


## Level Two: The TieredProfitSplitter Contract

In this contract, rather than splitting the profits between Associate-level employees, you will calculate rudimentary percentages for different tiers of employees (CEO, CTO, and Bob).
Using the starter code, within the deposit function, perform the following:


1. Calculate the number of points/units by dividing msg.value by 100.

This will allow us to multiply the points with a number representing a percentage. For example, points * 60 will output a number that is ~60% of the msg.value.



2. The uint amount variable will be used to store the amount to send each employee temporarily. For each employee, set the amount to equal the number of points multiplied by the percentage (say, 60 for 60%).


3. After calculating the amount for the first employee, add the amount to the total to keep a running total of how much of the msg.value we are distributing so far.


4. Then, transfer the amount to employee_one. Repeat the steps for each employee, setting the amount to equal the points multiplied by their given percentage.


### For example, each transfer should look something like the following for each employee, until after transferring to the third employee:


Step 1: amount = points * 60;


1. For employee_one, distribute points * 60.


2. For employee_two, distribute points * 25.


3. For employee_three, distribute points * 15.




Step 2: total += amount;


Step 3: employee_one.transfer(amount);




5. Send the remainder to the employee with the highest percentage by subtracting total from msg.value, and sending that to an employee.


6. Deploy and test the contract functionality by depositing various Ether values (greater than 100 wei).


7. The provided balance function can be used as a test to see if the logic you have in the deposit function is valid. Since all of the Ether should be transferred to employees, this function should always return 0, since the contract should never store Ether itself.


**Note: The 100 wei threshold is due to the way we calculate the points. If we send less than 100 wei, for example, 80 wei, points would equal 0 because 80 / 100 equals 0 because the remainder is discarded. We will learn more advanced arbitrary precision division later in the course. In this case, we can disregard the threshold as 100 wei is a significantly smaller value than the Ether or Gwei units that are far more commonly used in the real world (most people aren't sending less than a penny's worth of Ether).**

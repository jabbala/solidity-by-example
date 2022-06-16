# Array 
Array can have a compile-time fixed size or a dynamic size.

```solidity
pragme solidity ^0.8.13;

contract Array{
    // Several ways to initialize an array
    uint[] public arr;
    uint[] public arr2 = [1, 2, 3];
    // Fixed sized array, all elements initialize to 0
    uint[10] public myFixedSizeArr;
    function get(uint i) public view returns (uint) {
        return arr[i];
    }
    
    // Solidity can return the entire array.
    // But this function should be avoided for 
    // arrays that can grow indefinitely in length.
    function getArr() public view returns (uint[] memory) {
        return arr;
    }
    
    function push(uint i) public {
        // Append to array
        // This will increase the array length by 1. 
        arr.push(i);
    }
    
    function pop() public {
        // Remove last element from array
        // this will decrease the array length by 1
        arr.pop();
    }
    
    function getLength() public view returns (uint) {
        return arr.length;
    }
    
    function remove(uint index) public {
        // Delete does not change the array length
        // it resets the value at index to it's default value
        // in this case 0.
        delete arr[index];
    }
    
    function example() external {
        // create array in memory, only fixed size can be created.
        uint[] memory a = new uint[](5);
    }
}
```
## Example of removing array element
Remove array element by shifting elements from right to left
```solidity 
pragma solidity ^0.8.13;
contract ArrayRemoveByShifting {
    // [1, 2, 3] -- remove(1) --> [1, 3, 3] --> [1, 3]
    // [1, 2, 3, 4, 5, 6] -- remove(2) --> [1, 2, 4, 5, 6, 6] --> [1, 2, 4, 5, 6]
    // [1, 2, 3, 4, 5, 6] -- remove(0) --> [2, 3, 4, 5, 6, 6] --> [2, 3, 4, 5, 6]
    // [1] -- remove(0) --> [1] --> []
    uint[] public arr;
    function remove(uint _index) public {
        require(_index < arr.length, "index out of bound");
        for(uint i = _index; i < arr.length; i++){
            arr[i] = arr[i+1];
        }
        arr.pop();
    }
    
    function test() external {
        arr = [1, 2, 3, 4, 5];
        remove(2);
        assert(arr[0] == 1);
        assert(arr[0] == 2);
        assert(arr[0] == 4);
        assert(arr[0] == 5);
        assert(arr.length == 4);
        
        arr = [1];
        remove(0);
        // []
        assert(arr.length == 0);
    }
}
```

## Remove array element by copying the last element into the place to remove

```solidity
pragma solidity ^0.8.13;

contract ArrayReplaceFromEnd {
    uint[] public arr;
    // Deleting an element creates a gap in the array.
    // One trick to keep the array compact is to 
    // move the last element into the place to delete.
    function remove(uint index) public {
        arr[index] = arr[arr.length - 1];
        arr.pop();
    }
    function test() public {
        arr = [1, 2, 3, 4];
        remove(1);
        // [1, 4, 3]
        assert(arr.length==3);
        assert(arr[0] == 1);
        assert(arr[1] == 4);
        assert(arr[2] == 3);
        
        remove(2);
        assert(arr.length == 2);
        assert(arr[0] == 1);
        assert(arr[1] == 4);
    }
}
```

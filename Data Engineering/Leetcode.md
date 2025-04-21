Resources: 

https://neetcode.io/practice 

### Neetcode 150

#### Two Pointers 

##### Two Sum II Input Array Is Sorted

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        for i in range(len(numbers)):
            current_sum = numbers[left] + numbers[right]
            if current_sum == target:
                return [left + 1, right + 1]
            elif current_sum < target:
                left += 1
            elif current_sum > target:
                right -= 1
```

Logic:
- Given that the array is sorted we can use that to our advantage.
- This is a two pointer problem so we can start with a point at each end that gives us the largest and smallest values 
- We then check the sum of each value. If this value is too small then we will need to move the left pointer up by one because we need to increase our current sum and the same logic applies if the value is larger than the target.







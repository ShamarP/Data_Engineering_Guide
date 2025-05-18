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

##### container with most water

```python 
class Solution:
    def maxArea(self, height: List[int]) -> int:
        area = 0
        length = len(height)
        l = 0
        r = length - 1

        while l < r:
            curr_area = min(height[l],height[r]) * (r - l)
            if curr_area > area:
                area = curr_area
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
            
        return area

```
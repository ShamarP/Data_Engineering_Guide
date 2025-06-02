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


### SQL 50 

##### Recyclable and low fat products

```sql
SELECT product_id

FROM Products

WHERE low_fats = 'Y' AND recyclable = 'Y'
```

##### Find Customer Referee

**input:**

Table customer  

**columns:**
		1. id
		2. name
		3. referee_id

**output:** Table with all customers that are not referred by referee id 2

thoughts: basic problem that familiarizes you with sql syntax

```sql
SELECT name

FROM Customer

WHERE referee_id != 2 OR referee_id IS null

-- COALESCE(referee_id,0) != 2
```

learned that if a row is null it will be dropped when checking so I added that is null case. But a better way would be to coalesce the column with 0 and then compare.

##### Big Countries

**input:** 

Table Customer

columns: 
		1. name
		2. continent 
		3. area 
		4. population
		5. gdp 

**output:**

A table with name, population and area. Where it is filtered down to all countries that are big. Where big countries have an area of 3000000 $km^2$  or population of 25 million

```sql 
SELECT name,

population,

area

FROM World

WHERE population >= 25000000 OR area >= 3000000
```

##### Article Views

**Input:**

Table Views

columns:
		1. article_id
		2. author_id
		3. viewer_id 
		4. view_date

**Output:** 

a table with the ids of all authors who have viewed their own articles at least once 

```sql 
SELECT DISTINCT author_id AS id

FROM Views

WHERE author_id = viewer_id
```


# Search Algorithms

### Binary Search
+ Uses a *divide and conquer* design pattern: recursively breaking down a problem into two or more sub-problems of the same or related type until they become simple enough to be solved directly. Solutions of the sub-problems are then combined to give a solution to the original problem.

+ Based on a sorted array, binary search first determines the midpoint of the array, and then compares what we are searching for to the midpoint. Depending on whether our search is higher or lower than the midpoint, we can effectively ignore the other half of the array. This process is repeated until we either find what our search or determine that out search is not in the array.

+ Binary search is efficient, because it reduces the number of iterations required to search for what you are looking for.

**Implementation**

``` ruby
Example:

#Search for 'Goose' in entries

entries = ['Albert', 'Bob', 'Candice', 'Doh', 'Einhardt', 'Fenrir', 'Goose', 'Honk']


def binary_search(name)
  lower = 0
  upper = entries.length - 1

  while lower <= upper
    mid = (lower + upper) / 2
    mid_name = entries[mid]

    if name == mid_name
      return entries[mid]
    elsif name < mid_name
      upper = mid - 1
    elsif name > mid_name
      lower = mid + 1
    end
  end

  return nil
end

#1st iteration
lower = 0
upper = entries.length - 1 = 8 - 1 = 7
mid = (0 + 7) / 2 = 3
mid_name = entries[mid] = entries[3] = 'Doh'
name > mid_name => 'Goose' > 'Doh' => lower = mid + 1 = 3 + 1 = 4

#2nd iteration
lower = 4
upper = 7
mid = (4 + 7) / 2 = 5
mid_name = entries[mid] = entries[5] = 'Fenrir'
name > mid_name => 'Goose' > 'Fenrir' => lower = mid + 1 = 5 + 1 = 6

#3rd iteration
lower = 6
upper = 7
mid = (6 + 7) / 2 = 6
mid_name = entries[mid] = entries[6] = 'Goose'
name == mid_name ==> 'Goose' found

```

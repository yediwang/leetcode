# Binary Search

```text
int start = 0, end = nums.length - 1;
while(start + 1 < end) {
    int mid = (end - start) / 2 + start;
    if (target == nums[mid]) {
        end = mid;
    } else if (target < nums[mid]) {
        end = mid;
    } else {
        start = mid;
    }
}
    
// double check
if (nums[start] == target) {
    return start;
}

if (nums[end] == target) {
    return end;
}
```


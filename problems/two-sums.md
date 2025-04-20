ID: L1
Latest reference point: L1.6

### L1.1 Naive solution

Inspect every pair of numbers, and return when they add up to `target`. 

```c++
    vector<int> twoSum(const vector<int>& nums, int target) {
        for(int i=0; i<nums.size(); ++i) {
            for(int j=i+1; j<nums.size(); ++j) {
                if(target - nums[i] == nums[j]) return {i, j};
            }
        }
        return {};
    }
```

L1.2 Addition is commutative, so we can inspect only the upper triangle of the matrix (hence `j=i+1`).

<img src="https://github.com/user-attachments/assets/a551af34-9d55-4cb3-a718-3320e6f52006" width="150">

### L1.3 Using hash

L1.4 We know exactly what to look for in the nested loop of L1.1 - it's complement `target - nums[i]`.

So instead of iteration search, we can precalculate an inverted index of nums into unordered_map (for amortized O(1)), and then search for complement in it.

L1.6 One non-obvious case is when nums has repeated elements, like `[3, 3]` from examples. Only the last occurance ends up in `inv`, {3: 1}. Then in the second loop, we inspect i=0 first, and then find complement in `inv` with i=1. 
The code is correct, but this hidden behaviour may be problematic when modifying code in future. L1.5 eliminates this behaviour.

```c++
    vector<int> twoSum(const vector<int>& nums, int target) {
        unordered_map<int, int> inv;
        for(int i=0; i<nums.size(); ++i) {
            inv[nums[i]] = i;
        }
        for(int i=0; i<nums.size(); ++i) {
            int complement = target - nums[i];
            if(inv.find(complement) != inv.end() && i != inv[complement]) {
                return {i, inv[complement]};
            }
        }
        return {};
    }
```

### L1.5 Using hash, single pass

Using L1.2, we can search in `inv` and populate it in the same loop.

Note that there's no need to check for equal indexes, as when we inspect i-th item, it's not yet in `inv`.

```c++
    vector<int> twoSum(const vector<int>& nums, int target) {
        unordered_map<int, int> inv;
        for(int i=0; i<nums.size(); ++i) {
            int complement = target - nums[i];
            if(inv.find(complement) != inv.end()) {
                return {i, inv[complement]};
            }
            inv[nums[i]] = i;
        }
        return {};
    }
```

### Appendix 1. All code.
```c++
class Solution {
public:
    vector<int> twoSumNaive(const vector<int>& nums, int target) {
        for(int i=0; i<nums.size(); ++i) {
            for(int j=i+1; j<nums.size(); ++j) {
                if(target - nums[i] == nums[j]) return {i, j};
            }
        }
        return {};
    }

    vector<int> twoSumHash(const vector<int>& nums, int target) {
        unordered_map<int, int> inv;
        for(int i=0; i<nums.size(); ++i) {
            inv[nums[i]] = i;
        }
        for(int i=0; i<nums.size(); ++i) {
            int complement = target - nums[i];
            if(inv.find(complement) != inv.end()) {
                if (i == inv[complement]) continue;
                return {i, inv[complement]};
            }
        }
        return {};
    }

    vector<int> twoSum(const vector<int>& nums, int target) {
        unordered_map<int, int> inv;
        for(int i=0; i<nums.size(); ++i) {
            int complement = target - nums[i];
            if(inv.find(complement) != inv.end()) {
                return {i, inv[complement]};
            }
            inv[nums[i]] = i;
        }
        return {};
    }
};
```

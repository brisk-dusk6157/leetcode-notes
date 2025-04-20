

```c++
class Solution {
public:
    vector<int> twoSumNaive(const vector<int>& nums, int target) {
        for(int i=0; i<nums.size(); i++) {
            for(int j=i+1; j<nums.size(); j++) {
                if(target - nums[i] == nums[j]) return {i, j};
            }
        }
        return {};
    }

    vector<int> twoSumHash(const vector<int>& nums, int target) {
        unordered_map<int, int> inv;
        for(int i=0; i<nums.size(); i++) {
            inv[nums[i]] = i;
        }
        for(int i=0; i<nums.size(); i++) {
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
        for(int i=0; i<nums.size(); i++) {
            int complement = target - nums[i];
            if(inv.find(complement) != inv.end()) {
                return {i, inv[complement]};
            }
            inv[nums[i]] = i;
        }
        return {};
    }
};
``

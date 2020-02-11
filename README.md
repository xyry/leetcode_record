# leetcode_record
 leetcode做题记录



## 2020年2月11日

#### [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

一开始只想到$O(n^2)$的算法，TLE，然后一直在想如何优化。

答案用的双指针，$O(n)$的复杂度。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int l=0,r=numbers.size()-1;
        while(l<r){
            if(numbers[l]+numbers[r]>target){
                r--;
            }else if(numbers[l]+numbers[r]<target){
                l++;
            }else if(numbers[l]+numbers[r]==target){
                break;
            }
        }
        return vector<int>{l+1,r+1};
    }
};
```

#### [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)

自己写的方法T了，看了下评论区，有个讲的蛮不错的。

引用

>  思路：此题可以采用头插法，一个一个的移动。但是有种更加简单的选择数组的方式。我们可以采用翻转的方式，比如12345经过翻转就变成了54321，这样已经做到了把前面的数字放到后面去，但是还没有完全达到我们的要求，比如，我们只需要把12放在后面去，目标数组就是34512，与54321对比发现我们就只需要在把分界线前后数组再进行翻转一次就可得到目标数组了。所以此题只需要采取三次翻转的方式就可以得到目标数组，首先翻转分界线前后数组，再整体翻转一次即可。此题面试常考，大家可以记一下此方法。 

```c++
class Solution {
public:
    void reverse(vector<int>& nums,int begin,int end){
        int tmp;
        while(begin<end){
            tmp=nums[end];
            nums[end]=nums[begin];
            nums[begin]=tmp;
            begin++;
            end--;
        }
    }

    void rotate(vector<int>& nums, int k) {
        int len=nums.size();
        if(len<2) return;
        k=k%len;
        reverse(nums,0,len-1);
        reverse(nums,0,k-1);
        reverse(nums,k,len-1);
    }
};
```


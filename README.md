# leetcode_record
 leetcode做题记录

目前做的是这个数组系列， https://leetcode-cn.com/tag/array/  进去后，难度选择简单，从第一个开始做。

闲着没事的时候做一下，免得假期太闲了。

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

## 2020年2月12日

#### [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

思路：我的想法是用set数据结构，从nums数组往set里面传入，每传入一个数值，检测当前的索引$i$与st.size()大小比较，如果st.size()$<=i$ 那么证明，nums数组中出现了重复的数值。

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int len=nums.size();
        bool ans=false;
        set<int> st;
        for(int i=0;i<len;i++){
            st.insert(nums[i]);
            int len2=st.size();
            if(len2<=i){
                ans=true;
                break;
            }
        }
        return ans;
    }
};
```

## 2020年2月14日

#### [219. 存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)

思路：利用map映射 线性查找会超时,java用哈希查找

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        map<int,int> mp;
        for(int i=0;i<nums.size();i++){
            if(mp.count(nums[i])&&i-mp[nums[i]]<=k) return true;
            mp[nums[i]]=i;
        }
        return false;
    }
};
```

#### [268. 缺失数字](https://leetcode-cn.com/problems/missing-number/)

水

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int len=nums.size();
        long long sum=(1+len)*len/2;
        long long sum2=0;
        for(int i=0;i<len;i++){
            sum2+=nums[i];
        }
        return int(sum-sum2);
    }
};
```

# 2020年2月19日

#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

思路：类似于冒泡

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int len=nums.size();
        if(len==1) return;
        int cnt;
        do{
            cnt=0;
            for(int i=0;i<len-1;i++){
                int a=nums[i];
                int b=nums[i+1];
                if(a==0&&b!=0){
                    nums[i]=b;
                    nums[i+1]=a;
                    cnt++;
                }else{
                    continue;
                }
            }
        }while(cnt>0);
        
    }
};
```

# 2020年2月23日

#### [414. 第三大的数](https://leetcode-cn.com/problems/third-maximum-number/)

思路：我遇到的问题就是不知道怎么设置一个很小很小的数作为-INF来初始化fmax,smax,tmax,后来查资料发现有一种写法 LONG_MIN LONG_MAX。顾名思义，long long的上下界 

```c++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        int len=nums.size();
        long long inf=LONG_MAX;
        // 如何表示最小值呢,都越界了
        if(len==1) return nums[0];
        else if(len==2) return max(nums[0],nums[1]);
        else{
            long long  fmax=-inf;
            long long  smax=-inf;
            long long  tmax=-inf;
            for(int i=0;i<len;i++){
                if(nums[i]<tmax) continue;
                else if(nums[i]>=fmax){
                    if(nums[i]==fmax) continue;
                    if(fmax==-inf){
                        fmax=nums[i];
                    }else{
                        int tmp=fmax;
                        fmax=nums[i];
                        if(smax==-inf){
                            smax=tmp;
                        }else{
                            int tmp2=smax;
                            smax=tmp;
                            tmax=tmp2;
                        }
                    }
                }else if(nums[i]>=smax){
                    if(nums[i]==smax) continue;
                    if(smax==-inf){
                        smax=nums[i];
                    }else{
                        int tmp2=smax;
                        smax=nums[i];
                        tmax=tmp2;
                    }
                }else if(nums[i]>=tmax){
                    if(nums[i]==tmax) continue;
                    tmax=nums[i];
                }
            }
            if(tmax==-inf) return fmax;
            else return tmax;
        }
    }
};
```

#### [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

思路：就是利用set去掉重复元素，然后遍历set寻找和index不匹配的即可。重新复习了set的遍历。

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
            int len=nums.size();
            set<int> st(nums.begin(),nums.end());
            vector<int> ans;
            int index=1;
            for(set<int>::iterator i=st.begin();i!=st.end();i++){
                while(*i!=index){
                    ans.push_back(index);
                    index++;
                    if(index>len) break;
                }
                if(*i==index){
                    index++;
                }
            }
            for(int i=index;i<=len;i++) ans.push_back(index++);
            return ans;
    }
};
```


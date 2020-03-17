[TOC]

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

#### [485. 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

水题

```
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int len=nums.size();
        int cnt=0;
        int ans=0;
        for(int i=0;i<len;i++){
            if(nums[i]==1){
                cnt++;
            }else{
                ans=max(ans,cnt);
                cnt=0;
            }
        }
        ans=max(ans,cnt);
        return ans;
    }
};
```

#### [532. 数组中的K-diff数对](https://leetcode-cn.com/problems/k-diff-pairs-in-an-array/)

思路：WA了三次，第一次没考虑k=0;第二次和第三次都是特判K=0的时候想简单了，最后把K=0列出来单独处理，记录放进st2中，st2.size()不发生改变的数字，将这些数字放进st3中，最后记一下st3的大小就行

顺便学会了set中有find方法，返回找到数字的指针，找不到返回st.end()。

vector中没有find

```c++
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {
        int len=nums.size();
        set<int> st(nums.begin(),nums.end());
        int ans=0;
        if(k==0){
            set<int> st2;
            set<int> st3;
            // 把重复的数字放进st3,算st3.size()就知道有几种重复的数字了
            int presize=st2.size();
            for(int i=0;i<len;i++){
                st2.insert(nums[i]);
                if(st2.size()!=presize){
                    presize=st2.size();
                    continue;
                }
                st3.insert(nums[i]);
                presize=st2.size();
            }
            ans=st3.size();
        }else if(k<0) 
            return 0;
        else{
            for(set<int>::iterator it=st.begin();it!=st.end();it++){
                set<int>::iterator f=st.find(*it+k);
                if(f!=st.end()&&f!=it) ans++;
            }
        }
        
        return ans;
    }
};
```

# 2020年2月24日

#### [561. 数组拆分 I](https://leetcode-cn.com/problems/array-partition-i/)

思路：水题，反向思考，如果要求1到n的$min(a_i,b_i)$总和最小的话，那操作就是把最小的前n个数分别放进每一对中，这样求出来的和就是最小的。如果要求总和最大，直接排个序，然后取索引为偶数的即可。也可以直接算。

```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        int len=nums.size();
        sort(nums.begin(),nums.end());
        int ans=0;
        for(int i=0;i<len;i+=2){
            ans+=min(nums[i],nums[i+1]);
        }
        return ans;
    }
};
```

#### [605. 种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

思路：进行各种特判

- 中间出现3个0可以种一朵花，5个0可以种两朵花，x个0可以种$(x-3)/2+1$朵花

- 端点特判，全是0（左右端点都是0）；左端点是0；右端点是0

  写的有点麻烦，WA了3次

优秀的解法： **防御式编程思想：在 flowerbed 数组两端各增加一个 0， 这样处理的好处在于不用考虑边界条件，任意位置处只要连续出现三个 0 就可以栽上一棵花。** 

```c++
丑陋的代码
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int len=flowerbed.size();
        int cnt=0;
        vector<int> vec;
        for(int i=0;i<len;i++){
            if(flowerbed[i]==0){
                cnt++;
            }else if(flowerbed[i]==1&&cnt!=0){
                vec.push_back(cnt);
                cnt=0;
            }
        }
        if(cnt!=0) vec.push_back(cnt);
        int sum=0;
        for(int i=0;i<vec.size();i++){
            int tmp;
            if(vec.size()==1&&vec[0]==len){
                if(len%2==0){
                    sum+=len/2;
                }else{
                    sum+=(len+1)/2;
                }
            }
            else if(i==0&&flowerbed[0]==0||i==vec.size()-1&&flowerbed[len-1]==0){
                sum+=vec[i]/2;
            }else if(vec[i]<3){
                continue;
            }else if(vec[i]%2==0){
                tmp=vec[i]/3;
                sum+=tmp;
            }else{
                tmp=(vec[i]-3)/2+1;
                sum+=tmp;
            }        
        }
        if(sum>=n) return true;
        else return false;
    }
};
```

优雅的解法，顺便学习了auto的用法

```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int cnt=1,ans=0;//左边补零
        for(auto i:flowerbed){
            if(i){
                ans+=(cnt-1)/2;
                cnt=0;
            }else{
                cnt++;
            }
        }
        // 处理最后的右端点以及右端点补零
        ans+=cnt/2;
        return ans>=n;
    }
};

```


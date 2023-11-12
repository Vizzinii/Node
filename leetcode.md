#  壹 202.快乐数

 题目：编写一个算法来判断一个数 n 是不是快乐数。

 「快乐数」 定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果这个过程 结果为 1，那么这个数就是快乐数。如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

 代码随想录思路：题目中说了会 无限循环，那么也就是说求和的过程中，sum会重复出现，这对解题很重要！当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法了。所以这道题目使用哈希法，来判断这个sum是否重复出现，如果重复了就是return false， 否则一直找到sum为1为止。判断sum是否重复出现就可以使用unordered_set。还有一个难点就是求和的过程，如果对取数值各个位上的单数操作不熟悉的话，做这道题也会比较艰难。

### AC代码：
 ```cpp
class Solution 
{
public:
//将该数替换为每个位置上数字的平方和
int getsum(int n)
{
    int sum=0;
    while(n)
    {
        sum+=(n%10)*(n%10);
        n/=10;
    }
    return sum;
}
bool isHappy(int n)
 {
     unordered_set<int> Set;
     while(1)
     {
         n=getsum(n);
         if(n==1)
         {
             return true;
         }
         if(Set.find(n)!=Set.end())
         {
             return false;
         }
         else
         {
            Set.insert(n);
         }
     };
 }
};
```
# 贰 242. 有效的字母异位词

题目：给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

示例 1:
输入: s = "anagram", t = "nagaram"
输出: true
示例 2:
输入: s = "rat", t = "car"
输出: false

思路：
首先直接两层嵌套for循环可以做出来，但是时间是O(n^2)，还可以简化。
**看到判断是否存在，要直接联想到使用哈希。**
但是题目量级较小，使用set集合比较浪费，而数组也是一个简单哈希表。因此这里想到：**可以使用一个数组，来记录每个字母出现的次数。**
而字母只有26个，因此可以使用一个大小为26的数组来解题。
需要把字符映射到数组也就是哈希表的索引下标上，因为字符a到字符z的ASCII是26个连续的数值，所以字符a映射为下标0，相应的字符z映射为下标25。
再遍历 字符串s的时候，只需要将 s[i] - ‘a’ 所在的元素做+1 操作即可，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了。 这样就将字符串s中字符出现的次数，统计出来了。
看一下如何检查字符串t中是否出现了这些字符，同样在遍历字符串t的时候，对t中出现的字符映射哈希表索引上的数值再做-1的操作。
那么最后检查一下，record数组如果有的元素不为零0，说明字符串s和t一定是谁多了字符或者谁少了字符，return false。
最后如果record数组所有元素都为零0，说明字符串s和t是字母异位词，return true。
时间复杂度为O(n)，空间上因为定义是的一个常量大小的辅助数组，所以空间复杂度为O(1)。

### AC代码如下
```cpp
class Solution 
{
public:
    bool isAnagram(string s, string t) 
    {
        if(s.size()!=t.size())
        {
            return false;
        }
        int test[26]={0};
        for(int i=0;i<s.size();i++)
        {
            test[s[i]-'a']++;
        }
        for(int i=0;i<t.size();i++)
        {
            test[t[i]-'a']--;
        }
        for(int i=0;i<26;i++)
        {
            if(test[i]!=0)
            {
                return false;
            }
        }
        return true;
    }
};
```
# 叁 349.两个数组的交集

题目：给定两个数组 nums1 和 nums2 ，返回它们的交集 。输出结果中的每个元素一定是**唯一**的。我们可以 不考虑输出结果的顺序 。

思路：设立两个集合set1 和 set2 。set1 用来遍历数组 nums1 ，不重复地得到数组 numns1 中所有的元素。之后把 nums2 中的每个元素与 set1 中已有的元素对比，若是在 nums2 中出现 set1 也就是 nums1 中拥有的元素，则把这个元素加入到 set2 中。这样同样可以得到元素不重复的集合 set2 ，循环遍历结束后， set2 中所有的元素就是数组 nums1 和 nums2 共有的元素。最后用迭代器把 set2 中所有的元素加入到 vector 中并返回，就得到最终结果。
代码随想录思路：输出结果中的每个元素一定是唯一的，也就是说输出的结果的去重的， 同时可以不考虑输出结果的顺序。这道题用暴力的解法时间复杂度是O(n^2)，那来看看使用哈希法进一步优化。要注意，**使用数组来做哈希的题目，是因为题目都限制了数值的大小**。而这道题目没有限制数值的大小，就无法使用数组来做哈希表了。此时就要使用另一种结构体 set 了。

### AC代码如下
```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) 
    {
        //vector<int> sum;
        unordered_set<int> temp;
        unordered_set<int> tempe;
        for(int i=0;i<nums1.size();i++)
        {
            if(temp.find(nums1[i])==temp.end())
            {
                temp.insert(nums1[i]);
            }
        }
        for(int i=0;i<nums2.size();i++)
        {
            if(temp.find(nums2[i])!=temp.end())
            {
                tempe.insert(nums2[i]);
                //sum.push_back(nums2[i]);
            }
        }
        return vector<int> (tempe.begin(),tempe.end());
    }
};
```
# 肆 383. 赎金信

题目：给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。如果可以，返回 true ；否则返回 false 。magazine 中的每个字符只能在 ransomNote 中使用一次。

思路：本题与昨晚的“有效的字母异位词“相似，不同的是”有效的字母异位词“是检测两个字符串是否每个字母的数量都相等，而本题是检测字符串1能否由字符串2的部分字母组成得到。我的解法是先遍历字符串 magazine ，用哈希算法把每个字母出现的次数存储在数组中，之后遍历字符串 ransomNote ，ransomNote 中每个字母都会消耗一个 magazine所得数组中的字母。最后对数组进行检查，如果每一个数都大于等于零，即 magazine 中的字母足够组成 ransomNote ；若存在部分数小于零，则说明 ransomNote 不能完全由 magazine 得到。

代码随想录思路：因为题目说只有小写字母，那可以采用空间换取时间的哈希策略，用一个长度为26的数组来记录magazine里字母出现的次数。然后再用ransomNote去验证这个数组是否包含了ransomNote所需要的所有字母。依然是数组在哈希法中的应用。

### AC代码如下
```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine)
    {
        int note[26]={0};
        for(int i=0;i<magazine.size();i++)
        {
            note[magazine[i]-'a']++;
        }
        for(int i=0;i<ransomNote.size();i++)
        {
            note[ransomNote[i]-'a']--;
        }
        for(int i=0;i<26;i++)
        {
            if(note[i]<0)
            {
                return false;
            }
        }
        return true;
    }
};
```
# 伍 1. 两数之和
题目：给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。你可以按任意顺序返回答案。

思路：本题需求是得到和为某个数的两个数，较好的解法是遍历数组，每次根据当前遍历到的元素与目标那个和，相减得到一个数并判断这个数是否存在于已经遍历的数组部分中。存在则直接得到结果，不存在则把当前遍历到的元素加入到已经遍历的部分中。
本题需要存储和寻找到整数及其下标，而且需要判断一个元素是否在某个集合中，因此要用到可以存储两个关联元素且底层是通过哈希实现的的 map 。而因为是要根据元素值得到下标，因此元素值存成查找快的 key ,下标存为 value 。
### AC代码
``` cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        unordered_map<int,int> map;
        for(int i=0;i<nums.size();i++)
        {
            int temp=target-nums[i];
            if(map.find(temp)!=map.end())
            {
                return {map[temp],i};
            }
            map.insert(pair<int,int>(nums[i],i));
        }
        return {};
    }
};
```
# 陆 超过经理收入的员工

题目：id 是表 Employee 的主键（具有唯一值的列）。该表的每一行都表示雇员的ID、姓名、工资和经理的ID。
编写解决方案，找出收入比经理高的员工。以 任意顺序 返回结果表。

思路：解题有两步，第一步是找一个主键是否有"经理"，第二步是判断该主键的"薪水"是否高于其"经理"的主键值的"薪水"。SQL一次不能在查询到结果后返回去再次查询这张表。因此这里使用自连接，理解为调用的两张表都是原 Employee 表的引用。

### AC代码：

```sql
select e1.name as Employee
from Employee as e1, Employee as e2
where e1.salary > e2.salary
and e1.managerID = e2.id
```
# 柒 组合两个表

 题目：personId 是表 Person 的主键（具有唯一值的列）。该表包含一些人的 ID 和他们的姓和名的信息。addressId 是表 Address 的主键（具有唯一值的列）。该表的每一行都包含一个 ID = PersonId 的人的城市和州的信息。编写解决方案，报告 Person 表中每个人的姓、名、城市和州。如果 personId 的地址不在 Address 表中，则报告为 null 。以 任意顺序 返回结果表。

 思路：两表通过左连接进行联合查询，条件是表 Person 的 ID 等于表 Address 的 PersonID 。

 ### AC代码：
 ```sql
 select lastName,firstName,city,state 
 from Person  left join Address 
 on Person.personID=Address.personID;
 ```
 # 捌 从不订购的客户

 题目：id 是表 Customer 的主键。该表的每一行都表示客户的 ID 和名称。在 SQL 中，id 是表 Orders 的主键。customerId 是 Customers 表中 ID 的外键( Pandas 中的连接键)。该表的每一行都表示订单的 ID 和订购该订单的客户的 ID。找出所有从不点任何东西的顾客。以 任意顺序 返回结果表。

 思路：找出表 Orders 中所有出现过的 customerId（即订购过的客户的customerId值）得到一个列表，再查询表 Customer 中主键 id 值不在已得列表中的行的 name 。

### AC代码：
 ```sql
select Customers.name as 'Customers'
from Customers 
where Customers.id not in
(
  select customerId from Orders
)
```
# 玖 182.查找重复的电子邮箱

id 是表 Person 的主键（具有唯一值的列）。此表的每一行都包含一封电子邮件。电子邮件不包含大写字母.编写解决方案来报告所有重复的电子邮件。 请注意，可以保证电子邮件字段不为 NULL。

思路：根据每个不同的 email 来分组并求其 count() ,再查找 count() 大于1的 email 。

### AC代码
```sql
select email as Email
from Person
group by email
having count(email)>1
```
# 拾 577.员工奖金

题目：empId 是表 Employee 中具有唯一值的列。该表的每一行都表示员工的姓名和 id，以及他们的工资和经理的 id。
empId 是表 Bonus 具有唯一值的列。empId 是 Employee 表中 empId 的外键(reference 列)。该表的每一行都包含一个员工的 id 和他们各自的奖金。
编写解决方案，报告每个奖金 少于 1000 的员工的姓名和奖金数额。
以 任意顺序 返回结果表。

思路：注意到示例中给出的暗示，要查询出奖金低于1000的员工，包含奖金为0。首先要查询得到每位员工的奖金（使用外连接，以处理员工没有出现在 Bonus 表上的情况），再用 bonus 小于1000或等于null作为判断条件进行where筛选即可。注意是**bonus is null**而不是 bonus = null 。

### AC代码
```sql
select name , bonus
from Employee left join Bonus 
on Employee.empId = Bonus.empId
having bonus < 1000 or bonus is null
```
# 拾壹 1047.删除字符串中的所有相邻重复项

题目：给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。在 S 上反复执行重复项删除操作，直到无法继续删除。在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

思路：此题是不停地在前一次删除的基础上，删除现有的最靠前的相邻的连续字母。需要想到可能出现“必须先删除已有的连续字母，才能出现并删除随之产生的连续字母”的情况，如“abba”。因此这是一个遍历字符串，并对已遍历部分进行先进先出存储和读取操作的问题，自然而然想到需要使用栈来解决。若是当前遍历到的元素与栈顶元素相同，则栈顶元素出栈；若是不同，则把当前遍历到的元素进行入栈操作。在c++中，string中就已经包含了一些stack的特性，直接将一个临时的 string 当作 stack 来用即可。

### AC代码如下
```cpp
class Solution {
public:
    string removeDuplicates(string s)
    {
        string st;
        for(int i=0;i<s.size();i++)
        {
            if(!st.empty() && st.back()==s[i])
            {
                st.pop_back();
            }
            else
            {
                st.push_back(s[i]);
            }
        }
        return st;
    }
};
```
# 拾贰 20.有效的括号

题目：给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
每个右括号都有一个对应的相同类型的左括号。

思路：左括号与右括号必须成对出现，且只能包含而不能交叉。因此我的想法是：遇到左括号就入栈；遇到右括号时，第一步先判断栈是否为空，若栈为空直接可判定结果为 false ，若此时栈不为空则对比栈顶元素与当前得到的元素是否是一对匹配的括号，如果成对则把栈顶元素出栈并继续遍历剩下的元素，如果不成对可直接判定为 false 。这样解决了先遇到右括号、括号域交叉等问题。

### AC代码
```cpp
class Solution {
public:
    bool isValid(string s) 
    {
        stack<char> st;
        for(int i=0;i<s.size();i++)
        {
            if(s[i]=='('||s[i]=='['||s[i]=='{')
            {
                st.push(s[i]);
            }
            else if(s[i]==')')
            {
                if(st.size()==0)
                {
                    return false;
                }
                if(st.top()!='(')
                {
                    return false;
                }
                st.pop();
            }
            else if(s[i]==']')
            {
                if(st.size()==0)
                {
                    return false;
                }
                if(st.top()!='[' || st.size()==0)
                {
                    return false;
                }
                st.pop();
            }
            else if(s[i]=='}')
            {
                if(st.size()==0)
                {
                    return false;
                }
                if(st.top()!='{' || st.size()==0)
                {
                    return false;
                }
                st.pop();
            }
        }
        if(!st.empty())
        {
            return false;
        }
        return true;
    }
};
```
# 拾叁 150.逆波兰表达式

题目：给你一个字符串数组 tokens ，表示一个根据 逆波兰表示法 表示的算术表达式。请你计算该表达式。返回一个表示表达式值的整数。
注意：
有效的算符为 '+'、'-'、'*' 和 '/' 。
每个操作数（运算对象）都可以是一个整数或者另一个表达式。
两个整数之间的除法总是 向零截断 。
表达式中不含除零运算。
输入是一个根据逆波兰表示法表示的算术表达式。
答案及所有中间计算结果可以用 32 位 整数表示。
示例：
tokens = ["2","1","+","3","*"]输出为9
tokens = ["4","13","5","/","+"]输出为6

思路：后缀表达式是栈的典型运用之一。解法是设立一个栈，对输出的标准且正确的字符串 tokens 遍历，每次遇到数字就入栈，若遇到运算符号就弹出栈的最顶两个元素，使用运算符进行运算后，把结果作为新的结果压入栈中。

### AC代码：
```cpp
lass Solution {
public:
    int evalRPN(vector<string>& tokens) 
    {
        stack<long long> st;
        for(int i=0;i<tokens.size();i++)
        {
            if(tokens[i]=="+" || tokens[i]=="-"||tokens[i]=="*"||tokens[i]=="/")
            {
                long long i1=st.top();
                st.pop();
                long long i2=st.top();
                st.
                pop();
                long long temp=0;
                if(tokens[i]=="+")
                {
                    temp=i2+i1;
                }
                if(tokens[i]=="-")
                {
                    temp=i2-i1;
                }
                if(tokens[i]=="*")
                {
                    temp=i2*i1;
                }
                if(tokens[i]=="/")
                {
                    temp=i2/i1;
                }
                st.push(temp);
            }
            else
            {
                st.push(stoll(tokens[i]));
            }
        }
        int sum=st.top();
        st.pop();
        return sum;
    }
};
```
# 拾肆 239.滑动窗口最大值
题目：给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。返回 滑动窗口中的最大值 。
示例 1：
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]

第一次思路：创建两个 vector<int> ，一个 bianli 用来遍历整个已给数组 nums ，一个 juice 用来存放最后的结果。 vector 中每次只有三个元素，并且使用对 bianli 进行再遍历得到其内的最大值并返回和存入 juice 中。思路上没有问题，在数据规模小的时候也能ac代码。但在窗口的长度接近数组的长度且二者都较大时，就会因嵌套遍历及其O(k*n)的时间复杂度而导致超时。
### 第一次代码：
```cpp
    vector<int> maxSlidingWindow(vector<int>& nums, int k)
    {
        //保存每次结果的数组
        vector<int> juice;
        vector<int> bianli;
        int temp=0;
        for(int i=0;i<nums.size();i++)
        {
            if(i<k-1)
            {
                bianli.push_back(nums[i]);
            }
            else
            {
                bianli.push_back(nums[i]);
                int maxval=bianli[0];
                for(int j=0;j<k;j++)
                {
                    if(bianli[j]>maxval)
                    {
                        maxval=bianli[j];
                    }
                }
                juice.push_back(maxval);
                bianli.erase(bianli.begin());
            }
        }
        return juice;
    }
```

第二次思路：
实际上我们不需要在遍历的容器中存放所有的数据，因为题目需要的是最大值，因此我们只要在遍历的容器内存放“可能成为最大值”的元素即可。
我们想要容器内最左端的元素一直就是我们需要的此时窗口内的最大值。因此想到，我们可以设计出一个“对已给数组的部分元素排好序的容器”。
遍历到的一个元素准备从容器的右端进入时，倘若其左方的元素比它小，那么在这两个元素共同所处的一切窗口内，左边那个较小的元素永远不可能是最大值，因此此时可以把左方较小的元素从右端入口处踢出。这样循环下去，直到其左边的元素比它大或者容器变空了，此时再把遍历到的这个元素从右边进入容器中。而如果左方的元素比它大，那么直接不把它放入容器内。这样保证了容器内元素从左到右是递减的。
然而一个元素也有生命周期，这个周期就是包含它的窗口的个数。**一个元素要么没进容器，要么在容器中被右边较大的元素剔除掉，要么排到最左边成为最大值。**那么成为最大值后，如果没有元素来把它比下去，那么它什么时候在最左边离开容器呢？很显然，就是当这个窗口不再包含这个元素的时候，此时也正是这个元素在原数组中成为窗口左方相邻元素的时候。这个很容易在遍历的时候利用“此时遍历到的元素-窗口长度”得到。
以上剖析了元素的遍历、元素能否进入容器与何时进入容器、元素何时离开容器，而要能做到一端进两端出且先进先出，很容易想到双端队列。这里自己设计一个有特殊功能的队列，它可以判断元素是否进入队列、元素何时离开队列、返回队列最左端的最大值。
**一个值如果在队列中有在它左方的、比它大的元素。就说明较小的这个元素是比较大的元素慢进来的，而在较小和较大元素仍同处一个窗口的时候，又进来了一个比较小元素大的元素。由此可见，较小的元素要么和左方较大的元素同窗，要么和刚进来的较大的元素同窗，要么和两个都同窗，所以它永不可能在某个窗口中作为最大值，因此完全可以不用维护它，直接把它剔除掉。**
### AC代码
```cpp
class Solution {
private:
class myque
{
    public:
    deque<int> que;
    //弹出的条件是：iff此时窗口应该弹出的元素等于队前元素
    void pop(int val)
    {
        if(val==que.front() && !que.empty())
        {
            que.pop_front();
        }
    }
    //压入时：从队最前开始pop掉比应压元素小的，再压入
    void push(int val)
    {
        while(!que.empty() && val > que.back())
        {
            que.pop_back();
        }
        que.push_back(val);
    }
    int max()
    {
        return que.front();
    }
};
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k)
    {
        myque que;
        vector<int> juice;
        for(int i=0;i<k;i++)
        {
            que.push(nums[i]);
        }
        juice.push_back(que.max());
        for(int i=k;i<nums.size();i++)
        {
            que.pop(nums[i-k]);
            que.push(nums[i]);
            juice.push_back(que.max());
        }
        return juice;
    }
};
```
# 拾伍 3.无重复字符的最长子串
题目：给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

思路：仍是滑动窗口类题目，这类题一般设置两个指针来从左到右遍历整个字符串。本题中，**右指针右移的条件是子串中无重复字符，左指针右移的条件是子串中有重复字符。**本来一般做法是在符合左指针右移的条件时，把左指针一格一格向右移动。但这里采用一种省时的解法：由于右指针每次是右移一格，因此只要知道新进来的字符跟原子串内哪一个位置的字符重复就可以。**采用一个长 128 的 vector 来存放每次新进来子串的字符的位置。每次比较左指针与新增字符对应的下标的数字，可以得到新进的字符是否在原子串内有相同的字符，若有则把左指针直接移动到原子串内那个被重复的字符后边一位，此时新子串内就没有重复字符，最后再把当前子串的长度与已得的最长长度比较。**

### AC代码
```cpp
    int lengthOfLongestSubstring(string s) 
    {
        //左、右指针
        int left=0;
        int juice=0;
        //因为ASCII表内总共有128的字符
        vector<int> temp(128,0);
        for(int right=0;right<s.size();right++)
        {
            //在未更新 temp[s[right]] 时，把左指针移动到
            //即将被重复的字符的后面，即 temp[s[right]] 一直是某个元素后面位置的下标
            left=max(left,temp[s[right]]);
            //更新新增字符对应所存储的下标，即位置
            temp[s[right]]=right+1;
            //更新结果的最新长度
            juice=max(juice,right-left+1);
        }
        return juice;
    }
```
# 拾陆 219.存在重复元素

题目：给你一个整数数组 nums 和一个整数 k ，判断数组中是否存在两个 不同的索引 i 和 j ，满足 nums[i] == nums[j] 且 abs(i - j) <= k 。如果存在，返回 true ；否则，返回 false 。
示例：
输入：nums = [1,2,3,1], k = 3
输出：true

输入：nums = [1,0,1,1], k = 1
输出：true

输入：nums = [1,2,3,1,2,3], k = 2
输出：false

思路：与之前的“第五题：两数之和”相同，这里也同时需要存储数组的下标和数值。因此这里用一个 unordered_map ，把数组的数值作为 key ，把数组的下标作为 value 且全都初始化为0 。在向右遍历的时候，每次都把当前遍历到的元素的下标与已存放在 map 中的上一次的下标相比，若不满足 true 条件则更新当前遍历到的元素在 map 中的 value 值，若满足则直接返回 true 。

### AC代码
```cpp
    bool containsNearbyDuplicate(vector<int>& nums, int k) 
    {
        unordered_map<int,int> m;
        for(int right=0;right<nums.size();right++)
        {
           if(m.count(nums[right])>0)
           {
               if(right-m[nums[right]]<=k)
               {
                   return true;
               }
           } 
           m[nums[right]]=right;
        }
        return false;
    }
```
# 拾柒 977.有序数组的平方
题目：给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。
示例：
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]

思路：cpp有一个自带的 sort 函数，会自动根据情况调用合适的排序算法。因此排序不需要自己写。

### AC代码
```cpp
    vector<int> sortedSquares(vector<int>& nums) 
    {
        vector<int> juice;
        for(int num:nums)
        {
            juice.push_back(num*num);
        }
        sort(juice.begin(),juice.end());
        return juice;
    }
```
# 拾捌 209.长度最小的子数组
题目：给定一个含有 n 个正整数的数组和一个正整数 target 。找出该数组中满足其总和大于等于 target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

思路：这是一道经典的利用左右指针的窗口题目。右指针右移的条件是子数组的和小于(<) target ，而左指针右移的条件则是子数组的和大于等于(>=) target 。“和”在右指针右移时要加上当前遍历到的元素，在左指针右移时要减去刚刚离开窗口的元素数值。每次移动右指针都判断如何移动左指针，每次判断和移动左指针后都要比较当前窗口长度与已知最小长度。

### AC代码
```cpp
    int minSubArrayLen(int target, vector<int>& nums) 
    {
        int juice=INT32_MAX;
        int left=0;
        int sum=0;
        for(int right=0;right<nums.size();right++)
        {
            sum+=nums[right];
            while(sum>=target)
            {
                int temp=right-left+1;
                juice=temp>juice?juice:temp;
                sum-=nums[left++];
            }
        }
        return juice==INT32_MAX?0:juice;
    }
```
# 拾玖 203.移除链表元素
题目：给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。

思路：要注意的一点是，每次删除节点之后，新指向的节点也同样有可能符合删除的条件。因此要使用 while 而不是 if 来判定是否需要删除，直到指向的节点不再需要被删除为止，不应该用 if ，那样只删除了一次。

### AC代码
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) 
    {
        while(head!=NULL && head->val == val)
        {
            ListNode * temp = head;
            head = head->next;
            delete temp;
        };
        ListNode* gues = head;
        while(gues!=NULL && gues->next!=NULL)
        {
            if(gues->next->val == val && gues->next->next!=NULL)
            {
                ListNode* tem = gues->next;
                gues->next = gues->next->next;
                delete tem;
            }
            else if(gues->next->val == val && gues->next->next==NULL)
            {
                delete gues->next;
                gues->next = NULL;
            }
            else 
            {
               gues=gues->next; 
            }
        };
        return head;
    }
};
```

# 贰拾 206.反转链表
题目：给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

思路：设置两个指针，一个 **pre 指针指向当前节点的前一个节点A**，一个 **cur 指针指向当前节点B**。当 cur 非空时，将 **cur 复制给一个新指针 temp 指向节点B**，**然后 cur 指向下一个节点C**， 此时让节点 B 的指针域指向前一个元素 A ，再把 temp 复制给 pre 。

### AC代码
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) 
    {
        ListNode* pre = NULL;
        ListNode* cur = head;
        while(cur!=NULL)
        {
            ListNode* temp = cur;
            cur = cur->next;
            temp->next = pre;
            pre = temp;
        }
        return pre;
    }
};
```

# 贰拾壹 LCR_139.训练计划Ⅰ
题目：教练使用整数数组 actions 记录一系列核心肌群训练项目编号。为增强训练趣味性，需要将所有奇数编号训练项目调整至偶数编号训练项目之前。请将调整后的训练项目编号以 数组 形式返回。
示例：输入：actions = [1,2,3,4,5]
输出：[1,3,5,2,4] 
解释：为正确答案之一

思路：遍历原数组 actions ，把奇数与偶数分别 push_back 进两个数组中，遍历完成之后把偶数数组的每个元素 push_back 到奇数数组的后面。

### AC代码
```cpp
class Solution {
public:
    vector<int> trainingPlan(vector<int>& actions) 
    {
        vector<int> v1;
        vector<int> v2;
        for(int i=0;i<actions.size();i++)
        {
            if(actions[i]%2==1)//奇数
            {
                v1.push_back(actions[i]);
            }
            else
            {
                v2.push_back(actions[i]);
            }
        }
        for(int i=0;i<v2.size();i++)
        {
            v1.push_back(v2[i]);
        }
        return v1;
    }
};
```

# 贰拾贰 LCR_178.训练计划Ⅵ
题目：教学过程中，教练示范一次，学员跟做三次。该过程被混乱剪辑后，记录于数组 actions，其中 actions[i] 表示做出该动作的人员编号。请返回教练的编号。

示例：输入：actions = [12, 1, 6, 12, 6, 12, 6] ； 输出：1

思路：创建一个 unordered_map ，将数组的元素作为 key ，元素出现的次数作为 value 。在遍历数组的时候，每次把这个元素的 value 进行加一操作。遍历结束之后，只要找到 value 为一的 key 值就完成了题意。

注意：
①对某个 key 的 value 进行加一操作，语法是M[actions[i]]+=1，其中 M 是 unordered_map ， actions[i] 是 key 值。
②设 inner 是一个 unordered_map 容器类型的迭代器，那么 inner->first 指的是 key 值，inner->second 指的是 value 值。
### AC代码
```cpp
class Solution {
public:
    int trainingPlan(vector<int>& actions) 
    {
        unordered_map<int,int> M;
        for(int i=0 ; i<actions.size();i++)
        {
            M[actions[i]]+=1;
        }
        for(auto inner = M.begin();inner!=M.end();inner++)
        {
            if(inner->second==1)
            {
                return inner->first;
            }
        }
        return 0;
    }
};
```

# 贰拾叁 438.找到字符串中所有字母异位词
题目：给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。异位词 指由相同字母重排列形成的字符串（包括相同的字符串）。

示例：
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。

输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。

思路：设立两个大小为 26 的 int 数组 ss 和 pp ，数组 pp 可以存放字符串 p 中各个字母出现的次数。然后看成有一个固定大小的滑动窗口在向右滑动，数组 ss 存放的是这个滑动窗口中每个元素出现的次数。滑动窗口遍历字符串 s ，每次右移都右进一个、左边出一个元素。当数组 ss 和 pp 完全相同时，此时滑动窗口的最左边位置就是一个起始索引。

### AC 代码
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) 
    {
        vector<int> juice;
        vector<int> ss(26,0);
        vector<int> pp(26,0);

        if(s.size()<p.size())
        {
            return juice;
        }
        for(int i=0;i<p.size();i++)
        {
            pp[p[i]-'a']+=1;
            ss[s[i]-'a']+=1;
        }
        if(ss==pp)
        {
            juice.push_back(0);
        }
        for(int i=p.size();i<s.size();i++)
        {
            ss[s[i-p.size()]-'a']-=1;
            ss[s[i]-'a']+=1;
            if(ss==pp)
            {
            juice.push_back(i-p.size()+1);
            }
        }
        return juice;
    }
};
```

# 贰拾肆 260.只出现一次的数字Ⅲ
题目：给你一个整数数组 nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。你可以按 任意顺序 返回答案。**你必须设计并实现线性时间复杂度的算法且仅使用常量额外空间来解决此问题。**

示例：
输入：nums = [1,2,1,3,2,5]
输出：[3,5]
解释：[5, 3] 也是有效的答案。

思路：设置一个 unordered_map 来存放数组的元素作为 key 和元素出现的次数作为 value 。遍历数组之后，遍历 unordered_map ，当当前迭代器的 value 为一的时候，则将当前迭代器的 key 压入到结果数组中。遍历 unordered_map 之后返回结果数组，结果数组就是答案。
以上的算法的两次遍历都是 O(n) 的时间复杂度，使用的额外空间来自 unordered_map ，最多不超过 10 对映射，实现了常量额外空间。

### AC代码
```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) 
    {
        vector<int> juice;
        unordered_map<int,int> M;
        for(int i=0 ; i<nums.size();i++)
        {
            M[nums[i]]+=1;
        }
        for(auto inner = M.begin();inner!=M.end();inner++)
        {
            if(inner->second==1)
            {
                juice.push_back(inner->first);
            }
            if(juice.size()==2)
            {
                break;
            }
        }
return juice;
    }
};
```

# 贰拾伍 567.字符串的排列
题目：给你两个字符串 s1 和 s2 ，写一个函数来判断 s2 是否包含 s1 的排列。如果是，返回 true ；否则，返回 false 。换句话说，s1 的排列之一是 s2 的 子串 。即 s1 的某个异位字符串是 s2 的子串。

思路：设置两个 int 数组 a1 与 a2 ，a1 存放 s1 中每个元素出现的次数。想象一个固定大小的滑动窗口从左往右遍历字符串 s2 ，a2 存放滑动窗口中每个元素出现的次数。当数组 a1 与 a2 相等时，此时说明字符串 s2 中包含 s1 的异位字符串；若遍历完成之后还没有出现数组 a1 与 a2 相等的情况，则说明不存在符合要求的情形。**记得判断字符串 s2 长度小于 s1 的情况。**

### AC代码
 ```cpp
 class Solution {
public:
    bool checkInclusion(string s1, string s2) 
    {
        vector<int> a1(26,0);
        vector<int> a2(26,0);
        if(s1.size()>s2.size())
        {
            return false;
        }
        for(int i=0;i<s1.size();i++)
        {
            a2[s2[i]-'a']+=1;
            a1[s1[i]-'a']+=1;//不变
        }
        if(a1==a2)
        {
            return true;
        }
        for(int i=s1.size();i<s2.size();i++)
        {
            a2[s2[i]-'a']+=1;
            a2[s2[i-s1.size()]-'a']-=1;
            if(a1==a2)
            {
                return true;
            }
        }
        return false;
    }
};
 ```

# 贰拾陆 26.删除有序数组中的重复项
题目：给你一个 非严格递增排列 的数组 nums ，请你**原地**删除重复出现的元素，使每个元素 只出现一次 ，**返回删除后数组的新长度**。元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数。

示例：
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。

输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。

注意：题目要求在原数组中删除，通过测试得知，测试案例会调用传进来的数组检查，因此新建数组会导致全错。

### AC代码
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) 
    {
        int temp=0;
        for(auto n : nums)
        {
            if(temp<1 || n!=nums[temp-1])
            {
                nums[temp++] = n;
            }
        }
        return temp;
    }
};
```
思路：在 C++ 中，**使用基于范围的 for 循环遍历数组时，循环变量是数组元素的副本**，而不是指向数组元素的指针或引用。因此，**在循环体内部修改循环变量的值不会影响原始数组中的元素**。但是，在以上代码中，使用了基于范围的 for 循环遍历数组 nums，并在循环体内部修改了数组元素的值。这是因为在这个例子中，使用了引用类型 vector<int>& 来传递数组参数。这意味着在函数内部对数组元素的修改会直接反映到原始数组中。
变量 temp 用于记录新数组中已存储的元素个数。在循环开始时，temp 的值为 0。在每次循环中，变量 n 用于存储当前遍历到的元素。**如果 temp 小于 1 或者 n 不等于新数组中最后一个元素**，则将 n 存储到新数组中，并将 temp 的值加 1。


# 贰拾柒 80.删除有序数组中的重复项
题目：给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。**不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。**

示例：
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3。不需要考虑数组中超出新长度后面的元素。

思路：与上一题一样的思路，改变的是判断条件。即上一题是**temp 小于 1 或者 n 不等于新数组中最后一个元素**，本题应为**temp 小于 1 或者 n 不等于新数组的倒数第二个元素**。

### AC代码
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) 
    {
        int i=0;
        for(auto n : nums)
        {
            if(i<2 || n!=nums[i-2])
            {
                nums[i++] = n;
            }
        }
        return i;
    }
};
```

# 贰拾捌 102.二叉树的层序遍历
题目：给你二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。

思路：既然是层序遍历，那么就要每次输出同一层的所有节点值。同一层节点的父节点也是同一层节点，同理递推得，最终祖先一定是根节点的两个子节点。又因为这里需要从左到右分析节点和输出节点值，因此采用一个队列来存放同一层的节点。当同一层节点全部入队后，取得此时队列长度，再根据长度对队列内的节点进行其子节点的入队。
当函数取得根节点时，先把根节点入队。此时队列长度为 1 ，因此循环一次。循环体内操作是把节点的左右子节点入队，并把该节点的值 push_back 到数组，之后把该节点出队。第一轮循环结束后，此时队列中有两个元素（分别是根节点的左右子节点），因此循环两次，把两个子节点各自的子节点入队，同时把子节点自己出队。第二轮循环结束后，此时队列中有四个元素，因此循环四次，把此时队列中节点的子节点入队，并把自己出队…………。这样重复，直到队列为空，此时就遍历完树的所有节点。

### AC代码
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        if(root!=NULL)
        {
            que.push(root);
        }
        vector<vector<int>> result;
        while(!que.empty())
        {
            int size=que.size();
            vector<int> vec;
            for(int i=0;i<size;i++)
            {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);//本身的值
            if (node->left) 
            {que.push(node->left);}
            if (node->right) 
            {que.push(node->right);}
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

# 贰拾玖 222.完全二叉树的节点个数
题目：给你一棵 完全二叉树 的根节点 root ，求出该树的节点个数。
完全二叉树 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

思路：做题手法与二叉树的层序遍历相似，由于是完全二叉树，因此节点在每一层都是从左到右排列的。唯一的不同是：层序遍历时记录的是该节点的值，而求节点个数的时候则是每次对节点数量加一。

### AC代码
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) 
    {
        queue<TreeNode*> que;
        int juice=0;
        if(root!=NULL)
        {
            que.push(root);
            juice+=1;
        }
        while(!que.empty())
        {
            int temp=que.size();
            for(int i=0;i<temp;i++)
            {
                TreeNode* ff=que.front();
                que.pop();
                //juice+=1;
                if(ff->left)
                {
                    que.push(ff->left);
                    juice+=1;
                }
                if(ff->right)
                {
                    que.push(ff->right);
                    juice+=1;
                }

            }
        }
        return juice;
    }
};
```
注意，这里没有选择在节点出列时对结果加一，因为如果查询到没有节点的树就会报错，因此这里选择在节点入列的时候对结果加一。

# 叁拾 404.左叶子之和
题目：给定二叉树的根节点 root ，返回所有左叶子之和。

思路：**首先注意到是左叶子，而不是左子树上的节点。**左叶子节点是所有在某个节点的左叶子位置，且没有子节点的节点。
判断是否左叶子节点，要经过两层判断。第一层是对节点 A 判断是否存在左子节点 B ，第二层是判断节点 B 是否存在子节点。
这里选择把节点 A 当做当前遍历的节点而不是节点 B 。
函数内部首先判断当前遍历的节点 A 的左子节点 B 是否存在，然后判断左子节点 B 是否存在子节点。完成对左子节点 B 的判断之后，按顺序对左子节点 B 的左子节点 BL 、左子节点 B 的右子节点 BR 、本节点 A 的右子节点 AR 进行递归，类似前序遍历。最后返回相加结果。

### AC代码
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int getSum(TreeNode* node)
    {
        int juice=0;
        if(node->left)
        {
            if(node->left->left==NULL && node->left->right==NULL)
            {
                juice+=node->left->val;
            }
            //juice+=node->left->val;
            juice+=getSum(node->left);
        }
        if(node->right)
        {
            juice+=getSum(node->right);
        }
        return juice;
    }

    int sumOfLeftLeaves(TreeNode* root) 
    {
        int juice=getSum(root);
        return juice;

    }
};
```

# 叁拾壹 513.找树左下角的值
题目：给定一个二叉树的 根节点 root，请找出该二叉树的 最底层 最左边 节点的值。假设二叉树中至少有一个节点。

思路：最底层、最左边，很自然地想到用层序遍历的思路，记录二叉树的深度，同时层序遍历二叉树。最后需要返回二维数组左下方的数据即可。

### AC代码
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) 
    {
        queue<TreeNode*> que;
        int depth=0;
        vector<vector<int>> vec;
        if(root!=NULL)
        {
            que.push(root);
        }
        while(!que.empty())
        {
            vector<int> ve;
            int temp=que.size();
            for(int i=0;i<temp;i++)
            {
                TreeNode* ff=que.front();
                ve.push_back(ff->val);
                que.pop();
                if(ff->left)
                {
                    que.push(ff->left);
                }
                if(ff->right)
                {
                    que.push(ff->right);
                }
            }
            vec.push_back(ve);
            depth+=1;
        }
        return vec[depth-1][0];
    }
};
```

# 叁拾贰 104.二叉树的最大深度
题目：给定一个二叉树 root ，返回其最大深度。
二叉树的 最大深度 是指从根节点到最远叶子节点的最长路径上的节点数。

思路：实际上根节点的高度就是二叉树的最大深度。
使用前序遍历方法，就是从上往下找，每次把当前路径的最大深度与已有的最大深度比较，直到找遍所有路径。
使用后序遍历方法，从最底层开始遍历：从下往上，每个节点的高度就是该节点的左右子树中的最大高度加一，然后返回该节点的高度。最后得到根节点的高度就是二叉树的最大深度。

### AC代码
```cpp
//前序遍历
class solution {
public:
    int result;
    void getdepth(TreeNode* node, int depth) {
        result = depth > result ? depth : result; // 中

        if (node->left == NULL && node->right == NULL) return ;

        if (node->left) { // 左
            depth++;    // 深度+1
            getdepth(node->left, depth);
            depth--;    // 回溯，深度-1
        }
        if (node->right) { // 右
            depth++;    // 深度+1
            getdepth(node->right, depth);
            depth--;    // 回溯，深度-1
        }
        return ;
    }
    int maxDepth(TreeNode* root) {
        result = 0;
        if (root == NULL) return result;
        getdepth(root, 1);
        return result;
    }
};
```
```cpp
//后序遍历
class solution {
public:
    int getdepth(TreeNode* node) {
        if (node == NULL) return 0;
        int leftdepth = getdepth(node->left);       // 左
        int rightdepth = getdepth(node->right);     // 右
        int depth = 1 + max(leftdepth, rightdepth); // 中
        return depth;
    }
    int maxDepth(TreeNode* root) {
        return getdepth(root);
    }
};
```

# 叁拾叁 559.N叉树的最大深度
给定一个 N 叉树，找到其最大深度。
N 叉树中每个节点的子节点数目不一定相同，每个节点有一个存放子节点的vector。

思路:使用 N叉树 的后序遍历，对从最底层开始得到每个节点的所有子节点的最大高度，再加一得到当前子节点的最大高度，最后得到根节点的最大高度就是 N叉树 的最大深度。

### AC代码
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    int maxDepth(Node* root) 
    {
        int juice=0;
        if(root==NULL)
        {
            return 0;
        } 
        int chjuice=0;   
        for(auto child : root->children)
        {
            int temp=maxDepth(child);
            chjuice=max(chjuice,temp);
        }
        juice=chjuice+1;
        return juice;
    }
};
```

# 叁拾肆 111.二叉树的最小深度
题目：给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

思路：大体思路与最大深度类似，最小深度是取左右节点中的较小值加一。
但是有一点不同：如果一个节点 A 存在且仅存在一个子节点 B ，那么这个节点 A 就不是叶子节点，此时如果仍然取左右节点中的较小值加一，那么这个节点的高度就会变成空节点的高度加一，但这不是正确的结果。**正确做法是：当且仅当节点 A 存在一个子节点 B 时，节点 A 的高度等于它的子节点 B 的高度加一。而当 A 存在两个子节点或者不存在子节点时，节点 A 的高度就等于左右子节点的最小高度加一。**

### AC代码
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int minDepth(TreeNode* root) 
    {
        int juice=0;
        if(root==NULL)
        {
            return 0;
        }
        int ll=minDepth(root->left);
        int rr=minDepth(root->right);
        if(root->left==NULL)
        {
            juice=rr+1;
        }
        else if(root->right==NULL)
        {
            juice=ll+1;
        }
        else
        {
        juice=min(ll,rr)+1;
        }
        return juice;

    }
};
```

# 叁拾伍 501.二叉搜索树中的众数
题目：给你一个含重复值的二叉搜索树（BST）的根节点 root ，找出并返回 BST 中的所有 众数（即，出现频率最高的元素）。如果树中有不止一个众数，可以按 任意顺序 返回。

思路：第一种方法是我自己最开始想到的暴力解法，遍历一遍整棵树，用一个 map 来存放每一个出现的值及其次数，遍历完成以后把 map 中的key 和 value 分别存为一个 vector ，最后找出 vrctor 中最大的值及其对应的 key 值，从而得到众数。这样的暴力解法无可非议，但是这对于任一个数组都适用，既然已经给了我一个二叉搜索树，那我何必再去暴力解决呢？费力不讨好。
**第二种方法**：
思考一下我们可以得知，二叉搜索树按照中序遍历，那么就会是一个有序数组。其中元素是按照非递减的顺序排列。
既然是有序数组，那么我们可以用两个指针，分别指向相邻的两个元素：当前遍历元素 cur 和上一个被遍历的元素 pre 。当 cur 的值与 pre 的值相同时，说明当前遍历到的值 cur->val 又出现了一遍。此时把记录当前元素出现次数的 count 加一。
当 count 等于已有的最大出现次数 Maxcount 时，把当前遍历到的元素值放入结果容器中。如果当前元素值继续出现，就会出现 count 大于 Maxcount 的情况，此时就要把 Maxcount 更新为新的 count ，然后把已有的结果容器清空，再把当前元素值放入结果容器中。
注意：
- 以上的比较步骤只在对当前节点操作的时候进行，对左右子节点只有遍历操作。
- pre 指针初始化为 NULL ，count在第一次对当前节点进行判断的时候初始化为 1 。
- 由于这个结果容器和最大次数 Maxcount 都是实时更新的，因此实际上只需要遍历一次二叉树就能得出结果，而且 Maxcount 确实会一直是当前已有的最大次数。
  
### AC代码
```cpp
//暴力解法
class Solution {
private:
    void searchBST(TreeNode* cur, unordered_map<int, int>& map) 
{ // 前序遍历
    if (cur == NULL) return ;
    map[cur->val]++; // 统计元素频率
    searchBST(cur->left, map);
    searchBST(cur->right, map);
    return ;
}
    bool static cmp (const pair<int, int>& a, const pair<int, int>& b) 
{
    return a.second > b.second;
}
public:
    vector<int> findMode(TreeNode* root) 
    {
        unordered_map<int, int> map; // key:元素，value:出现频率
        vector<int> result;
        if (root == NULL) return result;
        searchBST(root, map);
        vector<pair<int, int>> vec(map.begin(), map.end());
        sort(vec.begin(), vec.end(), cmp); // 给频率排个序
        result.push_back(vec[0].first);
        for (int i = 1; i < vec.size(); i++) 
        {
            // 取最高的放到result数组中
            if (vec[i].second == vec[0].second) result.push_back(vec[i].first);
            else break;
        }
        return result;
    }
};
```
```cpp
//利用二叉搜索树的特性进行中序遍历的解法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
private:
        TreeNode* pre=NULL;
        int count =0;
        int Maxcount =0;
        vector<int> juice;
    void travel(TreeNode * cur)
    {
        if(cur==NULL)
        {
            return ;
        }
        if(cur->left)
        {
            travel(cur->left);
        }
        //下面是重头戏，是对当前节点的分析
        if(pre==NULL)
        {
            count=1;
        }
        else if(cur->val == pre->val)
        {
            count+=1;
        }
        else
        {
            count=1;
        }
        pre=cur;
        if(count==Maxcount)
        {
            juice.push_back(cur->val);
        }
        if(count>Maxcount)
        {
            Maxcount=count;
            juice.clear();
            juice.push_back(cur->val);
        }
        if(cur->right)
        {
            travel(cur->right);
        }
    }

public:
    vector<int> findMode(TreeNode* root) 
    {
        travel(root);
        return juice;
    }
};
```
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

第一次思路：
第一次代码：
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

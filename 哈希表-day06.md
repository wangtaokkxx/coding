#### 1.有效的字母异位词

**题目链接**：https://leetcode.cn/problems/valid-anagram/

**解题思路**：利用数组做为哈希表。

```java
// 自己写得
class Solution {
    public boolean isAnagram(String s, String t) {
        boolean flag = true;
        int[] arr = new int[26];
        for(int i = 0; i < s.length(); i++){
            arr[s.charAt(i)-'a']++;
        }
        for(int j = 0; j < t.length(); j++){
            arr[t.charAt(j)-'a']--;
        }
        for (int i = 0; i < arr.length; i++) {
            if(arr[i] != 0){
                flag = false;
            }
        }
        return flag;
    }
}

/**
 * 242. 有效的字母异位词 字典解法
 * 时间复杂度O(m+n) 空间复杂度O(1)
 */
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] record = new int[26];

        for (int i = 0; i < s.length(); i++) {
            record[s.charAt(i) - 'a']++;     // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
        }

        for (int i = 0; i < t.length(); i++) {
            record[t.charAt(i) - 'a']--;
        }
        
        for (int count: record) {
            if (count != 0) {               // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        return true;                        // record数组所有元素都为零0，说明字符串s和t是字母异位词
    }
}
```

#### 2.两个数组的交集

**题目链接**：https://leetcode.cn/problems/intersection-of-two-arrays/

**解题思路**：。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) {
            return new int[0];
        }
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resSet = new HashSet<>();
        //遍历数组1
        for (int i : nums1) {
            set1.add(i);
        }
        //遍历数组2的过程中判断哈希表中是否存在该元素
        for (int i : nums2) {
            if (set1.contains(i)) {
                resSet.add(i);
            }
        }
      
        //方法1：将结果集合转为数组
        return resSet.stream().mapToInt(x -> x).toArray();
        
        //方法2：另外申请一个数组存放setRes中的元素,最后返回数组
        int[] arr = new int[setRes.size()];
        int j = 0;
        for(int i : setRes){
            arr[j++] = i;
        }
        return arr;
    }
}
```



#### 3.快乐数

**题目链接**：https://leetcode.cn/problems/happy-number/

**解题思路**：。

```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextNumber(n);
        }
        return n == 1;
    }

    private int getNextNumber(int n) {
        int res = 0;
        while (n > 0) {
            int temp = n % 10;
            res += temp * temp;
            n = n / 10;
        }
        return res;
    }
}
```

#### 4.两数之和

**题目链接**：https://leetcode.cn/problems/two-sum/

**解题思路**：。

```java
// 暴力解法
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        for(int i = 0; i < nums.length - 1; i++){
            for(int j = i + 1 ; j < nums.length; j++){
                if(nums[i] + nums[j] == target){
                    res[0] = i;
                    res[1] = j;
                    return res;
                }
            }
        }
        return res;
    }
}
// 哈希表中map集合
public int[] twoSum(int[] nums, int target) {
    int[] res = new int[2];
    if(nums == null || nums.length == 0){
        return res;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for(int i = 0; i < nums.length; i++){
        int temp = target - nums[i];   // 遍历当前元素，并在map中寻找是否有匹配的key
        if(map.containsKey(temp)){
            res[1] = i;
            res[0] = map.get(temp);
            break;
        }
        map.put(nums[i], i);    // 如果没找到匹配对，就把访问过的元素和下标加入到map中
    }
    return res;
}
```


#### 1. 216-组合总和-III

**题目链接**：https://leetcode.cn/problems/combination-sum-iii/

**解题思路**：

```java
//模板方法
class Solution {
	List<List<Integer>> result = new ArrayList<>();
	LinkedList<Integer> path = new LinkedList<>();

	public List<List<Integer>> combinationSum3(int k, int n) {
		backTracking(n, k, 1, 0);
		return result;
	}

	private void backTracking(int targetSum, int k, int startIndex, int sum) {
		// 减枝
		if (sum > targetSum) {
			return;
		}

		if (path.size() == k) {
			if (sum == targetSum) result.add(new ArrayList<>(path));
			return;
		}

		// 减枝 9 - (k - path.size()) + 1
		for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) {
			path.add(i);
			sum += i;
			backTracking(targetSum, k, i + 1, sum);
			//回溯
			path.removeLast();
			//回溯
			sum -= i;
		}
	}
}

// 上面剪枝 i <= 9 - (k - path.size()) + 1; 如果还是不清楚
// 也可以改为 if (path.size() > k) return; 执行效率上是一样的
class Solution {
    LinkedList<Integer> path = new LinkedList<>();
    List<List<Integer>> ans = new ArrayList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        build(k, n, 1, 0);
        return ans;
    }

    private void build(int k, int n, int startIndex, int sum) {

        if (sum > n) return;

        if (path.size() > k) return;

        if (sum == n && path.size() == k) {
            ans.add(new ArrayList<>(path));
            return;
        }

        for(int i = startIndex; i <= 9; i++) {
            path.add(i);
            sum += i;
            build(k, n, i + 1, sum);
            sum -= i;
            path.removeLast();
        }
    }
}


//其他方法
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();

    public List<List<Integer>> combinationSum3(int k, int n) {
        res.clear();
        list.clear();
        backtracking(k, n, 9);
        return res;
    }

    private void backtracking(int k, int n, int maxNum) {
        if (k == 0 && n == 0) {
            res.add(new ArrayList<>(list));
            return;
        }

        // 因为不能重复，并且单个数字最大值是maxNum，所以sum最大值为
        // （maxNum + (maxNum - 1) + ... + (maxNum - k + 1)） == k * maxNum - k*(k - 1) / 2
        if (maxNum == 0
                || n > k * maxNum - k * (k - 1) / 2
                || n < (1 + k) * k / 2) {
            return;
        }
        list.add(maxNum);
        backtracking(k - 1, n - maxNum, maxNum - 1);
        list.remove(list.size() - 1);
        backtracking(k, n, maxNum - 1);
    }

}
```

#### 2. 17.电话号码的字母组合

**题目链接**：https://leetcode.cn/problems/letter-combinations-of-a-phone-number/

**解题思路**：

```java
class Solution {

    //设置全局列表存储最后的结果
    List<String> list = new ArrayList<>();

    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return list;
        }
        //初始对应所有的数字，为了直接对应2-9，新增了两个无效的字符串""
        String[] numString = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        //迭代处理
        backTracking(digits, numString, 0);
        return list;

    }

    //每次迭代获取一个字符串，所以会设计大量的字符串拼接，所以这里选择更为高效的 StringBuild
    StringBuilder temp = new StringBuilder();

    //比如digits如果为"23",num 为0，则str表示2对应的 abc
    public void backTracking(String digits, String[] numString, int num) {
        //遍历全部一次记录一次得到的字符串
        if (num == digits.length()) {
            list.add(temp.toString());
            return;
        }
        //str 表示当前num对应的字符串
        String str = numString[digits.charAt(num) - '0'];
        for (int i = 0; i < str.length(); i++) {
            temp.append(str.charAt(i));
            //c
            backTracking(digits, numString, num + 1);
            //剔除末尾的继续尝试
            temp.deleteCharAt(temp.length() - 1);
        }
    }
}
```


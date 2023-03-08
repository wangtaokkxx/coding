#### 1.反转字符串

**题目链接**：https://leetcode.cn/problems/reverse-string/

**解题思路**：。

```java
// 自己写得
class Solution {
    public void reverseString(char[] s) {
        int i = 0;
        int j = s.length - 1;
        char temp;
        while(i < j){
            temp = s[i];
            s[i] = s[j];
            s[j] = temp;
            i++;
            j--;
        }
    }
}
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while (l < r) {
            s[l] ^= s[r];  //构造 a ^ b 的结果，并放在 a 中
            s[r] ^= s[l];  //将 a ^ b 这一结果再 ^ b ，存入b中，此时 b = a, a = a ^ b
            s[l] ^= s[r];  //a ^ b 的结果再 ^ a ，存入 a 中，此时 b = a, a = b 完成交换
            l++;
            r--;
        }
    }
}

// 第二种方法用temp来交换数值更多人容易理解些
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while(l < r){
            char temp = s[l];
            s[l] = s[r];
            s[r] = temp;
            l++;
            r--;
        }
    }
}
```

#### 2.反转字符串II

**题目链接**：https://leetcode.cn/problems/reverse-string-ii/

**解题思路**：。

```java
//解法一
class Solution {
    public String reverseStr(String s, int k) {
        StringBuffer res = new StringBuffer();
        int length = s.length();
        int start = 0;
        while (start < length) {
            // 找到k处和2k处
            StringBuffer temp = new StringBuffer();
            // 与length进行判断，如果大于length了，那就将其置为length
            int firstK = (start + k > length) ? length : start + k;
            int secondK = (start + (2 * k) > length) ? length : start + (2 * k);

            //无论start所处位置，至少会反转一次
            temp.append(s.substring(start, firstK));
            res.append(temp.reverse());

            // 如果firstK到secondK之间有元素，这些元素直接放入res里即可。
            if (firstK < secondK) { //此时剩余长度一定大于k。
                res.append(s.substring(firstK, secondK));
            }
            start += (2 * k);
        }
        return res.toString();
    }
}

//解法二（似乎更容易理解点）
//题目的意思其实概括为 每隔2k个反转前k个，尾数不够k个时候全部反转
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0; i < ch.length; i += 2 * k){
            int start = i;
            //这里是判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1, start + k - 1);
            //用异或运算反转 
            while(start < end){
                ch[start] ^= ch[end];
                ch[end] ^= ch[start];
                ch[start] ^= ch[end];
                start++;
                end--;
            }
        }
        return new String(ch);
    }
}

// 解法二还可以用temp来交换数值，会的人更多些
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0;i < ch.length;i += 2 * k){
            int start = i;
            // 判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1,start + k - 1);
            while(start < end){
                
                char temp = ch[start];
                ch[start] = ch[end];
                ch[end] = temp;

                start++;
                end--;
            }
        }
        return new String(ch);
    }
}
// 解法3
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        // 1. 每隔 2k 个字符的前 k 个字符进行反转
        for (int i = 0; i< ch.length; i += 2 * k) {
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= ch.length) {
                reverse(ch, i, i + k -1);
                continue;
            }
            // 3. 剩余字符少于 k 个，则将剩余字符全部反转
            reverse(ch, i, ch.length - 1);
        }
        return  new String(ch);

    }
    // 定义翻转函数
    public void reverse(char[] ch, int i, int j) {
    for (; i < j; i++, j--) {
        char temp  = ch[i];
        ch[i] = ch[j];
        ch[j] = temp;
    }

    }
}
```



#### 3.替换空格

**题目链接**：https://leetcode.cn/problems/ti-huan-kong-ge-lcof/

**解题思路**：。

```java
// 方法一：使用一个新的对象，复制 str，复制的过程对其判断，是空格则替换，否则直接复制，类似于数组复制
public static String replaceSpace(String s) {
        if (s == null) {
            return null;
        }
        //选用 StringBuilder 单线程使用，比较快，选不选都行
        StringBuilder sb = new StringBuilder();
        //使用 sb 逐个复制 s ，碰到空格则替换，否则直接复制
        for (int i = 0; i < s.length(); i++) {
            //s.charAt(i) 为 char 类型，为了比较需要将其转为和 " " 相同的字符串类型
            //if (" ".equals(String.valueOf(s.charAt(i)))){}
            if (s.charAt(i) == ' ') {
                sb.append("%20");
            } else {
                sb.append(s.charAt(i));
            }
        }
        return sb.toString();
    }

//方式二：双指针法
public String replaceSpace(String s) {
    if(s == null || s.length() == 0){
        return s;
    }
    //扩充空间，空格数量2倍
    StringBuilder str = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {
        if(s.charAt(i) == ' '){
            str.append("  ");
        }
    }
    //若是没有空格直接返回
    if(str.length() == 0){
        return s;
    }
    //有空格情况 定义两个指针
    int left = s.length() - 1;//左指针：指向原始字符串最后一个位置
    s += str.toString();
    int right = s.length()-1;//右指针：指向扩展字符串的最后一个位置
    char[] chars = s.toCharArray();
    while(left>=0){
        if(chars[left] == ' '){
            chars[right--] = '0';
            chars[right--] = '2';
            chars[right] = '%';
        }else{
            chars[right] = chars[left];
        }
        left--;
        right--;
    }
    return new String(chars);
}
```

#### 4. 翻转字符串里的单词

**题目链接**：https://leetcode.cn/problems/reverse-words-in-a-string/

**解题思路**：。

```java
class Solution {
   /**
     * 不使用Java内置方法实现
     * <p>
     * 1.去除首尾以及中间多余空格
     * 2.反转整个字符串
     * 3.反转各个单词
     */
    public String reverseWords(String s) {
        // System.out.println("ReverseWords.reverseWords2() called with: s = [" + s + "]");
        // 1.去除首尾以及中间多余空格
        StringBuilder sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuilder removeSpace(String s) {
        // System.out.println("ReverseWords.removeSpace() called with: s = [" + s + "]");
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuilder sb = new StringBuilder();
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
        }
        // System.out.println("ReverseWords.removeSpace returned: sb = [" + sb + "]");
        return sb;
    }

    /**
     * 反转字符串指定区间[start, end]的字符
     */
    public void reverseString(StringBuilder sb, int start, int end) {
        // System.out.println("ReverseWords.reverseString() called with: sb = [" + sb + "], start = [" + start + "], end = [" + end + "]");
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
        // System.out.println("ReverseWords.reverseString returned: sb = [" + sb + "]");
    }

    private void reverseEachWord(StringBuilder sb) {
        int start = 0;
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while (end < n && sb.charAt(end) != ' ') {
                end++;
            }
            reverseString(sb, start, end - 1);
            start = end + 1;
            end = start + 1;
        }
    }
}
//解法二：创建新字符数组填充。时间复杂度O(n)
class Solution {
    public String reverseWords(String s) {
        //源字符数组
        char[] initialArr = s.toCharArray();
        //新字符数组
        char[] newArr = new char[initialArr.length+1];//下面循环添加"单词 "，最终末尾的空格不会返回
        int newArrPos = 0;
        //i来进行整体对源字符数组从后往前遍历
        int i = initialArr.length-1;
        while(i>=0){
            while(i>=0 && initialArr[i] == ' '){i--;}  //跳过空格
            //此时i位置是边界或!=空格，先记录当前索引，之后的while用来确定单词的首字母的位置
            int right = i;
            while(i>=0 && initialArr[i] != ' '){i--;} 
            //指定区间单词取出(由于i为首字母的前一位，所以这里+1,)，取出的每组末尾都带有一个空格
            for (int j = i+1; j <= right; j++) {
                newArr[newArrPos++] = initialArr[j];
                if(j == right){
                    newArr[newArrPos++] = ' ';//空格
                }
            }
        }
        //若是原始字符串没有单词，直接返回空字符串；若是有单词，返回0-末尾空格索引前范围的字符数组(转成String返回)
        if(newArrPos == 0){
            return "";
        }else{
            return new String(newArr,0,newArrPos-1);
        }
    }
}
//解法三：双反转+移位，在原始数组上进行反转。空间复杂度O(1)
class Solution {
    /**
     * 思路：
     *	①反转字符串  "the sky is blue " => " eulb si yks eht"
     *	②遍历 " eulb si yks eht"，每次先对某个单词进行反转再移位
     *	   这里以第一个单词进行为演示：" eulb si yks eht" ==反转=> " blue si yks eht" ==移位=> "blue si yks eht"
     */
    public String reverseWords(String s) {
        //步骤1：字符串整体反转（此时其中的单词也都反转了）
        char[] initialArr = s.toCharArray();
        reverse(initialArr, 0, s.length() - 1);
        int k = 0;
        for (int i = 0; i < initialArr.length; i++) {
            if (initialArr[i] == ' ') {
                continue;
            }
            int tempCur = i;
            while (i < initialArr.length && initialArr[i] != ' ') {
                i++;
            }
            for (int j = tempCur; j < i; j++) {
                if (j == tempCur) { //步骤二：二次反转
                    reverse(initialArr, tempCur, i - 1);//对指定范围字符串进行反转，不反转从后往前遍历一个个填充有问题
                }
                //步骤三：移动操作
                initialArr[k++] = initialArr[j];
                if (j == i - 1) { //遍历结束
                    //避免越界情况，例如=> "asdasd df f"，不加判断最后就会数组越界
                    if (k < initialArr.length) {
                        initialArr[k++] = ' ';
                    }
                }
            }
        }
        if (k == 0) {
            return "";
        } else {
            //参数三：以防出现如"asdasd df f"=>"f df asdasd"正好凑满不需要省略空格情况
            return new String(initialArr, 0, (k == initialArr.length) && (initialArr[k - 1] != ' ') ? k : k - 1);
        }
    }

    public void reverse(char[] chars, int begin, int end) {
        for (int i = begin, j = end; i < j; i++, j--) {
            chars[i] ^= chars[j];
            chars[j] ^= chars[i];
            chars[i] ^= chars[j];
        }
    }
}
/*
 * 解法四：时间复杂度 O(n)
 * 参考卡哥 c++ 代码的三步骤：先移除多余空格，再将整个字符串反转，最后把单词逐个反转
 * 有别于解法一 ：没有用 StringBuilder  实现，而是对 String 的 char[] 数组操作来实现以上三个步骤
 */
class Solution {
    //用 char[] 来实现 String 的 removeExtraSpaces，reverse 操作
    public String reverseWords(String s) {
        char[] chars = s.toCharArray();
        //1.去除首尾以及中间多余空格
        chars = removeExtraSpaces(chars);
        //2.整个字符串反转
        reverse(chars, 0, chars.length - 1);
        //3.单词反转
        reverseEachWord(chars);
        return new String(chars);
    }

    //1.用 快慢指针 去除首尾以及中间多余空格，可参考数组元素移除的题解
    public char[] removeExtraSpaces(char[] chars) {
        int slow = 0;
        for (int fast = 0; fast < chars.length; fast++) {
            //先用 fast 移除所有空格
            if (chars[fast] != ' ') {
                //在用 slow 加空格。 除第一个单词外，单词末尾要加空格
                if (slow != 0)
                    chars[slow++] = ' ';
                //fast 遇到空格或遍历到字符串末尾，就证明遍历完一个单词了
                while (fast < chars.length && chars[fast] != ' ')
                    chars[slow++] = chars[fast++];
            }
        }
        //相当于 c++ 里的 resize()
        char[] newChars = new char[slow];
        System.arraycopy(chars, 0, newChars, 0, slow); 
        return newChars;
    }

    //双指针实现指定范围内字符串反转，可参考字符串反转题解
    public void reverse(char[] chars, int left, int right) {
        if (right >= chars.length) {
            System.out.println("set a wrong right");
            return;
        }
        while (left < right) {
            chars[left] ^= chars[right];
            chars[right] ^= chars[left];
            chars[left] ^= chars[right];
            left++;
            right--;
        }
    }

    //3.单词反转
    public void reverseEachWord(char[] chars) {
        int start = 0;
        //end <= s.length() 这里的 = ，是为了让 end 永远指向单词末尾后一个位置，这样 reverse 的实参更好设置
        for (int end = 0; end <= chars.length; end++) {
            // end 每次到单词末尾后的空格或串尾,开始反转单词
            if (end == chars.length || chars[end] == ' ') {
                reverse(chars, start, end - 1);
                start = end + 1;
            }
        }
    }
}
```

#### 5. 剑指Offer58-II.左旋转字符串

**题目链接**：https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

**解题思路**：。

```java
// 解法一
class Solution {
    public String reverseLeftWords(String s, int n) {
        int len=s.length();
        StringBuilder sb=new StringBuilder(s);
        reverseString(sb,0,n-1);
        reverseString(sb,n,len-1);
        return sb.reverse().toString();
    }
     public void reverseString(StringBuilder sb, int start, int end) {
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
            }
        }
}
//解法二：空间复杂度：O(1)。用原始数组来进行反转操作
//思路为：先整个字符串反转，再反转前面的，最后反转后面 n 个
class Solution {
    public String reverseLeftWords(String s, int n) {
        char[] chars = s.toCharArray();
        reverse(chars, 0, chars.length - 1);
        reverse(chars, 0, chars.length - 1 - n);
        reverse(chars, chars.length - n, chars.length - 1);
        return new String(chars);
    }

    public void reverse(char[] chars, int left, int right) {
        while (left < right) {
            chars[left] ^= chars[right];
            chars[right] ^= chars[left];
            chars[left] ^= chars[right];
            left++;
            right--;
        }
    }
}
```


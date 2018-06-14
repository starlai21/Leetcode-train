# Leetcode-train

## 1. [Two Sum](https://leetcode.com/problems/two-sum/description/)
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> table = new HashMap<>(); 
        for (int i = 0; i < nums.length; i++){
            table.put(nums[i], i);
        }
        int [] result = new int[2];
        for (int i = 0; i < nums.length; i++){
            Integer a = table.get(target-nums[i]);
            if (a != null && (a != i)){
                result[0] = i;
                result[1] = a;
            }
        }
        return result;
    }
}
```

## 2. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode solution = new ListNode(0);
        helper(l1, l2, 0, solution);
        return solution.next;
    }
    
    public void helper(ListNode l1, ListNode l2, int carry, ListNode solution){
        if (l1 == null && l2 == null){
            if (carry == 1)
                solution.next = new ListNode(carry); 
        }
        else{
            int sum = 0;
            int val1 = l1 == null ? 0 : l1.val;
            int val2 = l2 == null ? 0 : l2.val;
            sum = val1 + val2 + carry;
            if (sum >= 10){
                sum = sum % 10;
                carry = 1;
            }
            else{
                carry = 0;
            }
            
            solution.next = new ListNode(sum);
            solution = solution.next;
            
            helper(
                l1 == null ? null:l1.next,
                l2 == null ? null:l2.next,
                carry,
                solution
            );   
        }
    }
}
```

## 3. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Map <Character, Integer> lastOccur = new HashMap<>();
        int [] mostLeft = new int [n];
        int maxLength = 0;
        for (int i = 0; i < n; i++){
            char c = s.charAt(i);
            if (i == 0)
                mostLeft[0] = 0;
            else{
                if (lastOccur.containsKey(c)){
                    mostLeft[i] = Math.max(mostLeft[i-1], lastOccur.get(c)+1);
                }
                else{
                    mostLeft[i] = mostLeft[i-1];
                }                
            }
            maxLength = Math.max(maxLength, i - mostLeft[i]+ 1);
            lastOccur.put(c,i);
        }
                
        return maxLength;
    }
}
```



## 4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

[A great explanation](https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn))-solution-with-explanation))

```
class Solution {
    public double findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length;
        int n = B.length;        
        // ensure n >= m
        if (m > n)
            return findMedianSortedArrays(B, A);
        int i,
            j,
            left = 0,
            right = m,
            lengthOfRightPart = (m + n + 1) / 2;
        while(left <= right){
            i = (left + right) / 2;
            j = lengthOfRightPart - i;
            if ( i > 0 && A[i-1] > B[j]){
                right = i - 1;
            }
            else if (i < m && B[j-1] > A[i]){
                left = i + 1;
            }
            else {
                double maxLeft;
                if (i == 0)
                    maxLeft = B[j-1];
                else if (j == 0)
                    maxLeft = A[i-1]; 
                else
                    maxLeft = Math.max(A[i-1], B[j-1]);
                if ( (m+n)%2 != 0)
                    return maxLeft;
                double minRight;
                if (i == m)
                    minRight = B[j];
                else if (j == n)
                    minRight = A[i];
                else
                    minRight = Math.min(A[i], B[j]);
                return (minRight + maxLeft) / 2;
                    
            }
        }
        return 0;
    
    }
}
```


## 5. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description/)
```
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        String res = null;
        boolean [][] dp = new boolean [n][n];
        for (int i=n-1; i>= 0; i--)
            for (int j=i; j<n; j++){
                dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i<= 2 || dp[i+1][j-1] );
                if (dp[i][j] && (res == null || j-i+1 > res.length()))
                    res = s.substring(i, j+1);
            }
        return res;
    }
}
```


## 6. [ZigZag Conversion] (https://leetcode.com/problems/zigzag-conversion/description/)
```
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1 || numRows >= s.length())
            return s;
        StringBuilder [] sb = new StringBuilder[numRows];
        for (int i = 0; i<numRows; i++)
            sb[i] = new StringBuilder();
        int j = 0, k = 1;
        for (int i = 0; i<s.length(); i++){
            sb[j].append(s.charAt(i));
            if (j == 0)
                k = 1;
            if (j == numRows - 1)
                k = -1;
            j += k;
        }
        StringBuilder res = new StringBuilder();
        for (int i = 0; i<numRows; i++)
            res.append(sb[i]);
        return res.toString();
        
    }
}
```

## 7. [Reverse Integer](https://leetcode.com/problems/reverse-integer/description/)
```
class Solution {
    public int reverse(int x) {
        int reversed = 0;
        boolean isNegative = x<0;
        if (isNegative)
            x = -x;
        while (x > 0){
            if (reversed > (Integer.MAX_VALUE - x%10)/10)
                return 0;
            reversed = 10*reversed + x%10;
            x = x/10;
        }
        if (isNegative)
            reversed = -reversed;
        return reversed;

    }
}
```

## 8. [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)
```
class Solution {
    public int myAtoi(String str) {
        int res = 0, i = 0;
        boolean isNegative = false, hasSign = false;
        while (i<str.length() && str.charAt(i) == ' ')
            i++;
        for (; i<str.length(); i++){
            if (str.charAt(i) == '+' || str.charAt(i) == '-'){
                if (hasSign)
                    return isNegative ? -res : res ;
                hasSign = true;
                isNegative = str.charAt(i) == '-' ? true : false;
            } else if (str.charAt(i) >= '0' && str.charAt(i) <= '9'){
                hasSign = true;
                if (res > Integer.MAX_VALUE/10 || (res == Integer.MAX_VALUE/10 && str.charAt(i) > '7'))
                    return isNegative ? Integer.MIN_VALUE : Integer.MAX_VALUE;
                res = res*10 + str.charAt(i) - '0';
            } else {
                break;
            }
        }
        return isNegative ? -res : res ;
    }
}
```

## 9. [Palindrome Number](https://leetcode.com/problems/palindrome-number/description/)
```
class Solution {
    public boolean isPalindrome(int x) {
        if (x<0 || (x%10 == 0 && x != 0))
            return false;
        int reversed = 0;
        while (x > reversed){
            reversed = 10*reversed + x%10;
            x /= 10;
        }
        return (reversed == x || reversed/10 == x);
    }
}
```

## 10. [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/description/)

```
class Solution {
    public boolean isMatch(String s, String p) {
        if (p.isEmpty())
            return s.isEmpty();
        int n = s.length()+1;
        int m = p.length()+1;
        boolean [][] dp = new boolean [n][m];
        dp[0][0] = true;
        for(int i=2; i<m; i++){
            if(p.charAt(i-1) == '*')
                dp[0][i] = dp[0][i-2];
        }
        for(int i=1; i<n; i++)
            for(int j=1; j<m; j++){
                if(p.charAt(j-1) == s.charAt(i-1) || p.charAt(j-1) == '.')
                    dp[i][j] = dp[i-1][j-1];
                else if (p.charAt(j-1) == '*'){
                    if (p.charAt(j-2) == s.charAt(i-1) || p.charAt(j-2) == '.')
                        dp[i][j] = dp[i][j-2] || dp[i-1][j];
                    else 
                        dp[i][j] = dp[i][j-2];
                }
            }
        return dp[n-1][m-1];
    }
}
```
## 11. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/description/)
```
class Solution {
    public int maxArea(int[] height) {
        int h = 0, start = 0, end = height.length-1, area = 0;
        while(start < end){
            h = Math.min(height[start], height[end]);
            area = Math.max(h*(end-start), area);
            while (height[start] <= h && start < end)
                start++;
            while (height[end] <= h && start < end)
                end--;
        }
        return area;
    }
}
```

## 12. [Integer to Roman](https://leetcode.com/problems/integer-to-roman/description/)
```
class Solution {
    public String intToRoman(int num) {
        String [] thousands = {"", "M", "MM", "MMM"};
        String [] hundreds = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC","DCCC","CM" };
        String [] tens = {"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"};
        String [] ones = {"","I","II","III","IV","V","VI","VII","VIII","IX"};
        return thousands[num/1000] + hundreds[(num%1000)/100] + tens[(num%100)/10] + ones[num%10];
    }
}
```

## 13. [Roman to Integer](https://leetcode.com/problems/roman-to-integer/description/)
```
class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> map = new HashMap<>();
        map.put('I',1);
        map.put('V',5);
        map.put('X',10);
        map.put('L',50);
        map.put('C',100);
        map.put('D',500);
        map.put('M',1000);
        int value = 0, 
            length = s.length(), 
            i = length-2;
        value = map.get(s.charAt(length-1));
        while (i >= 0){
            if (map.get(s.charAt(i)) >= map.get(s.charAt(i+1)))
                value += map.get(s.charAt(i));
            else
                value -= map.get(s.charAt(i));
            i--;
        }
        return value;
    }
}

```

## 14. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/description/)
```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0)
            return "";
        String s = strs[0];
        for(int i=1; i<strs.length; i++){
            while(strs[i].indexOf(s) != 0)
                s = s.substring(0, s.length()-1);
        }
        return s;
    }
}
```

## 15. [3Sum](https://leetcode.com/problems/3sum/description/)
```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List <Integer>> solution = new ArrayList<>();
        Arrays.sort(nums);
        for (int i=0; i<nums.length; i++){
            if (nums[i] > 0)
                break;
            int target = -nums[i];
            int start = i+1;
            int end = nums.length-1;
            List <Integer> sub = null;
            while(start < end){
                int sum = nums[start] + nums[end];
                if (sum < target){
                    start++;
                } else if (sum > target){
                    end--;
                } else {
                    sub = new ArrayList<>();
                    sub.add(nums[start]);
                    sub.add(nums[end]);
                    sub.add(nums[i]);
                    solution.add(sub);
                    
                    while(start < end && sub.get(0) == nums[start])
                        start++;
                    while(start < end && sub.get(1) == nums[end])
                        end--;
                }
            }
            while(i+1 < nums.length && nums[i] == nums[i+1])
                i++;
        }
        return solution;
    }
}

```
## 16. [3Sum Closest](https://leetcode.com/problems/3sum-closest/description/)
```
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int value = nums[0] + nums[1] + nums[nums.length-1];
        Arrays.sort(nums);
        for (int i=0; i<nums.length; i++){
            int start = i+1;
            int end = nums.length-1;
            while(start < end){
                int sum = nums[start] + nums[end] + nums[i];
                if (sum > target){
                    end--;
                    if (Math.abs(value - target) > Math.abs(sum - target))
                        value = sum;
                } else if (sum < target){
                    start++;
                    if (Math.abs(value - target) > Math.abs(sum - target))
                        value = sum;
                } else {
                    return target;
                }
            }
            while(i+1 < nums.length && nums[i] == nums[i+1])
                i++;
        }
        return value;
    }
}
```

## 17. [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)
```
class Solution {
    public List<String> letterCombinations(String digits) {
        LinkedList<String> res = new LinkedList<>();
        if (digits == null || digits.isEmpty())
            return res;
        res.add("");
        String [] maps = {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        for (int i=0; i<digits.length(); i++){
            int value = Character.getNumericValue(digits.charAt(i));
            while(res.peek().length() == i){
                String first = res.remove();
                for (char c: maps[value].toCharArray()){
                    res.addLast(first+c);
                }
            }
        }
        return res;
    }
}
```
```
class Solution {
    public List<String> letterCombinations(String digits) {
        LinkedList<String> res = new LinkedList<>();
        if (digits == null || digits.isEmpty())
            return res;
        String [] maps = {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        res.add("");
        while(res.peek().length() != digits.length()){
            String first = res.remove();
            int value = Character.getNumericValue(digits.charAt(first.length()));
            for(char c: maps[value].toCharArray())
                res.addLast(first+c);
        }
        return res;
    }
}

```
## 18. [4Sum](https://leetcode.com/problems/4sum/description/)
```
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List <Integer>> res = new ArrayList<>();
        if (nums.length < 4)
            return res;
        Arrays.sort(nums);
        int length = nums.length;
        for (int i=0; i<length-3; i++){
            if (nums[i]*4 > target)
                break;
            if (nums[i] + nums[length-3] + nums[length-2] + nums[length-1] < target)
                continue;
            if (i > 0 && nums[i] == nums[i-1])
                continue;
            for (int j=i+1; j<length-2; j++){
                if (nums[i] + nums[j]*3 > target)
                    break;
                if (nums[i] + nums[j] + nums[length-2] + nums[length-1] < target)
                    continue;
                if (j > i+1 && nums[j] == nums[j-1])
                    continue;
                int start = j+1;
                int end = length-1;
                while (start < end){
                    if (nums[i] + nums[j] + nums[start]*2 > target)
                        break;
                    int sum = nums[i] + nums[j] + nums[start] + nums[end];
                    if (sum > target)
                        end--;
                    else if (sum < target)
                        start++;
                    else{
                        res.add(Arrays.asList(nums[i], nums[j], nums[start++], nums[end--]));
                        while(start < end && nums[start] == nums[start-1])
                            start++;
                        while(start < end && nums[end] == nums[end+1])
                            end--;
                    }

                }
            }
        }
        return res;
    }
}
```

## 19. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null)
            return null;
        int length = 0;
        ListNode cur = head, pre = null;
        while(cur != null){
            cur = cur.next;
            length++;
        }
        cur = head;
        while(length-- > n){
            pre = cur;
            cur = cur.next;
        }
        if (pre == null){
            head = head.next;
        }
        else {
            pre.next = cur.next;
        }
        return head;
    }
}
```
## 20. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)
```
class Solution {
    public boolean isValid(String s) {
        if (s.isEmpty())
            return true;
        Stack<Character> stack = new Stack<>();
        for (char c: s.toCharArray()){
            if (c == '{')
                stack.push('}');
            else if (c == '[')
                stack.push(']');
            else if (c == '(')
                stack.push(')');
            else {
                if (stack.empty() || stack.pop() != c)
                    return false;
            }
        }
        return stack.empty();
    }
}
```

## 21. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(0),
                 nlist = head;
        while (l1 != null && l2 != null){
            if (l1.val > l2.val){
                nlist.next = l2;   
                l2 = l2.next;
            }
            else{
                nlist.next = l1;
                l1 = l1.next;
            }
            nlist = nlist.next;
        }
        nlist.next = l1 == null ? l2 : l1;
        return head.next;
    }
}
```
## 22. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/description/)
```
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> lists = new ArrayList<>();
        helper(lists, "", 0, 0, n);
        return lists;
    }
    
    public void helper(List<String> lists, String s,int left, int right, int n){
        if (s.length() == 2*n){
            lists.add(s);
            return;
        }
        if (left < n){
            helper(lists, s+"(", left+1, right, n);
        }
        if (right < left){
            helper(lists, s+")", left, right+1, n);
        }
    }
}
```

## 23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description/)
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0)
            return null;
        if (lists.length == 1)
            return lists[0];
        return mergeTwoLists(mergeKLists(Arrays.copyOfRange(lists, 0, lists.length/2)),
                             mergeKLists(Arrays.copyOfRange(lists, lists.length/2, lists.length)));
    }
    public ListNode mergeTwoLists(ListNode l1, ListNode l2){
        ListNode dummy = new ListNode(0),
                 tail = dummy;
        while (l1 != null && l2 != null){
            if (l1.val > l2.val){
                tail.next = l2;
                l2 = l2.next;
            }
            else{
                tail.next = l1;
                l1 = l1.next;
            }
            tail = tail.next;
        }
        tail.next = l1 == null ? l2 : l1;
        return dummy.next;
    }
}
```

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0)
            return null;
        ListNode dummy = new ListNode(0),
                 tail = dummy;
        PriorityQueue <ListNode> queue = new PriorityQueue<>(lists.length, (l1, l2) -> l1.val-l2.val);
        for (ListNode node: lists){
            if (node != null)
                queue.offer(node);
        }
        while(!queue.isEmpty()){
            tail.next = queue.poll();
            tail = tail.next;
            if (tail.next != null)
                queue.offer(tail.next);
        }
        return dummy.next;
    }
}
```

## 24. [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null)
            return head;
        ListNode dummy = new ListNode(0),
                 origin = dummy,
                 pre = head, 
                 cur = head.next;
        while(pre != null && cur != null){
            dummy.next = cur;
            pre.next = cur.next;
            cur.next = pre;
            dummy = pre;
            pre = pre.next;
            if (pre != null)
                cur = pre.next;
        }
        return origin.next;
    }
}
```

## 25. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode cur = head;
        int count = k;
        while(cur != null && count != 0){
            cur = cur.next;
            count--;
        }
        if (count == 0){
            cur = reverseKGroup(cur, k);
            ListNode tmp = null;
            while(count++ < k-1){
                tmp = head.next;
                head.next = cur;
                cur = head;
                head = tmp;
            }
            head.next = cur;
        }
        return head;
    }
}
```
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0), start = dummy;
        dummy.next = head;
        while (true){
            ListNode n = start, p = start.next, mark = p;
            int count = k;
            while (n != null && count > 0){
                n = n.next;
                count--;
            }
            if (n == null) break;
            for (int i=0; i<k-1; i++){
                ListNode tmp = p.next;
                start.next = tmp;
                p.next = n.next;
                n.next = p;
                p = tmp;
            }
            start = mark;
        }
        return dummy.next;
    }
}
```

## 26. [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
```
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0)
            return 0;
        int j = 0;
        for (int i=0; i<nums.length; i++){
            if (nums[i] != nums[j])
                nums[++j] = nums[i];
        }
        return ++j;
    }
}
```
## 27. [Remove Element](https://leetcode.com/problems/remove-element/description/)
```
class Solution {
    public int removeElement(int[] nums, int val) {
        if (nums.length == 0)
            return 0;
        int j = 0;
        for (int i=0; i< nums.length; i++){
            if (nums[i] != val)
                nums[j++] = nums[i];
        }
        return j;
    }
}
```

## 28. [Implement strStr()](https://leetcode.com/problems/implement-strstr/description/)
```
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle.isEmpty())
            return 0;
        if (haystack.isEmpty())
            return -1;

        for(int i=0; i<haystack.length(); i++){
            if (haystack.length() - i < needle.length())
                break;
            for (int j=0; j<needle.length(); j++){
                if (haystack.charAt(i+j) != needle.charAt(j))
                    break;
                if (j == needle.length()-1)
                    return i;
            }
        }
        return -1;
    }
}
```

## 29. [Divide Two Integers](https://leetcode.com/problems/divide-two-integers/description/)
```
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1)
            return Integer.MAX_VALUE;
        int isNegative = (dividend ^ divisor) >> 31;
        int ans = helper(Math.abs(Long.valueOf(dividend)), Math.abs(Long.valueOf(divisor)));
        return isNegative == 0 ? ans : -ans; 
    }
    
    public int helper(long dividend, long divisor){
        if (dividend < divisor)
            return 0;
        int multiple = 1;
        long div = divisor;
        while((div + div) <= dividend){
            div += div;
            multiple += multiple;
        }
        return multiple + helper(dividend - div, divisor);
    }
}
```

## 30. [Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/description/)
```
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res = new ArrayList<>();
        if (s.isEmpty() || words.length == 0 || words[0].isEmpty())
            return res;
        HashMap<String, Integer> counts = new HashMap<>();
        for(String word: words){
            counts.put(word, counts.getOrDefault(word, 0)+1);
        }
        int num = words.length,
            len = words[0].length();
        for (int i=0; i+len*num<s.length()+1; i++){
            HashMap<String, Integer> hash = new HashMap<>();
            int j = 0;
            while(j<num){
                String word = s.substring(i+j*len, i+(j+1)*len);
                if (counts.containsKey(word)){
                    hash.put(word, hash.getOrDefault(word, 0)+1);
                    if (hash.get(word) > counts.get(word))
                        break;
                }
                else{
                    break;
                }
                j++;
            }
            if (j == num)
                res.add(i);
        }
        return res;
    }
}
```

## 31. [Next Permutation](https://leetcode.com/problems/next-permutation/description/)
```
2,3,6,5,4,1

Solution:
Step1, from right to left, find the first number which not increase in a ascending order. In this case which is 3.
Step2, here we can have two situations:

We cannot find the number, all the numbers increasing in a ascending order. This means this permutation is the last permutation, we need to rotate back to the first permutation. So we reverse the whole array, for example, 6,5,4,3,2,1 we turn it to 1,2,3,4,5,6.

We can find the number, then the next step, we will start from right most to leftward, try to find the first number which is larger than 3, in this case it is 4.
Then we swap 3 and 4, the list turn to 2,4,6,5,3,1.
Last, we reverse numbers on the right of 4, we finally get 2,4,1,3,5,6.
```
```
class Solution {
    public void nextPermutation(int[] nums) {
        boolean isGreatest = true;
        int p1 = -1;
        for(int i=nums.length-1; i>0; i--){
            if(nums[i] > nums[i-1]){
                p1 = i-1;
                isGreatest = false;
                break;
            }
        }
        if (isGreatest)
            reverse(nums, 0, nums.length-1);
        else{
            int p2 = -1;
            for (int i=nums.length-1; i>p1; i--){
                if (nums[i] > nums[p1]){
                    p2 = i;
                    break;
                }
            }
            swap(nums, p1, p2);
            reverse(nums, p1+1, nums.length-1);
        }
        
    }
    
    public void swap(int[] nums, int p1, int p2){
        int temp = nums[p1];
        nums[p1] = nums[p2];
        nums[p2] = temp;
    }
    
    public void reverse(int[] nums, int start, int end){
        while(start < end){
            swap(nums, start++, end--);
        }
    }
}
```

## 32. [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/description/)
```
Stack Solution:
The workflow of the solution is as below.

1. Scan the string from beginning to end.
2. If current character is '(',
push its index to the stack. If current character is ')' and the
character at the index of the top of stack is '(', we just find a
matching pair so pop from the stack. Otherwise, we push the index of
')' to the stack.
3. After the scan is done, the stack will only
contain the indices of characters which cannot be matched. Then
let's use the opposite side - substring between adjacent indices
should be valid parentheses.
4. If the stack is empty, the whole input
string is valid. Otherwise, we can scan the stack to get longest
valid substring as described in step 3.
```

```
class Solution {
    public int longestValidParentheses(String s) {
        Stack<Integer> stack = new Stack<>();
        for(int i=0; i<s.length(); i++){
            if (s.charAt(i) == '(')
                stack.push(i);
            else {
                if(!stack.isEmpty()){
                    if(s.charAt(stack.peek()) == '(')
                        stack.pop();
                    else
                        stack.push(i);
                }
                else
                    stack.push(i);
            }
        }
        int l = 0, a = s.length(), b = 0;
        
        while(!stack.isEmpty()){
            b = stack.pop();
            l = Math.max(l, a-b-1);
            a = b;
        }
        return Math.max(l, a);
        
    }
}
```

```
DP Solution:
My solution uses DP. The main idea is as follows: I construct a array longest[], for any longest[i], it stores the longest length of valid parentheses which is end at i.

And the DP idea is :

If s[i] is '(', set longest[i] to 0,because any string end with '(' cannot be a valid one.

Else if s[i] is ')'

     If s[i-1] is '(', longest[i] = longest[i-2] + 2

     Else if s[i-1] is ')' and s[i-longest[i-1]-1] == '(', longest[i] = longest[i-1] + 2 + longest[i-longest[i-1]-2]

For example, input "()(())", at i = 5, longest array is [0,2,0,0,2,0], longest[5] = longest[4] + 2 + longest[1] = 6.

```

```
class Solution {
    public int longestValidParentheses(String s) {
        int [] counts = new int [s.length()];
        int l = 0;
        for (int i=0; i<s.length(); i++){
            if(s.charAt(i) == '(' || i == 0)
                counts[i] = 0;
            else {
                if(s.charAt(i-1) == '('){
                    counts[i] = (i >= 2) ? (counts[i-2]+2) : 2;
                    l = Math.max(l, counts[i]);
                }
                else {
                    if( i-counts[i-1]-1 >= 0 && s.charAt(i-counts[i-1]-1) == '('){
                        counts[i] = counts[i-1] + 2 + ((i-counts[i-1]-2) >= 0 ? counts[i-counts[i-1]-2] : 0);   
                        l = Math.max(l, counts[i]);
                    }
                }
            }
        }
        return l;
    }
}
```
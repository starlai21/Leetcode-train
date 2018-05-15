# Leetcode-train

## 1. Two Sum
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
                result[0]=i;
                result[1] = a;
            }
        }
        return result;
    }
}
```

## 2. Add Two Numbers
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

## 3. Longest Substring Without Repeating Characters
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



## 4. Median of Two Sorted Arrays

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


## 5. Longest Palindromic Substring
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



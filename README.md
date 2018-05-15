# Leetcode-train

## 3 Longest Substring Without Repeating Characters
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



## 4 Median of Two Sorted Arrays

[A great explanation](https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn))-solution-with-explanation))


## 5 Longest Palindromic Substring
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



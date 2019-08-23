### Longest Common Subsequence

Given two strings text1 and text2, return the length of their longest common subsequence.

A subsequence of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A common subsequence of two strings is a subsequence that is common to both strings.

### Solution

The solution uses dynamic programming.

```
Let lcs[i][j] be the length of the longest common subsequence between the string str1[0...i] and str2[0...j].

lcs[i][j] = 1 + lcs[i-1][j-1] if str1[i] == str2[j]
			or
			= max(lcs[i-1][j], lcs[i][j-1]) if str1[i] != str2[j]
```

### Algorithm

```
n = str1.length
m = str2.length
lcs = matrix of size n+1 x m+1
for i = 1 to n
	for j = 1 to n
		if str1[i] == str2[j]
			lcs[i][j] = 1 + lcs[i-1][j-1]
		else 
			lcs[i][j] = max(lcs[i-1][j], lcs[i][j-1])
return lcs[n][m]
```
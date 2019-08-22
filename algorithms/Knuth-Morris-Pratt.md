## Knuth-Morris-Pratt algorithm

Given a text txt[0..n-1] and a pattern pat[0..m-1], write a function, search(char pat[], char txt[]), that prints all occurrences of pat[] in txt[]. You may assume that n > m.

### Preprocessing

The KMP algorithm requires an array _lps_ where

```
lps[i] = longest proper prefix of pat[0...i] which is also a suffix of pat[0...i]
```

### Preprocessing algorithm

```
i = 1
j = 0
n = txt.length
lps = array of length n
lps[0] = 0
while(i < n)
	if(txt[i] == pat[j])
		j++
		lps[i] = j
		i++
	else 
		if(j == 0)
			lps[i] = 0
			i++;
		else
			j = lps[j-1]
return lps
```

### Algorithm

```
i = 0
j = 0
n = txt.length
p = pat.length
lps = computeLPS(pat)
while(i < n)
	if(txt[i] == pat[j])
		i++
		j++
		if(j == p)
			print "pattern occurs at ", i - j
			j = lps[j-1]
	else 
		if(j == 0)
			i++
		else
			j = lps[j-1]
			
```

[Reference](https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/)

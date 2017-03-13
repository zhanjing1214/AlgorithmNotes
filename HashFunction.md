## Hash Function##

Leetcode上strStr这道题最初的解法是`O(m*n)`，若果需要进一步减小时间复杂度到`O(n)`，我们就要学习Rabin Karp这个算法。实际上他就是一个hash function。对于target，我们使用hash function求出一个hashCode，再对source的每个与target同长度的str求出其hashCode进行比较。

难点在于如何把每次移动并算出hashCode简化为O(1)，由于target长度是固定的，所以我们每往后移动一个就加上最后一个char，减掉最前面一个char。

```java
class Solution {
	// 可以自己设大小，越大则collision可能性越小  
    public int BASE = 1000000;
    
    /**
     * Returns a index to the first occurrence of target in source,
     * or -1  if target is not part of source.
     * @param source string to be scanned.
     * @param target string containing the sequence of characters to match.
     */
    public int strStr(String source, String target) {
      	//先处理一些corner case
        if (source == null || target == null || source.length() < target.length()) {
            return -1;
        }
        int targetLength = target.length();
        if (targetLength == 0) {
            return 0;
        }
        int sourceLength = source.length();
      	//计算在hash function中最高位要乘的数，为防止超出Integer范围，则边乘边取mod
        int power = 1;
        for (int i = 0; i < targetLength; i ++) {
            power = (power * 31) % BASE;
        }
        //计算target的hashCode
        int targetCode = 0;
        for (int i = 0; i < targetLength; i ++) {
            targetCode = (targetCode * 31 + target.charAt(i)) % BASE;
        }
      	//计算source的hashCode
        int hashCode = 0;
        for (int i = 0; i < sourceLength; i ++) {
            hashCode = (hashCode * 31 + source.charAt(i)) % BASE;
          	//若没到target长度，则继续加
            if (i < targetLength - 1) {
            	continue;
            }
          	//若大于等于target长度，则要减掉第一个元素
            if (i >= targetLength) {
                hashCode = (hashCode - power * source.charAt(i - targetLength)) % BASE;
              	//防止减掉的结果小于0
                if (hashCode < 0) {
                    hashCode += BASE;
                }
            }
            if (hashCode == targetCode) {
              	//由于会有collision，所以要double check
                if (source.substring(i - targetLength + 1, i + 1).equals(target)) {
                    return i - targetLength + 1;
                }    
            }
        }
        return -1;
    }
}
```

模式识别：
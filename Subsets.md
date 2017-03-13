## Subsets Problem##

- subsetsII

```java
class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsets2(int[] nums) {
      	// if duplicate in the nums, we need to sort first
        Arrays.sort(nums);
        ArrayList<ArrayList<Integer>> results = new ArrayList<>();
        subsetHelper(results, new ArrayList<Integer>(), nums, 0);
        return results;
    }
    
    // helper for the subset
  	// the position is the start index for this round as the subset problem has a suqence.
  	// the permutation is different as the we need to capture each sequence, we need to start from 0 each time.
    private void subsetHelper(ArrayList<ArrayList<Integer>> results, ArrayList<Integer> subset, int[] nums, int position) {
      	//根据题目要求限制条件add到results中
        results.add(new ArrayList<>(subset));
      	//注意i的起始值
        for (int i = position; i < nums.length; i ++) {
          	//是否需要查重，是否有duplicate
            if (i > 0 && i > position && nums[i] == nums[i-1]) {
                continue;
            }
            subset.add(nums[i]);
          	//注意下一个起始的position为i+1
            subsetHelper(results, subset, nums, i + 1);
          	//这一句都有的...
            subset.remove(subset.size() - 1);
        }
    }
}
```



- PermutationII

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of unique permutations.
     */
    public List<List<Integer>> permuteUnique(int[] nums) {
      List<List<Integer>> results = new ArrayList<>();
      //check edge cases first
      if (nums == null) {
        return results;
      }
      List<Integer> subset = new ArrayList<>();
      Arrays.sort(nums);
      // record the used elements
      boolean[] used = new boolean[nums.length];
      permuteHelper(nums, used, results, subset);
      return results;
    }
  	// helper of the permutation
    private void permuteHelper(int[] nums,
                               boolean[] used,
                               List<List<Integer>> results,
                               List<Integer> subset) {
      if (subset.size() == nums.length) {
        results.add(new ArrayList<>(subset));
        return;
      }
      //go through nums
      // 注意每次都是i = 0 开始，因为没有顺序
      for (int i = 0; i < nums.length; i ++) {
        //always start from the first duplicate
        // continue if used
        if (used[i] || (i > 0 && nums[i] == nums[i-1] && !used[i-1])) {
          continue;
        }
        used[i] = true;
        subset.add(nums[i]);
        permuteHelper(nums, used, results, subset);
        used[i] = false;
        subset.remove(subset.size() - 1);
      }
      
    }
}
```



- CombinationSum 
```java
public class Solution {
    /**
     * @param candidates: A list of integers
     * @param target:An integer
     * @return: A list of lists of integers
     */
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> results = new ArrayList<>();
        if (candidates == null || candidates.length == 0) {
            return results;
        }
        Arrays.sort(candidates);
        List<Integer> subset = new ArrayList<>();
        combinationHelper(results, subset, candidates, target, 0);
        return results;
    }
    
    //比较像排列问题，但可以reuse元素，所以每次call helper不需要startIndex不要+1
    private void combinationHelper(List<List<Integer>> results,
                                   List<Integer> subset,
                                   int[] candidates,
                                   int target,
                                   int startIndex) {
        if (target == 0) {
            results.add(new ArrayList<>(subset));
            return;
        }          
        if (target < 0) {
            return;
        }
        for (int i = startIndex; i < candidates.length; i ++) {
          	//跳过重复值，如122跳过第二个2
            if (i > startIndex && candidates[i] == candidates[i-1]) {
                continue;
            }
            subset.add(candidates[i]);
            combinationHelper(results, subset, candidates, target - candidates[i], i);
            subset.remove(subset.size() - 1);
        }
    }
}
```



- Letter Combinations of a Phone Number

```java
public class Solution {
  	// mapping numbers to string on the phone
    public String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    /**
     * @param digits A digital string
     * @return all posible letter combinations
     */
    public ArrayList<String> letterCombinations(String digits) {
        ArrayList<String> results = new ArrayList<>();
        if (digits == null || digits.length() == 0) {
            return results;
        }
        combinationHelper(digits, results, "", 0);
        return results;
    }
    
    
  	// 排列模板
    private void combinationHelper(String digits,
                                   ArrayList<String> results,
                                   String subString, 
                                   int index) {
        if (subString.length() == digits.length()) {
            results.add(subString);
            return;
        }
        String letters = map[Integer.parseInt(digits.charAt(index)+"")];
        for (int i = 0; i < letters.length(); i ++) {
          	//不能重复用，所以index+1
            //由于subsetString没有变，只是传多了一个char进去，所以不用再去掉了
            combinationHelper(digits, results, subString + letters.charAt(i), index+1);
        }
    } 
}
```



- Palindrome Partitioning

```java
public class Solution {
    /**
     * @param s: A string
     * @return: A list of lists of string
     */
    public List<List<String>> partition(String s) {
        List<List<String>> results = new ArrayList<>();
        if (s == null) {
            return results;
        }
        List<String> subset = new ArrayList<>();
        partitionHelper(results, subset, s, 0);
        return results;
    }
    
    
  	// 相当于排列问题的延伸
    private void partitionHelper(List<List<String>> results,
                                 List<String> subset,
                                 String s,
                                 int startIndex) {
        if (startIndex == s.length()) {
            results.add(new ArrayList<>(subset));
            return;
        }
        for (int i = startIndex; i < s.length(); i ++) {
            String subString = s.substring(startIndex, i + 1);
          	//判断subString是否为palidrome
          	//若不是则看continue看下一个构成是不是
            if (!isPalidrome(subString)) {
                continue;
            }
            subset.add(subString);
          	//注意传入index为i+1
            partitionHelper(results, subset, s, i + 1);
            subset.remove(subset.size() - 1);
        }
    }
    
    
    private boolean isPalidrome(String s) {
        for (int i = 0, j = s.length() - 1; i < j; i ++, j --) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
        }
        return true;
    }
}
```



- Restore IP Addresses
```java
public class Solution {
    /**
     * @param s the IP string
     * @return All possible valid IP addresses
     */
    public ArrayList<String> restoreIpAddresses(String s) {
       ArrayList<String> results = new ArrayList<>();
       if (s == null || s.length() < 4) {
           return results;
       }
       restoreHelper(results, "", s, 0);
       return results;
    }
    
    //与前一题基本一样，就是一个是subString判断是不是panlidrome，一个是判断是不是valid IP
    private void restoreHelper(ArrayList<String> results,
                               String subset,
                               String s,
                               int startIndex) {
        if (subset.split("\\.").length == 4) {
            if (startIndex == s.length()) {
                results.add(subset.substring(0, subset.length()-1));    
            }
            return;
        }
        for (int i = startIndex; i < s.length(); i ++) {
            String subString = s.substring(startIndex, i+1);
            if (!isValidIP(subString)) {
                continue;
            }
            restoreHelper(results, subset + subString + ".", s, i + 1);
        }
    }
    
    
    private boolean isValidIP(String s) {
        if (s.length() > 1 && s.startsWith("0")) {
            return false;
        }
        if (s.length() > 3 || Integer.parseInt(s) > 255) {
            return false;
        }
        return true;
    }
}
```




## 模式总结

上面的问题都是subsets问题的变种和延伸，首先搜索问题类似于组合的排序的时候，是考虑顺序的，即一定要按照某个顺序，所以需要一个startIndex的参数，每次的循环都从startIndex开始；而组合的问题则不需要考虑顺序，所以每次都从0开始，不要startIndex。总结模板如下：

```java
public List<String> subsetProblems(String s) {
  //1. 初始化resutls
  List<String> results = new ArrayList<>();
  //2. 处理一下edge case
  if (s == null) {
    return results; 
  }
  //3. 开subset & helper 以及有时候要sort数组
  String subset = "";
  subsetHelper(results, subset, s, 0); //要不要最后的index就看是组合问题还是排列问题
  //4. 返回结果
  return results;
} 


//写个private的helper
private void subsetHelper(List<List<String>> results,
                          String subset,
                          String s,
                          int startIndex) {
  //1. 根据题目条件把subset加到results中
  if (startIndex == s.length) {
    results.add(subset); // 若是添加list则要new ArrayList<>(subset)
    reurn;
  }
  //2. 开for循环开始recursive
  for (int i = startIndex; i < s.length; i ++) { //i的起始值看是排列(startIndex)还是组合(0)问题
    //2.1 判断subset是否符合条件 或者 判断跳过重复值
    String subString = s.subString(startIndex, i+1);
    if (!isValid(subString)) { //跳过invalid的chunk，判断是否valid可以单独写成一个方法
      continue;
    }
    //2.2 把subString加到subset中
    subset.add(subString);
    //2.3 递归helper，注意传入和startIndex，若可重复用则不加1，反之则+1
    subsetHelper(results, subset, s, i+1);
    //2.4 减掉subset中最后一个元素
    subset.remove(subset.size() - 1);
  }
}
```


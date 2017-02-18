## Leet Code Courses Note ##
### Chapter 1. ARRAY/STRING ###

**I. Two pointer method**

One slow-runner and the other fast-runner.

Always be used in already SORTED array.

**167. Two Sum II**

*My Solution:*
 We could still apply the [Hash table] approach, but it costs us O(n) extra space, plus it does not make use of the fact that the input is already sorted.

*Clean Code Solution*

Two Pointers Method!

Three possibilities:
i. Ai + Aj > target. Increasing i isn’t going to help us, as it makes the sum even bigger. Therefore we should decrement j.

ii. Ai + Aj < target. Decreasing j isn’t going to help us, as it makes the sum even smaller. Therefore we should increment i.

iii. Ai + Aj == target. We have found the answer.
    
    public int[] twoSum(int[] numbers, int target) {
	// Assume input is already sorted.
	int i = 0, j = numbers.length - 1;
	while (i < j) {
		int sum = numbers[i] + numbers[j];
		if (sum < target) {
			i++;
		} else if (sum > target) {
			j--;
		} else {
			return new int[] { i + 1, j + 1 };//在返回的时候直接new新数组并赋值！
		}
	}
	throw new IllegalArgumentException("No two sum solution");
	}

**26. Remove Duplicates**

*Top Answer:*

    public class Solution {
    public int removeDuplicates(int[] nums) {
        int j=0;
        for(int i=0; i<nums.length; i++){
            if(nums[i]!=nums[j]){
                j++;
                nums[j]=nums[i];
            }
        }
        return j+1;
    }
	}


**II. Hash Table**

A. Use array as a counter.

B. Unicode

One Char = 2 Bytes = 16 bits = 4bits*4 = 4 hexadecimal to present one unicode.

*Example:*

    private static void printFreq(char[] str) {
    Map<Character, Integer> freq = new HashMap<>();

	//第一个variable Key是字符，第二个variable Value是相同字符对应的在map中的个数
    for (int i = 0; i < str.length; i++) {
        if (freq.containsKey(str[i])) {
            freq.put(str[i], freq.get(str[i]) + 1);
        } else {
            freq.put(str[i], 1);
        }
    }
    for (Map.Entry<Character, Integer> entry : freq.entrySet()) {
        System.out.println("[" + (char)(entry.getKey()) + "] = " + entry.getValue());
    }
	}

	public static void main(String[] args) {
    	char[] str = "◣⚽◢⚡◣⚾⚡◢".toCharArray();
    	printFreq(str);
	}

**242. Valid Anagram**

*My Solution:*

    public class Solution {
    public boolean isAnagram(String s, String t) {
        Map<Character, Integer> fre = new HashMap<Character, Integer>();
        char[] str1 = s.toCharArray();
        char[] str2 = t.toCharArray();
        if(str1.length!=str2.length) return false;
        for(int i=0; i<str1.length; i++){
            if(fre.containsKey(str1[i])){
                fre.put(str1[i], fre.get(str1[i])+1);
            }else{
                fre.put(str1[i],1);
            }
        }
        for(int j=0; j<str2.length; j++){
            if(fre.containsKey(str2[j])){
                fre.put(str2[j],fre.get(str2[j])-1);
                if(fre.get(str2[j])<0) return false;
            }else{
                return false;
            }
        }
        return true;
    }
	}

*Top Answer:*

	public boolean isAnagram(String s, String t) {
    	if (s.length() != t.length()) {
        	return false;
    	}
    	int[] counter = new int[26];
		for (int i = 0; i < s.length(); i++) {
        	counter[s.charAt(i) - 'a']++;
        	counter[t.charAt(i) - 'a']--;
    	}
    	for (int count : counter) {
        	if (count != 0) {
            	return false;
        	}
    	}
    	return true;
	}

Time complexity : O(n); Space complexity : O(1).

*注：*

1. String or Array, length: s.length(); a.length;
2. s.charAt(i) 可直接作为index, 因为转成ASK码。 但一定记住减去‘a’，否则数组第一个index不是从0计数，会out of range。

*Top Answer 2:*

    public boolean isAnagram(String s, String t) {
    	if (s.length() != t.length()) {
        	return false;
    	}
    	char[] str1 = s.toCharArray();
    	char[] str2 = t.toCharArray();
    	Arrays.sort(str1);
    	Arrays.sort(str2);
    	return Arrays.equals(str1, str2);
	}

Time complexity : O(nlogn). Space complexity : O(1).

**3. Longest Substring Without Repeating Char[Medium!]**

*Top Answer*
    
	public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); 
        for (int i = 0, j = 0; i < n; i++) {
            if (map.containsKey(s.charAt(i))) {
                j = Math.max(map.get(s.charAt(i))+1, j); 
				//注1：+1是指向前一个重复值的后一个值，ex: qwq 虽然遇到第二个q时i需要变化,但 是从第一个q后面的w开始往后找substring。
				//注2：pwwkp,i找到第二个p时，j此时在w，若取第一个p后面一个index时，会退回w，所以应取两者最大值。而这里也与下面一行的ans取max有关。
            }
            ans = Math.max(ans, i - j + 1);
            map.put(s.charAt(i), i );
        }
        return ans;
    }
	}

*注：*

1. 可以不将s转换成char[] array: s.toCharArray(); 可用：s.charAt(i) 来代替。
2. 
	

**III String Manipulation**

Concatenation works by first allocating enough space for the new string, copy the contents from the old string and append to the new string.
So use **StringBuilder** class to **append** strings together.

Python equivalent is to collect the parts in a list.
"+" operator in Python.



### Chatper 2  Liked List###

**237. Delete Node in a Linked List**

1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4

*My solution:*

    /**
 	* Definition for singly-linked list.
 	* public class ListNode {
 	*     int val;
 	*     ListNode next;
 	*     ListNode(int x) { val = x; }
 	* }
 	*/
	public class Solution {
    	public void deleteNode(ListNode node) {
        	node.val = node.next.val;
        	node.next = node.next.next;
    	}
	}

**206. Reverse Linked List**

*Iterative Solution:*

The linked list 1 → 2 → 3 → Ø, we would like to change it to Ø ← 1 ← 2 ← 3. 注意 是指针方向从最后一个element指向第一个 再指向null。 Iterative 的方法是从头到尾一个个改变指针方向。

    public class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        ListNode temp = null;
        while(cur!=null){
            temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
	}

*Recursive Solution:*
	
Explanation video:
[https://www.youtube.com/watch?v=MRe3UsRadKw](https://www.youtube.com/watch?v=MRe3UsRadKw)

	public class Solution {
    	public ListNode reverseList(ListNode head) {
        	if(head==null || head.next==null) return head;
			//used to 1.find the end 2.return the null list at first.
        	
			ListNode p = reverseList(head.next); 
			//recursively searching till find the end of the list.(停在倒数第二个数上，而p是最终指向倒数第一个数，因为是return了head.next)
        	
			//找到最后一个数后，recursively从最后到最前更改list指针方向。
        	head.next.next = head;//将head后的一个指针的next指向head
        	head.next = null;//删掉head的next指针
        	return p;//返回整个list，p是指向新的head，也就是旧list的最后一个数
    	}
	}	

**141. Linked List Cycle**

*HashSet Solution:*

    public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<ListNode>();
        while(head!=null){
            if(seen.contains(head)) return true;
            else{
                seen.add(head);
            }
            head = head.next;
        }
        return false;
    }
	}

*注：*

Set是<ListNode>存储的！

**21. Merge Two Sorted Lists**

*Top Solution:*

	public class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }//The ending part
        
        ListNode mergeHead;//Head是reference value，所以recursively返回head的时候，head是new的。
        if(l1.val < l2.val){
            mergeHead = l1;
            mergeHead.next = mergeTwoLists(l1.next, l2);
        }
        else{
            mergeHead = l2;
            mergeHead.next = mergeTwoLists(l1, l2.next);
        }
        return mergeHead;//最后溯源到第一个head的时候，它实际已经被串成了merge好的list.
    }
	}

**24. Swap Nodes in Pairs**

*注：*

	while(cur!=null){
            if(cur.next==null) break;
            ListNode temp = cur;
            cur = cur.next;
            cur.next = temp;
            cur = cur.next;
     }
这是完全不正确的swap方式！因为要考虑cur之前的对象要指向cur

*Top Solution:*

    public class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode all = new ListNode(0);
        all.next = head;
        ListNode cur = all;
		//cur的位置在需要swap的两个数前，这样能保证有三个已知数，cur; cur.next; cur.next.next。 这个方法速度快很多
        while(cur.next!=null&&cur.next.next!=null){
            ListNode temp = cur.next.next;
            cur.next.next = temp.next;
            temp.next = cur.next;
            cur.next = temp;
            cur = cur.next.next;
        }
        return all.next;
    }
	}

*Another Solution:*

    public class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode all = new ListNode(0);
        all.next = head;
        ListNode cur = all.next;//current pointer
        ListNode pre = all;
		//用了pre和cur，循环记录pre
        while(cur!=null&&cur.next!=null){
            ListNode temp = cur.next;
            cur.next = temp.next;
            temp.next = cur;
            pre.next = temp;
            pre = cur;
            cur = cur.next;
        }
        return all.next;
    }
	}

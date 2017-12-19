# Remove K digitals

## Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```


```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```

## Solution - Stack

```

1  4  3  2  2  1  9


stack:

|     |
|__1__|

1 < 4 -> push 4 -> i++

stack:

|  4  |
|__1__|

3 < 4 -> pop() -> k-- -> push 3 -> i++

stack:

|  2  |
|__1__|

2 < 3 -> pop() -> k-- -> push 2 -> i++
2 = 2 -> push 2 -> i ++

stack:
|  2  |
|  2  |
|__1__|

1 < 2 - > pop() -> k-- -> push 1 -> i++
stack:
|  9  |
|  1  |
|  2  |
|__1__|

-> Transfor to Sting and reserve

```

```java
class Solution {
    public String removeKdigits(String num, int k) {
        if(k == num.length()) {
            return "0";
        }

        Stack<Character> stack = new Stack<>();
        // pointer to scan the string
        int i = 0;
        while(i < num.length()) {
            // If the next digit less then the previous one, pop out the pre and Offer the new one
            while(k > 0 && !stack.isEmpty() && stack.peek() > num.charAt(i)) {
                stack.pop();
                // Remove K number
                k -- ;
            }

            stack.push(num.charAt(i));
            // Move to the next number
            i ++;
        }

        // Deal with the corner case like "1111"
            while (k > 0) {
                stack.pop();
                k --;
            }
        // Construct the number from stack to String
            StringBuilder sb = new StringBuilder();
             while(!stack.isEmpty()) {
                sb.append(stack.pop());
            }

            sb.reverse();

             // Remove all the 0 at head ex. 00400

        while(sb.length()>1 && sb.charAt(0) == '0') {
            sb.deleteCharAt(0);
        }

        return sb.toString();   
    }
}
```
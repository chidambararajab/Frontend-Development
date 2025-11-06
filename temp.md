### Parenthesis Matching

**Use**: Validating properly nested expressions in code editors, mathematical expressions, or JSON parsers.

```jsx
function isBalancedParentheses(str) {
  const stack = [];
  
  // Maps closing brackets to their corresponding opening brackets
  const closingBrackets = {
    ')': '(',
    '}': '{',
    ']': '['
  };
  
  // Set of opening brackets for quick lookup
  // Sets are optimized for membership checking with the .has() method, which operates in O(1) time complexity (constant time). 
  const openingBrackets = new Set(['(', '{', '[']);
  
  for (let char of str) {
    // If it's an opening bracket, push to stack
    if (openingBrackets.has(char)) {
      stack.push(char);
    }
    // If it's a closing bracket
    else if (closingBrackets[char]) {
      // Pop from stack and check if it matches the corresponding opening bracket
      const lastBracket = stack.pop();
      
      // If stack is empty or brackets don't match
      if (lastBracket !== closingBrackets[char]) {
        return false;
      }
    }
    // Ignore any other characters that aren't brackets
  }
  
  // Stack should be empty if all brackets are matched
  return stack.length === 0;
}
```

---

### Longest Palindromic Substring

**Use**: Text analysis, pattern matching algorithms, and certain string compression techniques.

### Merge Sorted Arrays

**Use**: Database merging operations, combining search results, and implementing efficient sorting algorithms.

```jsx
/**
 * Merges two sorted arrays into a single sorted array
 * @param {number[]} arr1 - First sorted array
 * @param {number[]} arr2 - Second sorted array
 * @returns {number[]} - Merged sorted array
 */
function mergeSortedArrays(arr1, arr2) {
  let result = [];
  let i = 0;
  let j = 0;
  
  // Compare elements from both arrays and add the smaller one to result
  while (i < arr1.length && j < arr2.length) {
    if (arr1[i] < arr2[j]) {
      result.push(arr1[i]);
      i++;
    } else {
      result.push(arr2[j]);
      j++;
    }
  }
  
  // Add remaining elements from arr1 if any
  while (i < arr1.length) {
    result.push(arr1[i]);
    i++;
  }
  
  // Add remaining elements from arr2 if any
  while (j < arr2.length) {
    result.push(arr2[j]);
    j++;
  }
  
  return result;
}

// Example usage:
// const array1 = [1, 3, 5, 7, 9];
// const array2 = [2, 4, 6, 8, 10];
// console.log(mergeSortedArrays(array1, array2));
// Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

---

### Count Occurrences of Each Char

```jsx
function countCharacters(str) {
    const arr =str.split("");
    
    return arr.reduce((acc,cur) => {
        acc[cur] = (acc[cur] || 0) + 1;
        return a;
    }, {});
}

const word = "aaaabbbbbbcccccccdd";
const result = countCharacters(word);

console.log(result);
// Output: { a: 4, b: 6, c: 7, d: 2 }

// To get specific counts:
console.log(`a: ${result['a']}`); // a: 4
console.log(`b: ${result['b']}`); // b: 6
console.log(`c: ${result['c']}`); // c: 7
```

### Word Frequency Count: Count the occurrences of each Word in a paragraph.

```jsx
function wordFrequencyCount(paragraph) {
  const arrayOfWords = paragraph
    .toLowerCase() // Convert to lowercase to count words case-insensitively
    .replace(/[^a-z\s]/g, "") // Remove punctuation
    .split(/\s+/); // Split by whitespace
	   
    return arrayOfWords.reduce((acc,cur) => {
        acc[cur] = (acc[cur] || 0) + 1;
        return acc;
    }, {});
}

// Example Usage:
const paragraph = "Hello world! Hello, Freshworks. Freshworks is great!";
console.log(wordFrequencyCount(paragraph));
```

### **Output:**

```json
{
  "hello": 2,
  "world": 1,
  "freshworks": 2,
  "is": 1,
  "great": 1
}
```

### **Explanation:**

1. **Convert to Lowercase**: Ensures that "Hello" and "hello" are treated as the same word.
2. **Remove Punctuation**: Cleans the text to only include words.
3. **Split into Words**: Uses whitespace as the delimiter.
4. **Count Occurrences**: Iterates through the array and maintains a frequency count.

This approach efficiently counts word occurrences in O(n) time complexity.

### **1️⃣ Longest Substring Without Repeating Characters**

**Problem:** Determine the length of the longest substring in a given string that contains all unique characters.

**Solution:**

```jsx
function lengthOfLongestSubstring(s) {
  let seen = new Map();
  let start = 0;
  let maxLength = 0;

  for (let end = 0; end < s.length; end++) {
    if (seen.has(s[end])) {
      start = Math.max(seen.get(s[end]) + 1, start);
    }
    seen.set(s[end], end);
    maxLength = Math.max(maxLength, end - start + 1);
  }

  return maxLength;
}

// Example usage:
console.log(lengthOfLongestSubstring("abcabcbb")); // Output: 3 ("abc")
console.log(lengthOfLongestSubstring("bbbbb"));    // Output: 1 ("b")
```

**Explanation:** This approach utilizes a sliding window technique with two pointers (`start` and `end`) to track the current substring. A `Map` stores the last seen index of each character. As we iterate through the string, if a character is repeated, we adjust the `start` pointer to ensure all characters in the window are unique.

---

### **2️⃣ Longest Palindromic Substring**

**Problem:** Find the longest substring in a given string that reads the same forward and backward.

**Solution:**

```jsx
function longestPalindrome(s) {
  if (s.length < 2) return s;

  let start = 0, maxLength = 1;

  function expandAroundCenter(left, right) {
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      left--;
      right++;
    }
    return right - left - 1;
  }

  for (let i = 0; i < s.length; i++) {
    let len1 = expandAroundCenter(i, i);   // Odd length palindromes
    let len2 = expandAroundCenter(i, i + 1); // Even length palindromes
    let len = Math.max(len1, len2);

    if (len > maxLength) {
      start = i - Math.floor((len - 1) / 2);
      maxLength = len;
    }
  }

  return s.substring(start, start + maxLength);
}

// Example usage:
console.log(longestPalindrome("babad")); // Output: "bab" or "aba"
console.log(longestPalindrome("cbbd"));  // Output: "bb"
```

**Explanation:** This solution employs the "expand around center" technique. For each character (and each pair of characters for even-length palindromes), it expands outward while the characters on both sides are equal, thus identifying the longest palindromic substring centered at that position.

---

### **3️⃣ String Concatenation**

**Problem:** Demonstrate how to concatenate two or more strings in JavaScript.

**Solution:**

```jsx
let str1 = "Hello";
let str2 = "World";

// Using the + operator
let result1 = str1 + " " + str2;
console.log(result1); // Output: "Hello World"

// Using the concat() method
let result2 = str1.concat(" ", str2);
console.log(result2); // Output: "Hello World"
```

**Explanation:** In JavaScript, strings can be concatenated using the `+` operator or the `concat()` method. Both methods combine multiple strings into one.

### **1️⃣ Check if Two Strings are Anagrams**

👉 Two words are **anagrams** if they have the same characters in the same frequency but in a different order.

**Example:** `"listen"` and `"silent"` are anagrams.

### **Solution**

```jsx
const isAnagram = (str1, str2) => {
  if (str1.length !== str2.length) return false;

  const sortedStr1 = str1.toLowerCase().split("").sort().join("");
  const sortedStr2 = str2.toLowerCase().split("").sort().join("");

  return sortedStr1 === sortedStr2;
};

console.log(isAnagram("listen", "silent")); // true
console.log(isAnagram("hello", "world"));   // false
```

✅ **Time Complexity:** `O(n log n)` (because of sorting)

✅ **Space Complexity:** `O(n)`

---

## **2️⃣ Reverse a String**

👉 Reverse the given string **without using built-in reverse()**.

### **Solution**

```jsx
javascript
CopyEdit
const reverseString = (str) => {
  let reversed = "";
  for (let i = str.length - 1; i >= 0; i--) {
    reversed += str[i];
  }
  return reversed;
};

console.log(reverseString("hello")); // "olleh"

```

✅ **Time Complexity:** `O(n)`

✅ **Space Complexity:** `O(n)`

---

## **3️⃣ Check if a String is a Palindrome**

👉 A **palindrome** is a word that reads the same forward and backward.

**Example:** `"racecar"`, `"madam"`

### **Solution**

```jsx
javascript
CopyEdit
const isPalindrome = (str) => {
  return str === str.split("").reverse().join("");
};

console.log(isPalindrome("racecar")); // true
console.log(isPalindrome("hello"));   // false

```

✅ **Time Complexity:** `O(n)`

✅ **Space Complexity:** `O(n)`

---

## **4️⃣ Find the First Non-Repeating Character**

👉 Find the **first character that appears only once** in the string.

**Example:** `"swiss"` → **First non-repeating character = `'w'`**

### **Solution**

```jsx
javascript
CopyEdit
const firstNonRepeatingChar = (str) => {
  const charMap = {};

  for (let char of str) {
    charMap[char] = (charMap[char] || 0) + 1;
  }

  for (let char of str) {
    if (charMap[char] === 1) return char;
  }

  return null;
};

console.log(firstNonRepeatingChar("swiss")); // "w"
console.log(firstNonRepeatingChar("aabbcc")); // null

```

✅ **Time Complexity:** `O(n)`

✅ **Space Complexity:** `O(n)`

---

## **5️⃣ Count Occurrences of Each Character**

👉 Count how many times each character appears in a string.

### **Solution**

```jsx
javascript
CopyEdit
const countCharacterFrequency = (str) => {
  const frequencyMap = {};

  for (let char of str) {
    frequencyMap[char] = (frequencyMap[char] || 0) + 1;
  }

  return frequencyMap;
};

console.log(countCharacterFrequency("hello"));
// { h: 1, e: 1, l: 2, o: 1 }

```

✅ **Time Complexity:** `O(n)`

✅ **Space Complexity:** `O(n)`

---

## **6️⃣ Find All Substrings of a String**

👉 Generate **all possible substrings** of a given string.

### **Solution**

```jsx
javascript
CopyEdit
const getAllSubstrings = (str) => {
  let substrings = [];

  for (let i = 0; i < str.length; i++) {
    for (let j = i + 1; j <= str.length; j++) {
      substrings.push(str.substring(i, j));
    }
  }

  return substrings;
};

console.log(getAllSubstrings("abc"));
// [ 'a', 'ab', 'abc', 'b', 'bc', 'c' ]

```

✅ **Time Complexity:** `O(n²)`

✅ **Space Complexity:** `O(n²)`

---

## **7️⃣ Find the Longest Common Prefix**

👉 Find the **longest prefix** that is common in an array of strings.

**Example:** `["flower", "flow", "flight"]` → **Output: `"fl"`**

### **Solution**

```jsx
javascript
CopyEdit
const longestCommonPrefix = (strs) => {
  if (!strs.length) return "";

  let prefix = strs[0];

  for (let i = 1; i < strs.length; i++) {
    while (strs[i].indexOf(prefix) !== 0) {
      prefix = prefix.slice(0, -1);
      if (!prefix) return "";
    }
  }

  return prefix;
};

console.log(longestCommonPrefix(["flower", "flow", "flight"])); // "fl"
console.log(longestCommonPrefix(["dog", "racecar", "car"])); // ""

```

✅ **Time Complexity:** `O(n * m)` (where `n` = number of strings, `m` = length of the shortest string)

✅ **Space Complexity:** `O(1)`

---

## **8️⃣ Find if One String is a Rotation of Another**

👉 Check if one string is a **rotation** of another string.

**Example:** `"waterbottle"` is a rotation of `"erbottlewat"`

### **Solution**

```jsx
javascript
CopyEdit
const isRotation = (s1, s2) => {
  if (s1.length !== s2.length) return false;
  return (s1 + s1).includes(s2);
};

console.log(isRotation("waterbottle", "erbottlewat")); // true
console.log(isRotation("hello", "llohe")); // true
console.log(isRotation("hello", "olelh")); // false

```

✅ **Time Complexity:** `O(n)`

✅ **Space Complexity:** `O(n)`

---

## **9️⃣ Find the Most Frequent Character**

👉 Find the character that appears **the most** in a string.

### **Solution**

```jsx
javascript
CopyEdit
const mostFrequentChar = (str) => {
  const freqMap = {};
  let maxChar = "", maxCount = 0;

  for (let char of str) {
    freqMap[char] = (freqMap[char] || 0) + 1;
    if (freqMap[char] > maxCount) {
      maxCount = freqMap[char];
      maxChar = char;
    }
  }

  return maxChar;
};

console.log(mostFrequentChar("aabbbccccd")); // "c"

```

✅ **Time Complexity:** `O(n)`

✅ **Space Complexity:** `O(n)`


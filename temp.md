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

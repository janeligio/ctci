# IX. Interview Questions

## 1. Arrays and Strings

** 1.1 Is Unique **
- Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?

JavaScript

Using Set data structure

`
const isUnique = (str) => {
	const uniqStr = new Set(str);
	return uniqStr.size === str.length;
}
`


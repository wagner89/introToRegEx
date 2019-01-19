# Introduction to Regular Expressions (RegEx)

```
Some people, when confronted with a problem, think 
“I know, I’ll use regular expressions.” Now they have two problems.
``` 
_source - Stack Exchange ([right here](https://softwareengineering.stackexchange.com/questions/223634/what-is-meant-by-now-you-have-two-problems))_

### Contents:

## 1. What is a Regular Expression

## 2. What is a RegEx good for
## 3. Fundamentals
## 4. Groups
## 5. Advanced topics
## 6. Tips and tricks
## 7. Resources


## 8. When to use RegEx

## 9. When not to use RegEx

### a. When there is a more readable solution
  ** Example: check if a string is lowercase**

   `var isLowercase = myString === myString.toLowerCase();`


   instead of


   `var isLowercase = /^[a-z]*$/.test(myString);`
     
### b. When the regular expression is costly
   **Example: check if a string is lowercase**

   `var isLowercase = myString === myString.toLowerCase();`


   instead of


   `var isLowercase = /^[a-z]*$/.test(myString);`

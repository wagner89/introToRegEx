# Introduction to Regular Expressions (RegEx)

```
Some people, when confronted with a problem, think 
“I know, I’ll use regular expressions.” Now they have two problems.
``` 
_source - Stack Exchange ([right here](https://softwareengineering.stackexchange.com/questions/223634/what-is-meant-by-now-you-have-two-problems))_

## 1. What is a Regular Expression

**Short answer:** a RegEx is a string, which defines a search pattern. We can then search for the described pattern in other strings, bodies of text, what have you.

**Long story:** RegExes are interpreted as being written in a language of their own, which allows for quite complex patterns to be defined. They are usually a combination of a pattern string and a set of flags, which define how the pattern should be interpreted. The patterns can range from simple wildcards to find strings of a given format within a text, or validate a format, to complex expression containing extractable groups, ranges, etc.

## 2. What is a RegEx good for

## 3. Fundamentals

## 4. Groups

## 5. Advanced topics

## 6. JavaScript specifics

## 7. When to use RegEx

## 8. When not to use RegEx

### a. When there is a more readable solution
  **Example: check if a string is lowercase**

   `var isLowercase = myString === myString.toLowerCase();`


   instead of


   `var isLowercase = /^[a-z]*$/.test(myString);`
     
### b. When the regular expression is costly
   **Example: check if a string is lowercase**

   `(x+x+)y+`
   
## 9. Tips and tricks
## 10. Resources

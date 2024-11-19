# ParaLLM
*work in process, not yet implemented*

## Concept
The idea of iterator is to be able to take some iterable, and allow an agent to interact with each element of the iterable based on a prompt. Essentially, this would allow concurrent operations of LLMs applied to a set of like elements.

Take the following example. You want to peruse a codebase and find any example of a particularly bad practice, such as using magic numbers rather than named constants.

```python
# Bad
if user_age > 18:
    print("Adult")

# Good
ADULT_AGE_THRESHOLD = 18
if user_age > ADULT_AGE_THRESHOLD:
    print("Adult")
```

With a standard agent, it would probably perform the operation the same way you would. Step through each file, check for this antipattern, if you find it, record it or change it. However, this linear process is not very efficient, and not particularly scalable. In a large codebase, you might need to scan thousands of files, and doing this one at a time is not very efficient.

This is where Iterator comes in. In such a case as this, the Iterator would be capable of looking at all files in parallel, and supplied with a prompt, apply an operation, such as checking for magic numbers, to each file.


For more information on what this library might look like in practice [this discussion](./discussions/interface.md)

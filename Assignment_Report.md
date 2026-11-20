# Assignment 5 – Modular, Memory-Aware and Persistent Program Engineering

**Student:** Alan Godwin J  
**Register Number:** 1921260244  
**Repository:** `CSA02-1921260244-Assignment5`  
**GitHub link:** [ADD AFTER CREATING THE REPOSITORY]

## Submission note
The task folders contain C implementations and captured outputs. The report below provides concise design and complexity notes. Measurements shown by the programs are sample local executions, not claims of large-scale benchmarking. Add your own terminal screenshots and timing tables on your Mac before submission.

## Part A – Design
Decompose the application into registration, eligibility checking, donor search, stock update, persistence and reporting functions. Use prototypes that make input-only pointers `const`; use output pointers when a function must return more than one result. A structure return is often clearer than multiple output parameters. Global variables increase coupling and make unit testing harder.

For digital root, recurse by summing digits until a single digit remains; a loop or `1+(n-1)%9` for positive n avoids recursive stack use. Linear power recursion makes O(n) calls and O(n) stack use; exponentiation by squaring makes O(log n) calls and stack use. Pointer arithmetic on `int *p=stock` advances in units of `sizeof(int)`. Returning an address of a local variable is invalid because its lifetime ends when the function returns.

A growable donor list should double capacity. Store `realloc` into a temporary pointer so failure does not lose the original allocation. Doubling gives amortized O(1) append and O(n) total copying over n appends. Fixed increments can cause O(n²) total copying. Store donor records as text for readability and fixed stock records as binary for direct record access, while documenting portability limits.

## Part B – Implementation
B1 implements linear and halving power with call counts and a digital-root function. For exponent 30, linear recursion makes 31 calls; halving recursion makes 6 calls. B3 updates one binary record by seeking to its byte offset, reading, modifying, seeking back and writing. An out-of-range record fails at `fread` without overwriting the file. B4 grows a donor array with `realloc` and frees it at program end.

## Experimental and production decision
Run the programs with ordinary, boundary and adversarial inputs. Use `-fsanitize=address` to check memory safety. For a small deployment, loading donors into a dynamic array gives fast O(1) indexed access after loading. At district scale, batch reads or a separate sorted index can reduce memory use; binary search over an index file uses O(log n) file reads and O(1) in-memory index space, at the cost of disk I/O.

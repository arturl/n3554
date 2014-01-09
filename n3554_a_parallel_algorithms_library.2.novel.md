# Novel Algorithms Introduced by this Proposal

## Novel specialized algorithms from `<memory>`

### None.

## Novel algorithms from `<algorithm>`

### Header `<algorithm>` synopsis

```
namespace std {
  // non-modifying sequence operations
  template<class ExecutionPolicy,
           class InputIterator, class Function>
    InputIterator for_each(ExecutionPolicy &&exec,
                           InputIterator first, InputIterator last,
                           Function f);
  template<class InputIterator, class Size, class Function>
    Function for_each_n(InputIterator first, Size n,
                        Function f);
  template<class ExecutionPolicy,
           class InputIterator, class Size, class Function>
    InputIterator for_each_n(ExecutionPolicy &&exec,
                             InputIterator first, Size n,
                             Function f);
}
```

Novel non-modifying sequence operations

### For each `[alg.foreach]`

```
template<class InputIterator, class Size n, class Function>
  Function for_each_n(InputIterator first, InputIterator last,
                      Function f);
```

1. *Requires:* `Function` shall meet the requirements of `MoveConstructible` [*Note:* `Function need not meet the requirements of `CopyConstructible`. -- *end note*]

2. *Effects:* Applies `f` to the result of dereferencing every iterator in the range `[first,first + n)`, starting from `first` and proceeding to `first + n - 1`.
    [*Note:* If the type of `first` satisfies the requirements of a mutable iterator, `f` may apply nonconstant functions throughthe dereferenced iterator. -- *end note*]

3. *Returns:* `std::move(f)`.

4. *Complexity:* Applies `f` exactly `n` times.

5. *Remarks:* If `f` returns a result, the result is ignored.

```
template<class ExecutionPolicy,
         class InputIterator, class Function>
  InputIterator for_each(ExecutionPolicy &&exec,
                         InputIterator first, InputIterator last,
                         Function f);

template<class ExecutionPolicy,
         class InputIterator, class Size, class Function>
  InputIterator for_each_n(ExecutionPolicy &&exec,
                           InputIterator first, Size n,
                           Function f);
```

1. *Effects:* The first algorithm applies `f` to the result of dereferencing every iterator in the range `[first,last)`.
   The second algorithm applies `f` to the result of dereferencing every iterator in the range `[first,first+n)`.
   The execution of the algorithm is parallelized as determined by `exec`. [*Note:* If the type of `first` satisfies
   the requirements of a mutable iterator, `f` may apply nonconstant functions through the dereferenced
   iterator. -- *end note*]

2. *Returns:* `for_each` returns `last` and `for_each_n` returns `first + n` for non-negative values of `n` and `first` for negative values.

3. *Complexity:* `O(last - first)` or `O(n)`.

4. *Remarks:* If `f` returns a result, the result is ignored.

    The signatures shall not participate in overload resolution if `is_execution_policy<ExecutionPolicy>::value` is `false`.

## Novel generalized numeric operations from `<numeric>`

### Header `<numeric>` synopsis

```
namespace std {
  template<class InputIterator>
    typename iterator_traits<InputIterator>::value_type
      reduce(InputIterator first, InputIterator last);
  template<class ExecutionPolicy,
           class InputIterator>
    typename iterator_traits<InputIterator>::value_type
      reduce(ExecutionPolicy &&exec,
             InputIterator first, InputIterator last);
  template<class InputIterator, class T>
    T reduce(InputIterator first, InputIterator last T init);
  template<class ExecutionPolicy,
           class InputIterator, class T>
    T reduce(ExecutionPolicy &&exec,
             InputIterator first, InputIterator last, T init);
  template<class InputIterator, class T, class BinaryOperation>
    T reduce(InputIterator first, InputIterator last, T init,
             BinaryOperation binary_op);
  template<class ExecutionPolicy,
           class InputIterator, class T, class BinaryOperation>
    T reduce(ExecutionPolicy &&exec,
             InputIterator first, InputIterator last, T init,
             BinaryOperation binary_op);

  template<class InputIterator, class OutputIterator>
    OutputIterator
      exclusive_scan(InputIterator first, InputIterator last,
                     OutputIterator result);
  template<class ExecutionPolicy,
           class InputIterator, class OutputIterator>
    OutputIterator
      exclusive_scan(ExecutionPolicy &&exec,
                     InputIterator first, InputIterator last,
                     OutputIterator result);
  template<class InputIterator, class OutputIterator,
           class T>
    OutputIterator
      exclusive_scan(InputIterator first, InputIterator last,
                     OutputIterator result,
                     T init);
  template<class ExecutionPolicy,
           class InputIterator, class OutputIterator,
           class T>
    OutputIterator
      exclusive_scan(ExecutionPolicy &&exec,
                     InputIterator first, InputIterator last,
                     OutputIterator result,
                     T init);
  template<class InputIterator, class OutputIterator,
           class T, class BinaryOperation>
    OutputIterator
      exclusive_scan(InputIterator first, InputIterator last,
                     OutputIterator result,
                     T init, BinaryOperation binary_op);
  template<class ExecutionPolicy,
           class InputIterator, class OutputIterator,
           class T, class BinaryOperation>
    OutputIterator
      exclusive_scan(ExecutionPolicy &&exec,
                     InputIterator first, InputIterator last,
                     OutputIterator result,
                     T init, BinaryOperation binary_op);

  template<class InputIterator, class OutputIterator>
    OutputIterator
      inclusive_scan(InputIterator first, InputIterator last,
                     OutputIterator result);
  template<class ExecutionPolicy,
           class InputIterator, class OutputIterator>
    OutputIterator
      inclusive_scan(ExecutionPolicy &&exec,
                     InputIterator first, InputIterator last,
                     OutputIterator result);
  template<class InputIterator, class OutputIterator,
           class BinaryOperation>
    OutputIterator
      inclusive_scan(InputIterator first, InputIterator last,
                     OutputIterator result,
                     BinaryOperation binary_op);
  template<class ExecutionPolicy,
           class InputIterator, class OutputIterator,
           class BinaryOperation>
    OutputIterator
      inclusive_scan(ExecutionPolicy &&exec,
                     InputIterator first, InputIterator last,
                     OutputIterator result,
                     BinaryOperation binary_op);
  template<class InputIterator, class OutputIterator,
           class T, class BinaryOperation>
    OutputIterator
      inclusive_scan(InputIterator first, InputIterator last,
                     OutputIterator result,
                     T init, BinaryOperation binary_op);
  template<class ExecutionPolicy,
           class InputIterator, class OutputIterator,
           class T, class BinaryOperation>
    OutputIterator
      inclusive_scan(ExecutionPolicy &&exec,
                     InputIterator first, InputIterator last,
                     OutputIterator result,
                     T init, BinaryOperation binary_op);
}
```

### Reduce `[reduce]`

```
template<class InputIterator>
  typename iterator_traits<InputIterator>::value_type
    reduce(InputIterator first, InputIterator last);

template<class ExecutionPolicy,
         class InputIterator>
  typename iterator_traits<InputIterator>::value_type
    reduce(ExecutionPolicy &&exec,
           InputIterator first, InputIterator last);
```

1. *Effects:* The second algorithm's execution is parallelized as determined by `exec`.

2. *Returns:* The result of the sum of `T(0)` and the elements in the range `[first,last)`.

    The order of operands of the sum is unspecified.

3. *Requires:* Let `T` be the type of `InputIterator`'s value type, then `T(0)` shall be a valid expression. The `operator+` function associated with `T` shall have associativity and commutativity.

    `operator+` shall not invalidate iterators or subranges, nor modify elements in the range `[first,last)`.

4. *Complexity:* `O(last - first)`.

5. *Remarks:* The second signature shall not participate in overload resolution if

    `is_execution_policy<ExecutionPolicy>::value` is `false`.

```
template<class InputIterator, class T>
  T reduce(InputIterator first, InputIterator last, T init);

template<class ExecutionPolicy,
         class InputIterator, class T>
  T reduce(ExecutionPolicy &&exec,
           InputIterator first, InputIterator last, T init);

template<class InputIterator, class T, class BinaryOperation>
  T reduce(InputIterator first, InputIterator last, T init,
           BinaryOperation binary_op);

template<class ExecutionPolicy,
         class InputIterator, class T, class BinaryOperation>
  T reduce(ExecutionPolicy &&exec,
           InputIterator first, InputIterator last, T init,
           BinaryOperation binary_op);
```

1. *Effects:* The execution of the second and fourth algorithms is parallelized as determined by `exec`.

2. *Returns:* The result of the generalized sum of `init` and the elements in the range `[first,last)`.

    Sums of elements are evaluated with `operator+` or `binary_op`. The order of operands of the sum is unspecified.

3. *Requires:* The `operator+` function associated with `InputIterator`'s value type or `binary_op` shall have associativity and commutativity.

    Neither `operator+` nor `binary_op` shall invalidate iterators or subranges, nor modify elements in the range `[first,last)`.

4. *Complexity:* `O(last - first)`.

5. *Remarks:* The second and fourth signatures shall not participate in overload resolution if

    `is_execution_policy<ExecutionPolicy>::value` is `false`.

### Exclusive scan `[exclusive.scan]`

```
template<class InputIterator, class OutputIterator,
         class T>
  OutputIterator
    exclusive_scan(InputIterator first, InputIterator last,
                   OutputIterator result,
                   T init);

template<class ExecutionPolicy,
         class InputIterator, class OutputIterator,
         class T>
  OutputIterator
    exclusive_scan(ExecutionPolicy &&exec,
                   InputIterator first, InputIterator last,
                   OutputIterator result,
                   T init);

template<class InputIterator, class OutputIterator,
         class T, class BinaryOperation>
  OutputIterator
    exclusive_scan(InputIterator first, InputIterator last,
                   OutputIterator result,
                   T init, BinaryOperation binary_op);

template<class ExecutionPolicy,
         class InputIterator, class OutputIterator,
         class T, class BinaryOperation>
  OutputIterator
    exclusive_scan(ExecutionPolicy &&exec,
                   InputIterator first, InputIterator last,
                   OutputIterator result,
                   T init, BinaryOperation binary_op);
```

1. *Effects:* For each iterator `i` in `[result,result + (last - first))`, produces a result such that upon
    completion of the algorithm, `*i` yields the generalized sum of `init` and the elements in the range
    `[first, first + (i - result))`.

    During execution of the algorithm, every evaluation of the above sum is either of the corresponding form

    `(init + A) + B)` or `A + B` or

    `binary_op(binary_op(init,A), B)` or `binary_op(A, B)`

    where there exists some iterator `j` in `[first, last)` such that:

    1. `A` is the partial sum of elements in the range `[j, j + n)`.

    2. `B` is the partial sum of elements in the range `[j + n, j + m)`.

    3. `n` and `m` are positive integers and `j + m < last`.

    The execution of the second and fourth algorithms is parallelized as determined by `exec`.

2. *Returns:* The end of the resulting range beginning at `result`.

3. *Requires:* The `operator+` function associated with `InputIterator`'s value type or `binary_op` shall have associativity.

    Neither `operator+` nor `binary_op` shall invalidate iterators or subranges, nor modify elements in the ranges `[first,last)` or `[result,result + (last - first))`.

4. *Complexity:* `O(last - first)`.

5. *Remarks:* The second and fourth signatures shall not participate in overload resolution if

    `is_execution_policy<ExecutionPolicy>::value` is `false`.

6. *Notes:* The primary difference between `exclusive_scan` and `inclusive_scan` is that `exclusive_scan` excludes the `i`th input element from the `i`th sum.

### Inclusive scan `[inclusive.scan]`

```
template<class InputIterator, class OutputIterator>
  OutputIterator
    inclusive_scan(InputIterator first, InputIterator last,
                   OutputIterator result);

template<class ExecutionPolicy,
         class InputIterator, class OutputIterator>
  OutputIterator
    inclusive_scan(ExecutionPolicy &&exec,
                   InputIterator first, InputIterator last,
                   OutputIterator result);

template<class InputIterator, class OutputIterator,
         class BinaryOperation>
  OutputIterator
    inclusive_scan(InputIterator first, InputIterator last,
                   OutputIterator result,
                   BinaryOperation binary_op);

template<class ExecutionPolicy,
         class InputIterator, class OutputIterator,
         class BinaryOperation>
  OutputIterator
    inclusive_scan(ExecutionPolicy &&exec,
                   InputIterator first, InputIterator last,
                   OutputIterator result,
                   BinaryOperation binary_op);

template<class InputIterator, class OutputIterator,
         class T, class BinaryOperation>
  OutputIterator
    inclusive_scan(InputIterator first, InputIterator last,
                   OutputIterator result,
                   T init, BinaryOperation binary_op);

template<class ExecutionPolicy,
         class InputIterator, class OutputIterator,
         class T, class BinaryOperation>
  OutputIterator
    inclusive_scan(ExecutionPolicy &&exec,
                   InputIterator first, InputIterator last,
                   OutputIterator result,
                   T init, BinaryOperation binary_op);
```

1. *Effects:* For each iterator `i` in `[result,result + (last - first))`, produces a result such that upon
    completion of the algorithm, `*i` yields the generalized sum of `init` and the elements in the range
    `[first, first + (i - result)]`.

    During execution of the algorithm, every evaluation of the above sum is either of the corresponding form

    `(init + A) + B)` or `A + B` or

    `binary_op(binary_op(init,A), B)` or `binary_op(A, B)`

    where there exists some iterator `j` in `[first, last)` such that:

    1. `A` is the partial sum of elements in the range `[j, j + n)`.

    2. `B` is the partial sum of elements in the range `[j + n, j + m)`.

    3. `n` and `m` are positive integers and `j + m < last`.

    The execution of the second and fourth algorithms is parallelized as determined by `exec`.

2. *Returns:* The end of the resulting range beginning at `result`.

3. *Requires:* The `operator+` function associated with `InputIterator`'s value type or `binary_op` shall have associativity.

    Neither `operator+` nor `binary_op` shall invalidate iterators or subranges, nor modify elements in the ranges `[first,last)` or `[result,result + (last - first))`.

4. *Complexity:* `O(last - first)`.

5. *Remarks:* The second, fourth, and sixth signatures shall not participate in overload resolution if

    `is_execution_policy<ExecutionPolicy>::value` is `false`.

6. *Notes:* The primary difference between `exclusive_scan` and `inclusive_scan` is that `inclusive_scan` includes the `i`th input element in the `i`th sum.


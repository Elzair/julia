.. _man-noteworthy-differences:

*******************************************
Noteworthy Differences from other Languages
*******************************************

Noteworthy differences from MATLAB
----------------------------------

Although MATLAB users may find Julia's syntax familiar, Julia is not a MATLAB
clone. There are major syntactic and functional differences. The following are
some noteworthy differences that may trip up Julia users accustomed to MATLAB:

- Julia arrays are indexed with square brackets, ``A[i,j]``.
- Julia arrays are assigned by reference. After ``A=B``, changing elements of
  ``B`` will modify ``A`` as well.
- Julia values are passed and assigned by reference. If a function modifies an
  array, the changes will be visible in the caller.
- Julia does not automatically grow arrays in an assignment statement.
  Whereas in MATLAB ``a(4) = 3.2`` can create the array ``a = [0 0 0 3.2]``
  and ``a(5) = 7`` can grow it into ``a = [0 0 0 3.2 7]``, the corresponding
  Julia statement ``a[5] = 7`` throws an error if the length of ``a`` is less
  than 5 or if this statement is the first use of the identifier ``a``.
  Julia has :func:`push!` and :func:`append!`, which grow :obj:`Vector`\ s
  much more efficiently than MATLAB's ``a(end+1) = val``.
- The imaginary unit ``sqrt(-1)`` is represented in Julia as :obj:`im`, not
  ``i`` or ``j`` as in MATLAB.
- In Julia, literal numbers without a decimal point (such as ``42``) create
  integers instead of floating point numbers. Arbitrarily large integer
  literals are supported. As a result, some operations such as ``2^-1`` will
  throw a domain error as the result is not an integer (see
  :ref:`the FAQ entry on domain errors <man-domain-error>` for details).
- In Julia, multiple values are returned and assigned as tuples, e.g.
  ``(a, b) = (1, 2)`` or ``a, b = 1, 2``. MATLAB's ``nargout``, which is
  often used in MATLAB to do optional work based on the number of returned
  values, does not exist in Julia. Instead, users can use optional and keyword
  arguments to achieve similar capabilities.
- Julia has true one-dimensional arrays. Column vectors are of size ``N``, not
  ``Nx1``. For example, :func:`rand(N) <rand>` makes a 1-dimensional array.
- In Julia v0.3, concatenating scalars and arrays with the syntax ``[x,y,z]``
  concatenates in the first dimension ("vertically"). For concatenation in the
  second dimension ("horizontally"), use spaces as in ``[x y z]``. To
  construct block matrices (concatenating in the first two dimensions),
  the syntax ``[a b; c d]`` is used to avoid confusion. In Julia v0.4, the
  concatenation syntax ``[x, [y, z]]`` is deprecated in favor of ``[x; [y, z]``.
- In Julia, ``a:b`` and ``a:b:c`` construct :obj:`Range` objects. To construct
  a full vector like in MATLAB, use :func:`linspace`, or use brackets like in
  ``[a:b]``.
- Julia functions return values using the ``return`` keyword instead of by
  listing names of variables to return in the function definition (see
  :ref:`man-return-keyword` for details).
- A Julia script may contain any number of functions, and all definitions will
  be externally visible when the file is loaded. Function definitions can be
  loaded from files outside the current working directory.
- In Julia, reductions such as :func:`sum`, :func:`prod`, and :func:`max` are
  performed over every element of an array when called with a single argument,
  as in ``sum(A)``, even if ``A`` has more than one dimension.
- In Julia, functions such as :func:`sort` that operate column-wise by default
  (``sort(A)`` is equivalent to ``sort(A,1)``) do not have special behavior for
  ``1xN`` arrays; the argument is returned unmodified since it still performs
  ``sort(A,1)``. To sort a ``1xN`` matrix like a vector, use ``sort(A,2)``.
- In Julia, if ``A`` is a 2-dimensional array, ``fft(A)`` computes a 2D FFT. In
  particular, it is not equivalent to ``fft(A,1)``, which computes a 1D FFT
  acting column-wise.
- In Julia, parentheses must be used to call a function with zero arguments,
  like in :func:`tic` and :func:`toc`.
- Julia discourages the used of semicolons to end statements. The results of
  statements are not automatically printed (except at the interactive prompt),
  and lines of code do not need to end with semicolons. :func:`println` or
  :func:`@printf` can be used to print specific output.
- In Julia, if ``A`` and ``B`` are arrays, logical comparison operations like
  ``A == B`` do not return an array of booleans. Instead, use ``A .== B``, and
  similarly for the other boolean operators like :obj:`<`, :obj:`>` and
  :obj:`!=`.
- In Julia, the operators :obj:`&`, :obj:`|`, and :obj:`$` perform the bitwise
  operations equivalent to ``and``, ``or``, and ``xor`` respectively in MATLAB,
  and have precedence similar to Python's bitwise operators (unlike C). They
  can operate on scalars or element-wise across arrays and can be used to
  combine logical arrays, but note the difference in order of operations:
  parentheses may be required (e.g., to select elements of ``A`` equal to 1 or
  2 use ``(A .== 1) | (A .== 2)``).
- In Julia, the elements of a collection can be passed as arguments to a
  function using the splat operator ``...``, as in ``xs=[1,2]; f(xs...)``.
- Julia's :func:`svd` returns singular values as a vector instead of as a dense
  diagonal matrix.
- In Julia, ``...`` is not used to continue lines of code. Instead, incomplete
  expressions automatically continue onto the next line.
- In both Julia and MATLAB, the variable ``ans`` is set to the value of the
  last expression issued in an interactive session. In Julia, unlike MATLAB,
  ``ans`` is not set when Julia code is run in non-interactive mode.
- Julia's ``type``\ s do not support dynamically adding fields at runtime,
  unlike MATLAB's ``class``\ es. Instead, use a :obj`Dict`.

Noteworthy differences from R
-----------------------------

One of Julia's goals is to provide an effective language for data analysis
and statistical programming. For users coming to Julia from R, these are some
noteworthy differences:

- Julia's single quotes enclose characters, not strings.
- Julia can create substrings by indexing into :obj:`String`\ s.  In R, strings
  must be converted into character vectors before creating substrings.
- In Julia, like Python but unlike R, strings can be created with triple quotes
  ``""" ... """``. This syntax is convenient for constructing strings that
  contain line breaks.
- In Julia, varargs are specified using the splat operator ``...``, which
  always follows the name of a specific variable, unlike R, for which ``...``
  can occur in isolation.
- In Julia, modulus, is :obj:`%`, not ``%%``.
- In Julia, not all data structures support logical indexing. Furthermore,
  logical indexing in Julia is supported only with vectors of length equal to
  the object being indexed. For example:
  - In R, ``c(1, 2, 3, 4)[c(TRUE, FALSE)]`` produces ``c(1,3)``.
  - In R, ``c(1, 2, 3, 4)[c(TRUE, FALSE, TRUE, FALSE)]`` produces ``c(1,3)``.
  - In Julia, ``[1, 2, 3, 4][[true, false]]`` throws a :exc:`BoundsError`.
  - In Julia, ``[1, 2, 3, 4][[true, false, true, false]]`` produces ``[1, 3]``.
- Like many languages, Julia does not always allow operations on vectors of
  different lengths, unlike R where the vectors only need to share a common
  index range.  For example, ``c(1,2,3,4) + c(1,2)`` is valid R but the
  equivalent ``[1:4] + [1:2]`` will throw an error in Julia.
- Julia's :func:`apply` takes the function first, then its arguments, unlike
  ``lapply(<structure>, function, arg2, ...)`` in R.
- Julia uses ``end`` to denote the end of conditional blocks, like ``if``,
  loop blocks, like ``while``/``for``, and functions. In lieu of the one-line
  ``if ( cond ) statement``, Julia allows statements of the form
  ``if cond; statement; end``, ``cond && statement`` and
  ``!cond || statement``. Assignment statements in the latter two syntaxes must
  be explicitly wrapped in parentheses, e.g. ``cond && (x = value)``.
- In Julia, ``<-``, ``<<-`` and ``->`` are not assignment operators.
- Julia's ``->`` creates an anonymous function, like Python.
- Julia constructs vectors using brackets. Julia's ``[1, 2, 3]`` is the
  equivalent of R's ``c(1, 2, 3)``.
- Julia's :obj:`*` operator can perform matrix multiplication, unlike in R.
  If ``A`` and ``B`` are matrices, then ``A * B`` denotes a matrix
  multiplication in Julia, equivalent to R's ``A %*% B``. In R, this same
  notation would perform an element-wise (Hadamard) product. To get the
  element-wise multiplication operation, you need to write ``A .* B`` in Julia.
- Julia performs matrix transposition using the :obj:`.'` operator and conjugated
  transposition using the :obj:`'` operator. Julia's ``A.'`` is therefore
  equivalent to R's ``t(A)``.
- Julia does not require parentheses when writing ``if`` statements or
  ``for``/``while`` loops: use ``for i in [1, 2, 3]`` instead of
  ``for (i in c(1, 2, 3))`` and ``if i == 1`` instead of ``if (i == 1)``.
- Julia does not treat the numbers ``0`` and ``1`` as Booleans.
  You cannot write ``if (1)`` in Julia, because ``if`` statements accept only
  booleans. Instead, you can write ``if true``, ``if Bool(1)``, or ``if 1==1``.
- Julia does not provide ``nrow`` and ``ncol``. Instead, use ``size(M, 1)``
  for ``nrow(M)`` and ``size(M, 2)`` for ``ncol(M)``.
- Julia is careful to distinguish scalars, vectors and matrices.  In R,
  ``1`` and ``c(1)`` are the same. In Julia, they can not be used
  interchangeably. One potentially confusing result of this is that
  ``x' * y`` for vectors ``x`` and ``y`` is a 1-element vector, not a scalar.
  To get a scalar, use :func:`dot(x, y) <dot>`.
- Julia's :func:`diag` and :func:`diagm` are not like R's.
- Julia cannot assign to the results of function calls on the left hand side of
  an assignment operation: you cannot write ``diag(M) = ones(n)``.
- Julia discourages populating the main namespace with functions. Most
  statistical functionality for Julia is found in
  `packages <http://docs.julialang.org/en/latest/packages/packagelist/>`_
  under the `JuliaStats organization <https://github.com/JuliaStats>`_. For
  example:

  - Functions pertaining to probability distributions are provided by the
    `Distributions package <https://github.com/JuliaStats/Distributions.jl>`_.
  - The `DataFrames package <https://github.com/JuliaStats/DataFrames.jl>`_
    provides data frames.
  - Generalized linear models are provided by the `GLM package
    <https://github.com/JuliaStats/GLM.jl>`_.

- Julia provides tuples and real hash tables, but not R-style lists. When
  returning multiple items, you should typically use a tuple: instead of
  ``list(a = 1, b = 2)``, use ``(1, 2)``.
- Julia encourages users to write their own types, which are easier to use than
  S3 or S4 objects in R. Julia's multiple dispatch system means that
  ``table(x::TypeA)`` and ``table(x::TypeB)`` act like R's ``table.TypeA(x)``
  and ``table.TypeB(x)``.
- In Julia, values are passed and assigned by reference. If a function modifies
  an array, the changes will be visible in the caller. This is very different
  from R and allows new functions to operate on large data structures much more
  efficiently.
- In Julia, vectors and matrices are concatenated using :func:`hcat`,
  :func:`vcat` and :func:`hvcat`, not ``c``, ``rbind`` and ``cbind`` like in R.
- In Julia, a range like ``a:b`` is not shorthand for a vector like in R,
  but is a specialized :obj:`Range` that is used for iteration without high
  memory overhead. To convert a range into a vector, you need to wrap the range
  with brackets ``[a:b]``.
- Julia's :func:`max`` and :func:`min` are the equivalent of ``pmax`` and
  ``pmin`` respectively in R, but both arguments need to have the same
  dimensions.  While :func:`maximum` and :func:`minimum` replace ``max`` and
  ``min`` in R, there are important differences.
- Julia's :func:`sum`, :func:`prod`, :func:`maximum`, and :func:`minimum` are
  different from their counterparts in R. They all accept one or two arguments.
  The first argument is an iterable collection such as an array.  If there is a
  second argument, then this argument indicates the dimensions, over which the
  operation is carried out.  For instance, let ``A=[[1 2],[3 4]]`` in Julia and
  ``B=rbind(c(1,2),c(3,4))`` be the same matrix in R.  Then ``sum(A)`` gives
  the same result as ``sum(B)``, but ``sum(A, 1)`` is a row vector containing
  the sum over each column and ``sum(A, 2)`` is a column vector containing the
  sum over each row.  This contrasts to the behavior of R, where
  ``sum(B,1)=11`` and ``sum(B,2)=12``.  If the second argument is a vector,
  then it specifies all the dimensions over which the sum is performed, e.g.,
  ``sum(A,[1,2])=10``.  It should be noted that there is no error checking
  regarding the second argument.
- Julia has several functions that can mutate their arguments. For example,
  it has both :func:`sort` and :func:`sort!`.
- In R, performance requires vectorization. In Julia, almost the opposite is
  true: the best performing code is often achieved by using devectorized loops.
- Julia is eagerly evaluated and does not support R-style lazy evaluation. For
  most users, this means that there are very few unquoted expressions or column
  names.
- Julia does not support the ``NULL`` type.
- Julia lacks the equivalent of R's ``assign`` or ``get``.
- In Julia, ``return`` does not require parentheses.


Noteworthy differences from Python
----------------------------------

- In Julia, a vector of vectors can automatically concatenate into a
  one-dimensional vector *if* no explicit element type is specified. For example:

  - In Julia, ``[1, [2, 3]]`` concatenates into ``[1, 2, 3]``, like in R.
  - In Julia, ``Int[1, Int[2, 3]]`` will *not* concatenate, but instead throw an error.
  - In Julia, ``Any[1, [2,3]]`` will *not* concatenate.
  - In Julia, ``Vector{Int}[[1, 2], [3, 4]]`` will *not* concatenate, but
    produces an object similar to Python's list of lists. This object is
    *different* from a two-dimensional :obj:`Array` of :obj:`Int`\ s.

- Julia requires ``end`` to end a block. Unlike Python, Julia has no ``pass``
  keyword.
- In Julia, indexing of arrays, strings, etc. is 1-based not 0-based.
- Julia's slice indexing includes the last element, unlike in Python.
  ``a[2:3]`` in Julia is ``a[1:3]`` in Python.
- Julia does not support negative indexes. In particular, the last element of a
  list or array is indexed with :obj:`end` in Julia, not ``-1`` as in Python.
- Julia's list comprehensions do not support the optional ``if`` clause that
  Python has.
- Julia's ``for``, ``if``, ``while``, etc. blocks are terminated by the
  ``end`` keyword. Indentation level is not significant as it is in Python.
- Julia has no line continuation syntax: if, at the end of a line, the input so
  far is a complete expression, it is considered done; otherwise the input
  continues. One way to force an expression to continue is to wrap it in
  parentheses.
- Julia arrays are column major (Fortran ordered) whereas NumPy arrays are row
  major (C-ordered) by default. To get optimal performance when looping over
  arrays, the order of the loops should be reversed in Julia relative to NumPy
  (see relevant section of :ref:`man-performance-tips`).
- Julia evaluates default values of function arguments every time the method is
  invoked, unlike in Python where the default values are evaluated only once
  when the function is defined. For example, the function ``f(x=rand()) = x``
  returns a new random number every time it is invoked without argument. On the
  other hand, the function ``g(x=[1,2]) = push!(x,3)`` returns ``[1,2,3]`` every
  time it is called as ``g()``.

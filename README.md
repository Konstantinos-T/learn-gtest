# learn-gtest


## Basic Concepts

When using GoogleTest, you start by writing assertions, which are statements that check whether a condition is true. An assertion’s result can be success, nonfatal failure, or fatal failure. If a fatal failure occurs, it aborts the current function; otherwise the program continues normally.

Tests use assertions to verify the tested code’s behavior. If a test crashes or has a failed assertion, then it fails; otherwise it succeeds.

A test suite contains one or many tests. You should group your tests into test suites that reflect the structure of the tested code. When multiple tests in a test suite need to share common objects and subroutines, you can put them into a test fixture class.

A test program can contain multiple test suites.

We’ll now explain how to write a test program, starting at the individual assertion level and building up to tests and test suites.


## Assertions


GoogleTest assertions are macros that resemble function calls. You test a class or function by making assertions about its behavior. When an assertion fails, GoogleTest prints the assertion’s source file and line number location, along with a failure message. You may also supply a custom failure message which will be appended to GoogleTest’s message.

The assertions come in pairs that test the same thing but have different effects on the current function. ASSERT_* versions generate fatal failures when they fail, and abort the current function. EXPECT_* versions generate nonfatal failures, which don’t abort the current function. Usually EXPECT_* are preferred, as they allow more than one failure to be reported in a test. However, you should use ASSERT_* if it doesn’t make sense to continue when the assertion in question fails.

Since a failed ASSERT_* returns from the current function immediately, possibly skipping clean-up code that comes after it, it may cause a space leak. Depending on the nature of the leak, it may or may not be worth fixing - so keep this in mind if you get a heap checker error in addition to assertion errors.

To provide a custom failure message, simply stream it into the macro using the << operator or a sequence of such operators. See the following example, using the ASSERT_EQ and EXPECT_EQ macros to verify value equality:

```
ASSERT_EQ(x.size(), y.size()) << "Vectors x and y are of unequal length";

for (int i = 0; i < x.size(); ++i) {
  EXPECT_EQ(x[i], y[i]) << "Vectors x and y differ at index " << i;
}
```

Anything that can be streamed to an ostream can be streamed to an assertion macro–in particular, C strings and string objects. If a wide string (wchar_t*, TCHAR* in UNICODE mode on Windows, or std::wstring) is streamed to an assertion, it will be translated to UTF-8 when printed.

GoogleTest provides a collection of assertions for verifying the behavior of your code in various ways. You can check Boolean conditions, compare values based on relational operators, verify string values, floating-point values, and much more. There are even assertions that enable you to verify more complex states by providing custom predicates. For the complete list of assertions provided by GoogleTest, see the Assertions Reference.

## Simple Tests

To create a test:

Use the TEST() macro to define and name a test function. These are ordinary C++ functions that don’t return a value.
In this function, along with any valid C++ statements you want to include, use the various GoogleTest assertions to check values.
The test’s result is determined by the assertions; if any assertion in the test fails (either fatally or non-fatally), or if the test crashes, the entire test fails. Otherwise, it succeeds.
```
TEST(TestSuiteName, TestName) {
  ... test body ...
}

```
TEST() arguments go from general to specific. The first argument is the name of the test suite, and the second argument is the test’s name within the test suite. Both names must be valid C++ identifiers, and they should not contain any underscores (_). A test’s full name consists of its containing test suite and its individual name. Tests from different test suites can have the same individual name.

For example, let’s take a simple integer function:

int Factorial(int n);  // Returns the factorial of n
A test suite for this function might look like:


```
// Tests factorial of 0.
TEST(FactorialTest, HandlesZeroInput) {
  EXPECT_EQ(Factorial(0), 1);
}

// Tests factorial of positive numbers.
TEST(FactorialTest, HandlesPositiveInput) {
  EXPECT_EQ(Factorial(1), 1);
  EXPECT_EQ(Factorial(2), 2);
  EXPECT_EQ(Factorial(3), 6);
  EXPECT_EQ(Factorial(8), 40320);
}

```

GoogleTest groups the test results by test suites, so logically related tests should be in the same test suite; in other words, the first argument to their TEST() should be the same. In the above example, we have two tests, HandlesZeroInput and HandlesPositiveInput, that belong to the same test suite FactorialTest.

When naming your test suites and tests, you should follow the same convention as for naming functions and classes.


## Simple Tests

To create a test:

Use the TEST() macro to define and name a test function. These are ordinary C++ functions that don’t return a value. In this function, along with any valid C++ statements you want to include, use the various GoogleTest assertions to check values. The test’s result is determined by the assertions; if any assertion in the test fails (either fatally or non-fatally), or if the test crashes, the entire test fails. Otherwise, it succeeds.
```
TEST(TestSuiteName, TestName) {
  ... test body ...
}

```

TEST() arguments go from general to specific. The first argument is the name of the test suite, and the second argument is the test’s name within the test suite. Both names must be valid C++ identifiers, and they should not contain any underscores (_). A test’s full name consists of its containing test suite and its individual name. Tests from different test suites can have the same individual name.

For example, let’s take a simple integer function:

```
int Factorial(int n); // Returns the factorial of n A test suite for this function might look like:

// Tests factorial of 0.
TEST(FactorialTest, HandlesZeroInput) {
  EXPECT_EQ(Factorial(0), 1);
}

// Tests factorial of positive numbers.
TEST(FactorialTest, HandlesPositiveInput) {
  EXPECT_EQ(Factorial(1), 1);
  EXPECT_EQ(Factorial(2), 2);
  EXPECT_EQ(Factorial(3), 6);
  EXPECT_EQ(Factorial(8), 40320);
}

```

GoogleTest groups the test results by test suites, so logically related tests should be in the same test suite; in other words, the first argument to their TEST() should be the same. In the above example, we have two tests, HandlesZeroInput and HandlesPositiveInput, that belong to the same test suite FactorialTest.

When naming your test suites and tests, you should follow the same convention as for naming functions and classes.
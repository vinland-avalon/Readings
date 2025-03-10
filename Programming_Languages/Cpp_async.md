# [Futures and Promises](https://github.com/peter-can-write/cpp-notes/blob/master/future-and-promise.md)

`std::future` and `std::promise` were introduced in C++11's concurrency API as
the two ends of a read-write channel. `std::future` represents the
consumer/read-end and `std::promise` the producer/write-end.

A `std::future` object may be created either by a call to `std::async` or
through a `std::packaged_task` or a `std::promise`. The latter two both provide
a `get_future()` method for this purpose. In general, you use a `std::future`
object to hold the return value of a (possibly) asynchronously run function
call.

## `std::future` and `std::promise`

On the lowest level, a `std::future` is always associated with a
`std::promise`. A future is used to wait for some value, while the promise is
used to supply that precise value. Alongside `std::thread`, these two primitives
could thus be used like so:

```C++
int f(int x) {
	return x + 1;
}

// This will be the write-end
std::promise<int> promise;

// This will be the read-end
auto future = promise.get_future();

// Launch f asynchronously
std::thread thread([&promise] (int x) {
    promise.set_value(f(x));
  },
  5
);

// Get the value
std::cout << future.get() << std::endl;

// Make sure the main thread doesn't die
// before the children threads (or detach
// the child thread)
thread.join();
```

As you can see, you use `std::promise::set_value` from the callee-thread to set
the value to be communicated. `std::future` objects then have a method `get()`
which returns this value. Actually, `get()` calls `std::future::wait()` until
the return value is available, so you can also use `wait()` if you don't want
to explicitly retrieve the value as soon as it is ready.

Note that next to giving a promise a value, you can also give it an
exception. For this, `std::promise` has a method `set_exception` which takes a
`std::exception_ptr` object. There is no conversion from a `std::runtime_error`
object to a `std::exception_ptr` or anything, so you cannot really just "create"
an exception like that. You have to use `std::current_exception`, which holds a
pointer to the currently caught exception during exception handling (in a catch
block).

When you then call `get()` on the corresponding future, the exception set will
be rethrown (this is very useful when you don't have to do this manually, such
as with `std::async`). Here an example:

```C++
std::promise<int> promise;

// Get the future
auto future = promise.get_future();

std::thread thread([&promise] {
	try {
		throw std::runtime_error("Foobar!");
	} catch(...) {
		promise.set_exception(std::current_exception());
	}
});

// Will throw right here!
std::cout << future.get() << std::endl;
```

## `std::future` and `std::async`

Arguably the most common situation where you encounter `std::future`s is with
calls to `std::async`. `std::async` is an alternative and higher-level way of
executing a callable object in a separate thread, next to creating a
`std::thread` object. When you call `std::async`, it returns a
`std::future`. You could use it like so:

```C++
int f(int x) {
	return x + 1;
}

// Note that a peculiarity about std::async is
// that this call will not necessarily result in
// f being run concurrently; it is based on the
// scheduler's decision (to avoid oversubscription)
auto result = std::async(f, 5);

std::cout << result.get() << std::endl;
```

Clearly, this is a lot less work than manual promise/future
management. Exceptions are also handled.

For the purpose or our collective joy and happiness we could in fact implement a
simplified version of `std::async` ourselves:

```C++
int f(int x) {
	return x + 1;
}

template<typename Function, typename... Args>
auto
async(Function&& function, Args&&... args) {
	std::promise<std::result_of_t<Function(Args...)>> outer_promise;
	auto future = outer_promise.get_future();

	auto lambda = [promise = std::move(outer_promise), function]
								(Args&&... args)
								mutable {
		try {
			promise.set_value(function(args...));
		} catch (...) {
			promise.set_exception(std::current_exception());
		}
	};

	std::thread(
		std::move(lambda),
		std::forward<Args>(args)...
	).detach();

	return future;
}
```

Or with packaged tasks:

```C++
template<typename Function, typename... Args>
auto
async_packaged(Function&& function, Args&&... args) {
	std::packaged_task<std::remove_reference_t<Function>> outer_task(function);
	auto future = outer_task.get_future();
	auto lambda = [task = std::move(outer_task)] (Args&&... args) mutable {
		task(args...);
	};

	std::thread(
		std::move(lambda),
		std::forward<Args>(args)...
	).detach();

	return future;
}
```

## Waiting for Futures

One thing to mention here is that there exist two other ways of waiting for a
future other than the plain `wait()`. These two are:

1. `std::future::wait_for`, which waits for a given (`std::chrono`) duration.
2. `std::future::wait_until`, which waits until a given (`std::chrono`)
   time-point.

Both of these methods return one of three codes upon finishing:

1. `std::future_status::ready`: The future is ready.
2. `std::future_status::timeout`: The future was not yet ready after waiting.
3. `std::future_status::deferred`: The future is actually deferred and was not
   yet started.

Note that for (1) `std::chrono` literals are especially nice
(`future.wait_for(100ns)`).

## SemiFuture
Key Difference: Executor Association

folly::SemiFuture:
- A SemiFuture represents a future result but does not have an associated executor. This means it doesn't inherently dictate where or how its continuations (callbacks) will be executed.
- It's essentially a "raw" future, a placeholder for a value that will be available later.
- The intent of SemiFuture, is that the user of the SemiFuture will decide what executor the work will be done on.
- Users can use ``.via()`` to convert SemiFuture to Future and chain continuations using methods like ``.thenValue()``.

### SemiFuture.defer
folly::makeSemiFuture().defer() handles the creation of the Promise and the setting of the value for you.
```C++
#include <folly/executors/ThreadPoolExecutor.h>
#include <folly/futures/SemiFuture.h>
#include <iostream>
#include <thread>
#include <chrono>

folly::SemiFuture<int> heavyComputation(int input) {
  return folly::makeSemiFuture().defer([input]() {
    std::cout << "Starting heavy computation in thread: "
              << std::this_thread::get_id() << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(2)); // Simulate work
    std::cout << "Heavy computation finished in thread: "
              << std::this_thread::get_id() << std::endl;
    return input * 2;
  });
}

int main() {
  // Create a thread pool with 4 threads
  auto executor = std::make_shared<folly::ThreadPoolExecutor>(4);

  // Start the heavy computation
  folly::SemiFuture<int> computationResult = heavyComputation(10);

  // Attach the executor using via()
  folly::Future<int> futureResult = computationResult.via(executor.get());

  // Attach a continuation to handle the result
  futureResult.thenValue([](int result) {
    std::cout << "Result received in thread: " << std::this_thread::get_id()
              << ", result: " << result << std::endl;
  });

  std::cout << "Main thread continues..." << std::endl;

  //Keep the program alive long enough for the thread pool to complete.
  std::this_thread::sleep_for(std::chrono::seconds(3));
  return 0;
}
```
is equal to
```C++
#include <folly/futures/Promise.h>
#include <folly/futures/SemiFuture.h>
#include <iostream>
#include <thread>
#include <chrono>

folly::SemiFuture<int> heavyComputationManual(int input) {
  folly::Promise<int> promise;
  folly::SemiFuture<int> semiFuture = promise.getSemiFuture();

  std::thread([promise = std::move(promise), input]() mutable {
    std::cout << "Starting heavy computation in thread: "
              << std::this_thread::get_id() << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(2));
    std::cout << "Heavy computation finished in thread: "
              << std::this_thread::get_id() << std::endl;
    promise.setValue(input * 2); // Set the value using the promise
  }).detach();

  return semiFuture;
}

int main() {
  folly::SemiFuture<int> result = heavyComputationManual(10);

  result.thenValue([](int value) {
    std::cout << "Result: " << value << std::endl;
  });

  std::this_thread::sleep_for(std::chrono::seconds(3));

  return 0;
}
```


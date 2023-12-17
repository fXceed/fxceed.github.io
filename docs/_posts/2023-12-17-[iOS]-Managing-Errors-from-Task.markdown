---
layout: post
title:  "[iOS] Managing Errors from Task"
date:   2023-12-17 14:37:30 -0500
categories: jekyll update development
---
## 1. Propagate the Error to the Caller

When an async function inside a `Task` throws an error, one method is to propagate the error to the caller. This requires the caller to be an asynchronous function.


```swift
func performTask() async throws -> SomeResultType {
    return try await Task {
        return try await someAsyncThrowingFunction()
    }.value
}

Task {
    do {
        let result = try await performTask()
        // Handle the result
    } catch {
        // Handle the error
        print("Error occurred: \(error)")
    }
}
```



## 2. Handling Errors Within the Task

Alternatively, handle the error within the `Task` but notify the external context, like using completion handlers or delegate patterns.


```swift
func performTask(completion: @escaping (Result<SomeResultType, Error>) -> Void) {
    Task {
        do {
            let result = try await someAsyncThrowingFunction()
            completion(.success(result))
        } catch {
            completion(.failure(error))
        }
    }
}

performTask { result in
    switch result {
        case .success(let data):
            // Handle success
        case .failure(let error):
            // Handle error
    }
}
```

The completion handler approach is a common pattern in Swift and Objective-C for asynchronous callbacks, and its adaptation to Swift's new concurrency model is a logical extension of existing practices. So this specific usage as a bridge within the new concurrency model is not formally documented, however, still an effective approach.



## 3. Using Shared State

For UI updates, shared state like a published property in a `@MainActor` class or an `actor` can be used, ensuring thread safety.


```swift
class ViewModel: ObservableObject {
    @Published var error: Error?

    func performTask() {
        Task {
            do {
                let result = try await someAsyncThrowingFunction()
                // Update UI with result
            } catch {
                await MainActor.run {
                    self.error = error
                }
            }
        }
    }
}
```


For more on `@MainActor` and thread safety, and more managing guidances, see the [Concurrency section](https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html) in Swift documentation.

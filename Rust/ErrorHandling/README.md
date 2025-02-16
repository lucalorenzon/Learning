# Error Handling in RUST

From `Rust Programming Language Book`:
>Rust groups errors into two major categories: recoverable and unrecoverable errors. For a recoverable error, such as a file not found error, we most likely just want to report the problem to the user and retry the operation. Unrecoverable errors are always symptoms of bugs, such as trying to access a location beyond the end of an array, and so we want to immediately stop the program.
... it has the type Result<T, E> for recoverable errors and the panic! macro that stops execution when the program encounters an unrecoverable error

## Unrecoverable error
There are two way this to happen:
- executing an operation that goes to **panic**
- explicity call `panic!`

### What's happen in rust when PANIC
1. It walks back up the stack
2. it cleans up the data from each function in the stack

Because this can be a lot of work to do, there is the option to avoid to add code for doing that and abort the execution leaving OS to do the dirty job:
```toml
[profile.release]
panic = 'abort'
```

### View BACKTRACE
There are several env variable that control how the BACKTRACE can be produced:
- RUST_BACKTRACE
  - `0`: Disabled, no BACKTRACE is produced.
  - `1`: Basic BACKTRACE is produced.
  - `full`: Complete BACKTRACE is produced.


## Recoverable error

In rust the Error is part of a Result type that is an `enum` like the following:
```rust
enum Result<T, E> {
    OK(T),
    Err(E)
}
```

So if an operation can fail, it should return a `Result` or `panic` if the fail cannot be controlled.
The Result object allows us to performe the following action:


## Purpose of errors handling

|   | **Internal (function calling another function)** | **At the Edge (an api request failed to fullfil)** |
|---|---|---|
| **Control Flow (determine what to do next)** | Types, methods, fields | Status codes |
| **Reporting (investigate, after the fact)** | Logs/traces | Response body |



## thiserror macro
It's a convenient derive macro for the standard library's std::error::Error trait. Reference: [thiserror crate](https://docs.rs/thiserror/2.0.11/thiserror/)
It can be added to the project using:
```shell
cargo add thiserror
```
Actually there are a lot of small implementation that has to be write to manage errors in rust:
- conversion from one type to others,
- implementation of Display trait
- identifying the underlying lower level error that caused your error.
- hide implementation details of an error representation behind an opaque error type, so that the representation is able to evolve without breaking the crate's public API.
All this stuff are covered by thiserror macro throught a **derive macro**

### How it work
You can create your type of error and annotate it with:
```rust
#[derive(Error, Debug)]
pub enum MyError {

}
```
To implement the Display trait you can add `#[error(...)]` annotation. Example:
```rust
use thiserror::Error;

#[derive(Error)]
pub enum DataStoreError {
    #[error("data store disconnected")]
    Disconnect(#[from] io::Error),
    #[error("the data for key `{0}` is not available")]
    Redaction(String),
    #[error("invalid header (expected {expected:?}, found {found:?})")]
    InvalidHeader {
        expected: String,
        found: String,
    },
    #[error("unknown data store error")]
    Unknown,
}
```

The messages support a shorthand for interpolating fields from the error.
```rust
#[error("{var}")] ⟶ write!("{}", self.var)
#[error("{0}")] ⟶ write!("{}", self.0)
#[error("{var:?}")] ⟶ write!("{:?}", self.var)
#[error("{0:?}")] ⟶ write!("{:?}", self.0)
```

To implement a `From` trait to convert a generic error Type to a specific one, you can use the macro `#[from]`:
```rust
#[derive(Error)]
enum MyGenericError {
    #[error("Specific msg related to a specific error")]
    MySpecificError(#[from] std::Error)
}
```
Become:
```rust
impl From<std::Error> for MySpecificError
```

To extract to source of an error, you can use the `#[source]` annotation.

Example:
```rust
#[derive(thiserror::Error)]
pub enum SubscribeError {
    #[error("{0}")]
    ValidationError(String),
    #[error("Failed to acquire a Postgres connection from the pool")]
    PoolError(#[source] sqlx::Error),
    #[error("Failed to insert new subscriber in the database.")]
    InsertSubscriberError(#[source] sqlx::Error),
    #[error("Failed to store the confirmation token for a new subscriber.")]
    StoreTokenError(#[from] StoreTokenError),
    #[error("Failed to commit SQL transaction to store a new subscriber.")]
    TransactionCommitError(#[source] sqlx::Error),
    #[error("Failed to send a confirmation email.")]
    SendEmailError(#[from] reqwest::Error),
}
```

## Debug with error chain
Ref: [Zero to Production - Luca Palmieri](https://www.lpalmieri.com/posts/error-handling-rust/)
In order to have a Debug trait for our error that can really help in troubleshooting we can define a useul generic function like the following:
```rust
fn error_chain_format(
    e: &impl std::error::Error,
    f: &mut std::fmt:Formatter<'_>, ) -> std::fmt::Result {

    writeln!(f, "{}\n", e)?;
    let mut current = e.source();
    whike let Some(cause) = current {
        writeln!(f, "Caused by:\n\t{}", cause)?;
        current = cause.source();
    }
    Ok(())
}
```
And we can use in Debug trait implementation:
```rust
impl std::fmt::Debug for MyError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        error_chain_format(self, f)
    }
}
```

## Anyhow
The library mainly defines:
- a generic wrapper Result<T> or Result<T, anyhow::Error> that simplified errors handling boilerplate.
- a Context trait like the following:
```rust
pub trait Context<T, E>: Sealed {
    // Required methods
    fn context<C>(self, context: C) -> Result<T, Error>
       where C: Display + Send + Sync + 'static;
    fn with_context<C, F>(self, f: F) -> Result<T, Error>
       where C: Display + Send + Sync + 'static,
             F: FnOnce() -> C;
}
```

  For example:
```rust
use anyhow::{Context, Result};
use std::fs;
use std::path::PathBuf;

pub struct ImportantThing {
    path: PathBuf,
}

impl ImportantThing {
    pub fn detach(&mut self) -> Result<()> {...}
}

pub fn do_it(mut it: ImportantThing) -> Result<Vec<u8>> {
    it.detach().context("Failed to detach the important thing")?;

    let path = &it.path;
    let content = fs::read(path)
        .with_context(|| format!("Failed to read instrs from {}", path.display()))?;

    Ok(content)
}
```
  It's worth to mention that:
  - `context` it's executed when it is called
  - `with_context` it's executed deferred to the moment that an error occured.

- three macro for common error handling stuff:
  - **anyhow!** => Construct an ad-hoc error from a string or existing non-anyhow error value. It return a `anyhow::Error`
  - **bail!** => Return early with an error. It's the equivalent of `return Err(anyhow!( ... ))`
  - **ensure!** => Retun early with an error if a condition is not satisfied. It's the qeuivalent of `if (cond) { return Err(anyhow!( ... ))}`




## References
- [Standard Backtrace library](https://doc.rust-lang.org/std/backtrace/index.html)
- [Zero to Production](https://www.zero2prod.com/index.html)

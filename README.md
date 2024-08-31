## Description

`cargo check` emits a good summary on error count, but with `--message-format json`,
the count number is increased by 1 because it seems to include the error summary 
itself as a new error.

```rust
$ cargo check
    Checking cargo-error-count v0.1.0 (/rust/tmp/os-checker/cargo-error-count)
error: unused variable: `a`
 --> src/main.rs:3:9
  |
3 |     let a = 1;
  |         ^ help: if this is intentional, prefix it with an underscore: `_a`
  |
note: the lint level is defined here
 --> src/main.rs:1:9
  |
1 | #![deny(warnings)]
  |         ^^^^^^^^
  = note: `#[deny(unused_variables)]` implied by `#[deny(warnings)]`

error: could not compile `cargo-error-count` (bin "cargo-error-count") due to 1 previous error
```

```rust
$ cargo check --message-format json
    Checking cargo-error-count v0.1.0 (/rust/tmp/os-checker/cargo-error-count)
{"reason":"compiler-message","package_id":"path+file:///rust/tmp/os-checker/cargo-error-count#0.1.0","manifest_path":"/rust/tmp/os-checker/cargo-error-count
/Cargo.toml","target":{"kind":["bin"],"crate_types":["bin"],"name":"cargo-error-count","src_path":"/rust/tmp/os-checker/cargo-error-count/src/main.rs","edit
ion":"2021","doc":true,"doctest":false,"test":true},"message":{"rendered":"error: unused variable: `a`\n --> src/main.rs:3:9\n  |\n3 |     let a = 1;\n  |  
       ^ help: if this is intentional, prefix it with an underscore: `_a`\n  |\nnote: the lint level is defined here\n --> src/main.rs:1:9\n  |\n1 | #![deny
(warnings)]\n  |         ^^^^^^^^\n  = note: `#[deny(unused_variables)]` implied by `#[deny(warnings)]`\n\n","$message_type":"diagnostic","children":[{"chil
dren":[],"code":null,"level":"note","message":"the lint level is defined here","rendered":null,"spans":[{"byte_end":16,"byte_start":8,"column_end":17,"colum
n_start":9,"expansion":null,"file_name":"src/main.rs","is_primary":true,"label":null,"line_end":1,"line_start":1,"suggested_replacement":null,"suggestion_ap
plicability":null,"text":[{"highlight_end":17,"highlight_start":9,"text":"#![deny(warnings)]"}]}]},{"children":[],"code":null,"level":"note","message":"`#[d
eny(unused_variables)]` implied by `#[deny(warnings)]`","rendered":null,"spans":[]},{"children":[],"code":null,"level":"help","message":"if this is intentio
nal, prefix it with an underscore","rendered":null,"spans":[{"byte_end":40,"byte_start":39,"column_end":10,"column_start":9,"expansion":null,"file_name":"sr
c/main.rs","is_primary":true,"label":null,"line_end":3,"line_start":3,"suggested_replacement":"_a","suggestion_applicability":"MaybeIncorrect","text":[{"hig
hlight_end":10,"highlight_start":9,"text":"    let a = 1;"}]}]}],"code":{"code":"unused_variables","explanation":null},"level":"error","message":"unused var
iable: `a`","spans":[{"byte_end":40,"byte_start":39,"column_end":10,"column_start":9,"expansion":null,"file_name":"src/main.rs","is_primary":true,"label":nu
ll,"line_end":3,"line_start":3,"suggested_replacement":null,"suggestion_applicability":null,"text":[{"highlight_end":10,"highlight_start":9,"text":"    let 
a = 1;"}]}]}}
{"reason":"compiler-message","package_id":"path+file:///rust/tmp/os-checker/cargo-error-count#0.1.0","manifest_path":"/rust/tmp/os-checker/cargo-error-count
/Cargo.toml","target":{"kind":["bin"],"crate_types":["bin"],"name":"cargo-error-count","src_path":"/rust/tmp/os-checker/cargo-error-count/src/main.rs","edit
ion":"2021","doc":true,"doctest":false,"test":true},"message":{"rendered":"error: aborting due to 1 previous error\n\n","$message_type":"diagnostic","childr
en":[],"code":null,"level":"error","message":"aborting due to 1 previous error","spans":[]}}
error: could not compile `cargo-error-count` (bin "cargo-error-count") due to 2 previous errors
{"reason":"build-finished","success":false}
```

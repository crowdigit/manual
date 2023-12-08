# Lisp Development Environment Manual

## Creating new project

[See link](https://lispcookbook.github.io/cl-cookbook/systems.html)

After setting up project, add symlink to linkfarm

```bash
cd ~/common-lisp
ln -s "${project_dir}"
```

## Setting up test

[Cookbook](https://lispcookbook.github.io/cl-cookbook/testing.html) seems outdated

### 1. Setup ASD

Define system (`.asd`)

```lisp
; main system
(defsystem "foo"
    ; ...
    in-order-to ((test-op (test-op :foo/tests))))

; test system
(defsystem "foo/tests"
    ; ...
    :depends-on ("foo"
                 "fiveam")
    :perform (test-op (op c)
                      (system-call :fiveam :run!
                                   (find-symbol* :foo-suite ; must match root test suite
                                                 :foo/tests))))
```

### 2. Add test runner script

```lisp
; /test.lisp
(require :sb-cover)

(declaim (optimize sb-cover:store-coverage-data))
; list packages to generate coverage data
(asdf:oos 'asdf:load-op :foo :force t)
(declaim (optimize (sb-cover:store-coverage-data 0)))

(asdf:test-system :foo)
(sb-cover:report "coverage/")
```


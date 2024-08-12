# Using a private golang module with pants build
This is to demonstrate using pants build with a golang application that refers to a private github repo containing the module.

# go get - Attempt to use module without setting GOPRIVATE
```
/simple-consumer/consumer$ go get github.com/henri14/simple-module/hello@v0.0.2
go: downloading github.com/henri14/simple-module/hello v0.0.2
go: github.com/henri14/simple-module/hello@v0.0.2: verifying module: github.com/henri14/simple-module/hello@v0.0.2: reading https://sum.golang.org/lookup/github.com/henri14/simple-module/hello@v0.0.2: 404 Not Found
        server response:
        not found: github.com/henri14/simple-module/hello@v0.0.2: invalid version: git ls-remote -q origin in /tmp/gopath/pkg/mod/cache/vcs/3650a5475de977aabb40e7fc1653b859d9ac6936337ccb79dac03b750edcc192: exit status 128:
                fatal: could not read Username for 'https://github.com': terminal prompts disabled
        Confirm the import path was entered correctly.
        If this is a private repository, see https://golang.org/doc/faq#git_https for additional information.
```

# go get - with GOPRIVATE set

```
export GOPRIVATE=github.com/henri14/simple-module/*
/simple-consumer/consumer$ go get github.com/henri14/simple-module/hello@v0.0.2
go: downloading github.com/henri14/simple-module/hello v0.0.2
go: added github.com/henri14/simple-module/hello v0.0.2
```

# Run pants package with private module
```
/simple-consumer$ pants package consumer::
21:52:19.57 [ERROR] 1 Exception encountered:

Engine traceback:
  in `package` goal

ProcessExecutionFailure: Process 'Download Go module github.com/henri14/simple-module/hello@v0.0.2.' failed with exit code 1.
stdout:
{
        "Path": "github.com/henri14/simple-module/hello",
        "Version": "v0.0.2",
        "Error": "github.com/henri14/simple-module/hello@v0.0.2: git init --bare in /tmp/pants-sandbox-Y4qFwW/gopath/pkg/mod/cache/vcs/3650a5475de977aabb40e7fc1653b859d9ac6936337ccb79dac03b750edcc192: exec: \"git\": executable file not found in $PATH"
}

stderr:



Use `--keep-sandboxes=on_failure` to preserve the process chroot for inspection.
```
This folder demonstrates a bug where `#fileID` returns a full absolute path when using `vsoverlay`.

Any of below commands:

```
bazelisk run --xcode_version_config=//:xcode_12_5_1 //:cli
bazelisk run --xcode_version_config=//:xcode_12_5_1 --features=swift.vfsoverlay //:cli
bazelisk run --xcode_version_config=//:xcode_13_2_1 //:cli
```

Prints the expected file identifier:

```
cli/main.swift
```

But running:

```
bazelisk run --xcode_version_config=//:xcode_13_2_1 --features=swift.vfsoverlay //:cli
```

Prints the full absolute path of the file:

```
/private/var/tmp/_bazel_<username>/<sha>/execroot/__main__/main.swift
```

See:
* Root cause may be https://bugs.swift.org/browse/SR-15123
* A draft PR fixing the issue for rules_swift can be found in https://github.com/bazelbuild/rules_swift/pull/685

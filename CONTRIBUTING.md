# Contributing

Keep each reusable workflow aligned with a command that has already passed in a flagship repository. Do not add a release job, a secret, or a permission broader than `contents: read`.

`dependency-review.yml` runs only when the caller event is a pull request. It needs the GitHub dependency graph enabled on that repository. If the graph is off, leave the caller unconnected rather than weakening the permission set.

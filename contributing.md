# Contributing

Contributions are welcome!

## Scope of contributions

Deciding on whether or not your new feature may be a good fit here or over in `yt` is a bit nebulous!
The goal of `yt_experiments` is to provide a home for rapid prototyping of experimental `yt` features, accompanied by frequent
releases as often as needed. And eventually, the features released here may be moved over to `yt` (or their own separate packages).

## Developer notes

### Test suite

Tests are run with `pytest`, to install test dependencies:

```shell
pip install -e .[test,full]
```

To run tests:

``` shell
pytest -v .
```

### Type hints

There is a `mypy` test run through CI, but fully type hinting may not be necessary. If you hope
to eventually move your new feature over to `yt`, you'll want to fully type-hint your new code
eventually, but you can do so gradually or release here initially without type hints. We won't judge.
Remember the goal here is to quickly get experimental new features out to folks who want them quick!

### Releasing

Releases use a mostly automated pipeline of github actions triggered by pushing
a new version tag.

Assuming your local fork has a remote `upstream` pointing to this repo, first
make sure your local `main` branch matches `upstream`:

```shell
git checkout main
git fetch --all
git rebase upstream/main
```

Also make sure the version in `yt_experiments/_version.py` matches the upcoming
release. If not, stop here and create a PR to update the version to the upcoming
release.

Now create the new version tag:

```shell
git tag v2.1.3
```

And push it upstream

```shell
git push upstream v2.1.3
```

This will trigger the `build_and_publish.yaml` and `run_tests.yaml` actions. If
`build_and_publish.yaml` succeeds in building the sdist and wheels, then a new
github release draft will be created (but the release will not be pushed to pypi
yet!).

Next, go to the [release page](https://github.com/yt-project/yt_experiments/releases),
open up the draft release, edit the title and write up the release notes. When ready,
hit publish -- this will again trigger the `build_and_publish.yaml` action, but this
time it will push up to pypi on success.

Note that the pypi publication configuration is setup via a [Trusted Publisher](https://docs.pypi.org/trusted-publishers/)
under @chrishavlin's pypi account (chavlin).

# Test Cherry Pick 🍒

The repository for integration test of the [gh-cherry-pick](https://github.com/134130/gh-cherry-pick) and [action-cherry-pick](https://github.com/134130/action-cherry-pick).

## Structure

- `main` branch: The main branch of the repository.
- `release/1.*` branch: The release branch of the repository.
- `feature/*` branch: The feature branch of the repository.

### Pull Requests

- **[#1](https://github.com/134130/test-cherry-pick/pull/1)** : The PR always opened.
- **[#2](https://github.com/134130/test-cherry-pick/pull/2)** : The PR closed.
- **[#3](https://github.com/134130/test-cherry-pick/pull/3)** : The PR in draft mode.
- **[#4](https://github.com/134130/test-cherry-pick/pull/4)** : The PR merged with `Squash and merge` option.
- **[#5](https://github.com/134130/test-cherry-pick/pull/5)** : The PR merged with `Rebase and merge` option.
- **[#7](https://github.com/134130/test-cherry-pick/pull/7)** : The PR's merge commit will be conflict with the `release/1.0` branch.

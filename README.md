***DO NOT USE THIS ACTION***

[ruby/setup-ruby](https://github.com/ruby/setup-ruby) provides everything that this action has provided.

# setup-rbenv

Setup Ruby by [rbenv](https://github.com/rbenv/rbenv) on Ubuntu 18.04 / 16.04 virtual environments, not using tool-cache provided by GitHub to catch up the latest versions of Ruby.

# Usage

You need to initialize rbenv by dispatching `eval "$(rbenv init -)"` in each step before you run Ruby or use `shell: bash -l {0}` to run the commands.

```yaml
steps:
- uses: actions/checkout@master
- uses: masa-iwasaki/setup-rbenv@v1
- name: Install Ruby
  run: |
    eval "$(rbenv init -)"
    rbenv install -s `cat .ruby-version` # or specify the version you want

- name: Run bundler
  run: |
    eval "$(rbenv init -)"
    bundle install --path vendor/bundle
```

## Cache

By using [actions/cache](https://github.com/actions/cache), you can keep your rubies in your cache.

```yaml
steps:
- uses: actions/checkout@master
- uses: masa-iwasaki/setup-rbenv@v1

- name: Cache RBENV_ROOT
  uses: actions/cache@v1
  id: cache_rbenv
  with:
    path: /home/runner/.rbenv
    key: ${{ runner.os }}-rbenv-${{ hashFiles('**/.ruby-version') }}
    restore-keys: |
      {{ runner.os }}-rbenv-

- name: Install Ruby
  run: |
    eval "$(rbenv init -)"
    rbenv install -s `cat .ruby-version`
```

You don't need to check cache hit because `-s` option for `rbenv install` does the job. You need to be aware of [cache limits](https://github.com/actions/cache#cache-limits) if you want to keep multiple rubies.

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)

# Changelog

All notable changes to this collection will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

### Added

- Ansible Galaxy publish workflow, pre-commit hooks, Keep a Changelog history, and argument specs for the `records` role.

### Changed

- `galaxy.yml` version is the `0.0.0-semantic-version` placeholder substituted at publish time. A commit message containing `#major` or `#minor` bumps that component; otherwise the workflow increments the patch version.
- Minimum supported Ansible version is 2.15.0.
- Collection license is the repository `LICENSE` file (GPL-3.0).

## [1.0.0] and earlier

Releases prior to this file being introduced were tracked only via [GitHub tags](https://github.com/autodotes/zjleblanc.servicenow/tags) and commit history.

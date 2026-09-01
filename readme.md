# github-readme-stats

Self-hosted GitHub stats cards, serving the widgets on my profile README.

Runs on my own infrastructure at `readme-stats.m1k.cloud` instead of the public
instance, so the cards are rendered with my own GitHub token and are not subject
to shared rate limits.

## Deployment

Tagging `v*.*.*` builds and publishes `ghcr.io/ironashram/github-readme-stats`.
The container is deployed by the [commstack](https://github.com/ironashram/commstack)
Ansible playbook and expects a GitHub PAT in `PAT_1`.

## Credits

Original work by [Anurag Hazra](https://github.com/anuraghazra) and the
[github-readme-stats](https://github.com/anuraghazra/github-readme-stats)
contributors. Licensed under MIT - see [LICENSE](LICENSE).

Usage and customization options are documented in the upstream README.

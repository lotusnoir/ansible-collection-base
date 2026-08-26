# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/compare/1.0.0...1.1.0) - 2025-10-10

### Commits

- fix missing folder [`80a0c92`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/80a0c92efdd9ee98db340bf4b2e20d969122239f)
- add version on molecule play image to maintain support on old release [`04c22f7`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/04c22f74fb04b124ab58938c8071e58789d499ce)
- remove support for debian10 / ubuntu18 / redhat8 [`8652f79`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/8652f793143e94c96eb2a2d98337066f4bf1e82d)
- update molecule [`01a205b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/01a205b6b055c84a9d61bfdb43a4f421b8a7ca3c)
- add redhat 9 to default supported distrib [`0e70c02`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/0e70c0208faa2114da2c40cf874a9ce3afb7ecb2)
- add redhat 8 to default supported distrib [`c8c8a82`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/c8c8a822bf0338e83293d804a5c6eb91d19572cf)
- sort testing distrib to avoid random changes [`9c7e3f7`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/9c7e3f745047b393bce49fdee601504ae85be2cd)
- update pre-commit and lint fix [`18e0d68`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/18e0d688343b1ec957f125c5c19dfa901c5fc9f3)
- add variable for oner to support ubuntu18 [`8e4f647`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/8e4f647a879e77dccdc42ca213409dd68f22e242)
- remove support for rhel7 and docker dind on gitlab [`744dd0c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/744dd0c4dced62736a9bb4cce55d55ab0cda006b)
- remove lint from pipeline [`9fccd74`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/9fccd74a3278dfc16d242e26eaa6bb3d7816653f)
- remove pipelines tests, moved in precommits [`8542692`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/854269222cd491c87886668c85cc1e480c5cba08)
- add retries on packages install, to avoid error if temp pkg lock [`ecc2f0c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/ecc2f0cc8d2999b0629f63da953e819817ead41c)
- support debian12 and change pkg name into var [`172582c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/172582c5d37e064fe3e96fe046ff089de2ea1ba4)
- ensure apt packages absent when bootstrap install [`c1d1196`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/c1d119681106ca955dba7214e38b6a08f1447739)
- change facts var to be able to surcharge them on a pre_tasks [`09341d0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/09341d0f468a130b367498b86baeeac5d6ec8041)
- doc: update changelog [`a685f6d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/a685f6d9a37863065ff56a4beb4d71b74baf0342)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/compare/0.2.0...1.0.0) - 2025-10-10

### Commits

- update vars [`cf91615`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/cf916151df594f261454f2b6c592d0b948bf42c3)
- remove precommit gitlab [`ef9328d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/ef9328d1691cb75da57fa0df327078e423a5c604)
- remove local file as its downloaded [`7bf3cfa`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/7bf3cfa0d5fca31bd9e739ef47d37a9f757464b6)
- change import tasks to include in order to support facts on name [`dda643b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/dda643b1abe8d07fb078072c134510e5ba3de0f7)
- fix wrong mode for gpg key [`d2aa47f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/d2aa47f433eb4a68ebea766e83786262c214062a)
- doc: update changelog [`64e86cf`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/64e86cf405ca1a519241e8e2f01497ab393d8806)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/compare/0.1.0...0.2.0) - 2025-10-10

### Commits

- fix new distrib ora9 [`b268ecb`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/b268ecbcb95ae744a5df050bcd725ee611a5a527)
- add galaxy id [`4a98f49`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/4a98f49fb7e7317286f19815ea919ac8539ca500)
- doc: update changelog [`dbecc65`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/dbecc6575929c66a1deb2794c4ce263efce9157b)

## 0.1.0 - 2025-10-10

### Commits

- add code of conduc and small changes [`9269adc`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/9269adc7ffaa4ba3618e9e4d6decf98e6f7e4515)
- add precommit for lint [`23c402d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/23c402d98610d4c3d537ea5f7dab4e6ac3a78599)
- fix checks [`b4de9ce`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/b4de9cee40de17399ee22dfc0615936ed75b3b48)
- update default vars [`60056b5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/60056b5f4bb349dba926958fb6977f4908a6fe97)
- split distro for molecule tests on ci [`d81760a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/d81760a47296364343812af09b9d97a6b5a28103)
- remove verify [`54f12d3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/54f12d3e71c5ceabc3963742df10c8d32ab32e1c)
- lint project [`2dbea37`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/2dbea37f11fd6c56668394faaf0990bc8c707644)
- fix lint + update common files [`ae0def9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_salt_minion/commit/ae0def9954421e7a0e9861e8bd9dd7e905ecdcdc)

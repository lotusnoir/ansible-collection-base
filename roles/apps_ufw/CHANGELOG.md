# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/compare/2.0.0...2.1.0) - 2025-10-10

### Commits

- add support for ubuntu24 [`cd6fbbb`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/cd6fbbb9a95185c993006db8fec20f35fc938d37)
- add version on molecule play image to maintain support on old release [`a287d04`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/a287d044b5d2f3445f664cc0644050a87c4c4cbb)
- remove support for debian10 / ubuntu18 / redhat8 [`ea4d589`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/ea4d589be0b490a5ee7547c6e20a83ce4f91edb2)
- remove molecule test [`0b7543d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/0b7543d0df1717d6795c47ee14270355dcfd68a5)
- add restart on start command as no change on fore-reload [`fb6074b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/fb6074ba96091a93f6a5fb29274bab297c2a527e)
- revert starting task and add force-reload on redhat distrib [`12426ab`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/12426abb9ff05bee0af0b10595a99cc07a427ce0)
- change started task to ansible.builtin.service [`d400248`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/d400248f5655f3a8ae2285339646cc7ac7dc08bf)
- add redhat 9 to default supported distrib [`decacc3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/decacc3cfa8674b1ad2ab49346e077b12c454808)
- remove redhat molecule checks [`90d2aba`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/90d2abac6d555302171e40ef3e6722d1fb9b46e3)
- sort testing distrib to avoid random changes [`84e631f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/84e631f64ceaeb2c7659b2c7576fb6c9d6964924)
- update pre-commit and lint fix [`f413385`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/f4133852c9909413489b78a45843ad26b936383b)
- remove support for rhel7 and docker dind on gitlab [`a2efda6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/a2efda6abfc71e221e9987109cfd16e1b3841092)
- remove lint from pipeline [`b253778`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/b2537789e9ca5e4187cdd36390d3c130906973f9)
- remove pipelines tests, moved in precommits [`a95b496`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/a95b4967fececac0aa606b8aaf647d0fdba294af)
- add retries on packages install, to avoid error if temp pkg lock [`a6f72c5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/a6f72c539531706af18eb66437ec5f84da5e381d)
- change facts var to be able to surcharge them on a pre_tasks [`771307b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/771307b16b31e5c24aa105a1e4b35fdef54701e6)
- doc: update changelog [`a914c83`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/a914c837c9c2752341702833f1bfeccbd406bc77)

## [2.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/compare/1.1.0...2.0.0) - 2025-10-10

### Commits

- update vars [`a8db140`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/a8db14055f71fde10bd9c2a742d8f1d232b06514)
- update readme + precommit + include vars [`84a61b0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/84a61b077de32575f331f1c99862406443182ec2)
- add ansible comment on template files [`605220d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/605220d8b4e19a0df05438667076be70b9dba483)
- update molecule playbook order and main include vars [`bacf71a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/bacf71a85fea29800e5eb96f2800c6f46fdea0e1)
- add option to disable icmp pings check [`c83c626`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/c83c62692487a3e6b4292e65171c718b76afd0c4)
- doc: update changelog [`27637d6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/27637d6886d3120bc0a9e651c79c9775ebe19d87)

## [1.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/compare/1.0.0...1.1.0) - 2025-10-10

### Commits

- add debian12 support [`beb28db`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/beb28db2928d326d19df8815fc427bf59c128715)
- doc: update changelog [`c7265ee`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/c7265eef397383298d4717e6a34c927f77c3ee68)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/compare/0.3.0...1.0.0) - 2025-10-10

### Commits

- add code of conduc and small changes [`106526a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/106526a2bdbf1860b6d6b0d00c31afc18bbab918)
- add precommit for lint [`6b043fd`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/6b043fd783b32e7d857ecff78509ca662cf9061a)
- fix checks [`3089fb7`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/3089fb7c9db28589aedcaaabdc0a9f599159e059)
- add new molecule all scenario [`c665978`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/c665978f2af2509401ed082fbeda6f21235170a0)
- split distro for molecule tests on ci [`c2bc35e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/c2bc35e1089ba72f0596eda33f9fd43c5b647a4a)
- update checks and Readme [`b3f7605`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/b3f76050b0ca6db578905facba4cb053d4bb9187)
- add molecule fix for ora9 [`d28f6a5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/d28f6a573f3d1b788957d9ba6840e6da1001c6d6)
- add ora9 support [`ab73bd6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/ab73bd61930451d30401e11ba59c8ac6f33f918d)
- update install option [`c7c4fa9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/c7c4fa9b793a2d0046c8a63fa4ae27deba3f112a)
- add condition to check redhat epel release [`0cf33f2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/0cf33f21daf5a615cbc84e24a98bcec9d9bc8dac)
- fix checks [`c6e5cf4`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/c6e5cf40084ca065d271b3075f35ed1e7884f74e)
- fix test and pipeline [`32c7f68`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/32c7f6850a26dd579609cfec785bceb2c3139abf)
- change verify on molecule [`7d96cc7`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/7d96cc7cff46907a09cfd301c9294e19356a206d)
- fix: molecule image and optimize scenario [`331062e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/331062e8d06228bf941b86c12cd72c63892943c3)
- fix: repo to dowload package on rhel [`563c3e6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/563c3e68115bc209e967a0ce77e58b4ab13e4abe)
- doc: update changelog [`d2c5c80`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/d2c5c804753c4dd22309a2f3a60f1ccb2349ac33)

## [0.3.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/compare/0.2.0...0.3.0) - 2025-10-10

### Commits

- minor: add oracleLinux7 support [`22b2298`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/22b22987d88ca02feb1ee2270394524dba13d3a8)
- minor: add oracleLinux support + little fixes [`d351f47`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/d351f47a93c62f6689bb09ffe4b839d0e3232274)
- doc: update changelog [`c9c411b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/c9c411bb0542b72e2217610e9a9f146e8d1ebcef)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/compare/0.1.1...0.2.0) - 2025-10-10

### Commits

- fix: molecule tests on ubuntu 18&20 [`2aea29e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/2aea29ef009852b5cad2bf9e7cb1a82d64ab632f)
- fix: remove molecule centos8 [`9c93cd4`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/9c93cd426707082ce102aa3a0de9ce5ffe1c7014)
- fix: remove unsupported centos8 + minor fixes [`cbf8027`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/cbf802798e701043961b96ab38e231c2a843333d)
- doc: update changelog [`d473c17`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/d473c173f6f9300de5e2fc980a9ac5490faea5db)

## [0.1.1](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/compare/0.1.0...0.1.1) - 2025-10-10

### Commits

- fix: Changes on README + molecule container names [`2b4c583`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/2b4c5830c0a14f43b738c88ee4391b9ebc271da3)
- fix: add role name on molecule container names to avoid concurrent conflict on runners [`5e625b2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/5e625b2a9943b5d18de08ae62a73e433867d62b1)
- minor: add changelog + automatic files checks [`57d84c9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/57d84c9bed8ab96d678303b16fa2eeac585c439b)
- update after.rules [`75bee06`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/75bee06fc33b83ebb5561009e21e7306ffba3044)

## 0.1.0 - 2025-10-10

### Commits

- initial commit [`a067692`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_ufw/commit/a0676929a650dd37483293db6c96c54acfef294e)

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/3.0.0...3.1.0) - 2025-10-10

### Commits

- add support for ubuntu24 [`c52c087`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/c52c087955ce24c402aa4791b3510fd2ebb32afb)
- add ufw ack ignore file [`3f69b8b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/3f69b8be46ec5642cbba13b31b3662fba2158d70)
- add version on molecule play image to maintain support on old release [`6479b7a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/6479b7ab6253c86ed681e30aa6f7136e82813127)
- remove support for debian10 / ubuntu18 / redhat8 [`7ab48de`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/7ab48dee5ebfde9346c3cda738ffb43a20a294bc)
- update molecule [`565ba26`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/565ba260ff8c9a27ca3688cd38b5576012551756)
- add redhat 9 to default supported distrib [`28bd224`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/28bd224ade77039654b1e1f2fb6019c3daa35799)
- fix redhat8 repo [`4221aa1`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/4221aa124e969ca9da6d007fe91680fe20fedd71)
- remove redhat molecule checks [`96e7188`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/96e718843bf39e8a382beba4ddf2073a9af7d120)
- sort testing distrib to avoid random changes [`f3246cd`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/f3246cd9fe9470fccca8d830fd9ea9783b9a8e09)
- update pre-commit and lint fix [`53edf09`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/53edf0953b4ae4441ca3b608d27a39da5400028f)
- remove support for rhel7 and docker dind on gitlab [`56ee04d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/56ee04d3021a3b682b214197a2bc1d4f7dc033ec)
- remove unsupported rhel7 [`e991639`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/e99163968fd47779e80f5e9a51fa8b292d43e12c)
- add retries on packages install, to avoid error if temp pkg lock [`d8e832d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/d8e832de9026a3438c8edca1a4c4ac7a2e7632a4)
- change facts var to be able to surcharge them on a pre_tasks [`186a361`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/186a3617e36e728b78719462e6d91b8bc66324aa)
- remove default files config and make it variables [`3a67824`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/3a6782498d68d6b454e275e2b09e705dad6d80e2)
- doc: update changelog [`1c81d41`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/1c81d41b81a1f9a5ec56a13798cb4c6d1b3b89cf)

## [3.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/2.1.0...3.0.0) - 2025-10-10

### Commits

- update vars [`fa79382`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/fa793828b5564b007e9c740e5a143234994294a1)
- remove precommit gitlab [`16c8bbb`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/16c8bbb523d7a7296408cfb9eb8e13778d955156)
- update readme + precommit + include vars [`45c91b2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/45c91b227329a4ba90af4fd4bc8d98e19a4f47ca)
- add ansible comment on template files [`05d76b2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/05d76b2c28c40d18212ea332dad480259633dd4b)
- update molecule playbook order and main include vars [`7eac3dc`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/7eac3dc4e249b15f3e2bd9d10afa415ce7ca43a8)
- update forward options vars [`54b121e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/54b121e358dfe6a6f72c77a84a2a0430a6d33e25)

## [2.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/2.0.0...2.1.0) - 2025-10-10

### Commits

- doc: update changelog [`598baf0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/598baf0122f25cb7bae7d406f169730c016fd41d)

## [2.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/1.0.0...2.0.0) - 2025-10-10

### Commits

- add debian12 support [`561b516`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/561b516dcff1445baaf539578ebc37738cca73cd)
- remove vars files [`e823251`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/e823251c8ea2d1ab167121f3fbe2fe6f7afda0d4)
- change default perm on files [`41b9311`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/41b931187359cdac6cd47d5b78e4e2da6da773c1)
- review rsyslog.conf to default config [`55aa462`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/55aa462d3837126249db7b36e41a48936ccb09c3)
- doc: update changelog [`909d1a0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/909d1a0a89550fe44a808d84ec9f776b6ba4ab6d)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/0.4.0...1.0.0) - 2025-10-10

### Commits

- add code of conduc and small changes [`f3fe936`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/f3fe9362ba5eb74f0350d7a07fe4fb344dd54ca3)
- add precommit for lint [`7d034a3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/7d034a3a166f9b9c070924bf922c1529f646d28a)
- fix checks [`e9dbf65`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/e9dbf65652c4ec0103c91e5f48ba3e0cc7eecd77)
- add new molecule all scenario [`411be43`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/411be43e61cbb03b842c627f763c0f007effb045)
- split distro for molecule tests on ci [`a367ae1`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/a367ae13eb611c80868e31b9a08489265732aafa)
- change default rules [`47d869d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/47d869dd596cfd83058994324b15d87e276d72bb)
- add molecule fix for ora9 [`c9993a5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/c9993a500de5e9f6c64c5ae1f426d34e8d852106)
- add ora9 support [`f768530`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/f76853075cea012d29a72504fa2cb09af70e113b)
- fix checks [`b7f5a43`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/b7f5a4370e725c2630e863c801c2bf1a2c1af3f9)
- add traditionalfileformat optional [`9904bbc`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/9904bbc8f628194f6a4b0390876813ae819e8273)
- lint: fix trailing space [`8eb266d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/8eb266ddd1e70c850295a95cb03fd32a648a9c65)
- update and clean [`1e3d42f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/1e3d42f2789e2c86c2d525a7fcd20928d750d5e1)
- doc: update changelog [`bfba5f6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/bfba5f6b5aa79338029aa6ceaf9ebd5bf8a89f03)

## [0.4.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/0.3.0...0.4.0) - 2025-10-10

### Commits

- fix test and pipeline [`9f56965`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/9f56965da2fc26a4a778d8e08638ae0133b14048)
- change verify on molecule [`4576cd1`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/4576cd1f5740246614c5e32f3ee447e900b7fa89)
- fix: change dnf to yum for redhat install to support major 7 [`b6fd7ff`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/b6fd7ff79f07e13da40d74adc3134f361e1090e0)
- doc: update changelog [`75f9d76`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/75f9d767d4cd21e0347e3c062fcad5ad96f221c5)

## [0.3.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/0.2.0...0.3.0) - 2025-10-10

### Commits

- minor: add oracleLinux7 support [`ab33f58`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/ab33f580860fafa6ee8733362b8a6b26dab08b0f)
- doc: update changelog [`2225b43`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/2225b4394172a06ddfb17686efbfaae1fd392ad7)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/0.1.1...0.2.0) - 2025-10-10

### Commits

- minor: add oracleLinux support + little fixes [`68f82fa`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/68f82fa4f2b3b78ecf843078d687795a9d5dfa48)
- fix: add variables on default config [`67f2e4d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/67f2e4da926261a314219c21b000d99b2ad1dd7c)
- fix: changes yes to true vars and changes container names on molecule [`500e728`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/500e728177211c4d170db78bcffd5dc50dc39373)
- fix: remove unsupported centos8 [`72c9496`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/72c9496c9bbba91bd8f2b242f2238abf46471f96)
- lint: remove space [`abcb6f2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/abcb6f20a5fb161c26021163d3d2c35ac7bceb7a)
- doc: update changelog [`fc590ad`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/fc590ad79ceed4785cc874e03517834257525ce9)

## [0.1.1](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/compare/0.1.0...0.1.1) - 2025-10-10

### Commits

- fix: Changes on README + molecule container names [`de597b6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/de597b6e15005eda5523ddb9694bff685f7e7da1)
- fix: add role name on molecule container names to avoid concurrent conflict on runners [`7b48320`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/7b4832062802507492d030b17280f8d42c8b681b)
- minor: add changelog + automatic files checks [`cccd599`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/cccd5997c7df823f1b027112d808dcd65ae7ef6d)

## 0.1.0 - 2025-10-10

### Commits

- initial commit [`dae7c89`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog/commit/dae7c897f54146e1d022dc641bba73c130b8d1ed)

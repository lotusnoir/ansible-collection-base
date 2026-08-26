# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/2.0.0...2.1.0) - 2025-10-10

### Commits

- add support for ubuntu24 [`11d9f2a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/11d9f2a8aca33d9ab62409205db5ce7d20db274b)
- add version on molecule play image to maintain support on old release [`ee8ade9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/ee8ade950e6063781e0b04ce4d669dbd4b3d6b75)
- remove support for debian10 / ubuntu18 / redhat8 [`495b5de`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/495b5dea5dcccc8775edae7b716ae09920046b92)
- add redhat 9 to default supported distrib [`6850560`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/6850560e219fef50e90cd32137a4835182c47483)
- remove redhat molecule checks [`727a87a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/727a87a0f513c0dfb2f89722ec30409581adef6e)
- sort testing distrib to avoid random changes [`b1c567f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/b1c567f2222378b06f7e4c5b9b498502e1df078b)
- update pre-commit and lint fix [`e292974`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/e292974e370c05fc500a25ba8883c7b4bbc2a4ed)
- remove support for rhel7 and docker dind on gitlab [`afd6ad5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/afd6ad599e697b0c29e79fb522550d2c14750b65)
- remove lint from pipeline [`edb4ef2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/edb4ef2aa16776229d9414ea876865480fb8bc71)
- remove pipelines tests, moved in precommits [`5769655`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/5769655359393cd910836a947e93472d7bd2a95a)
- add retries on packages install, to avoid error if temp pkg lock [`20eb833`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/20eb83379de0f214d9a63501d3f4ccb681dd9d00)
- change facts var to be able to surcharge them on a pre_tasks [`9a987fd`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/9a987fd408399ad01f1aa6c8def29487f37133e6)
- reoder vars and add force condition [`914d767`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/914d76755f06c233d6743525743894c4a5e4889f)
- change name for the server related variables [`36ba914`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/36ba91421359ef925496cc50179976e3499f256e)
- doc: update changelog [`6459252`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/6459252a6af6cd6a8ca06cbf826aacaf2f6372fd)

## [2.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/1.1.0...2.0.0) - 2025-10-10

### Commits

- update vars [`d74a991`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/d74a9911ececc46d87d2c94b76886a56c9bcf1b3)
- update precommit [`461efb8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/461efb8903dc83289aecf93a1b2317c3970b5b5c)
- update readme + precommit + include vars [`2264792`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/2264792f1539958be94fa82fd8923f84c4c6bfac)
- doc: update changelog [`796716c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/796716c1b5db12dbcb8823b479c571782cc59809)

## [1.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/1.0.0...1.1.0) - 2025-10-10

### Commits

- add debian12 support [`9334fd2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/9334fd29d74a849e2ed383c055d34864e559d86e)
- doc: update changelog [`6eefd94`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/6eefd9407cbda99aaa0ff8b8fcd37f34eacb0f55)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/0.4.0...1.0.0) - 2025-10-10

### Commits

- add code of conduc and small changes [`5a21047`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/5a2104719feb9b31900a8f28b591b92cbee518f5)
- add precommit for lint [`13b6aff`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/13b6affc26684ac916752a1b242d3b3aa685c395)
- fix checks [`7213c64`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/7213c647c8b6c5303156d428316a8ada1065ab9e)
- add new molecule all scenario [`7dcbf0c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/7dcbf0c0846873faaf98e9fa3721dcd5428e90f8)
- split distro for molecule tests on ci [`5faba69`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/5faba697ebc584aec3003493a1e757ade4325162)
- fix molecule ora9 [`0b051b6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/0b051b6a9d244d07a4e203249f6de0a7d771b8b0)
- add ora9 support [`4f207a5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/4f207a57b43f1d09796abb39ea41d1c275b637a9)
- fix checks [`e086708`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/e086708109696e927268992b07948b33e45c3e98)
- fix clone pipeline [`3da8fe2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/3da8fe2f3c5f38ef71b85579ba9d44a9c5d9c9b9)
- fix: molecule image and optimize scenario [`0270737`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/0270737856aff19604f797b1b51b8d68cd821b54)
- doc: update changelog [`0a99489`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/0a9948916c153ccb3975c3b7c8f67cf16c941034)

## [0.4.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/0.3.0...0.4.0) - 2025-10-10

### Commits

- minor: add oracleLinux7 support [`7b57f05`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/7b57f0531caadaa9823ca55e8b3a7d3245abb998)
- doc: update changelog [`dc5c93e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/dc5c93ec99b83417efc0d9dccfd5dec009e976bc)

## [0.3.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/0.2.0...0.3.0) - 2025-10-10

### Commits

- minor: add oracleLinux support + little fixes [`d968769`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/d968769a3e68799006c79089546c1f4901c51963)
- doc: update changelog [`acb84b1`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/acb84b1f46cdde2d093464a5ff96f477910409a6)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/0.1.1...0.2.0) - 2025-10-10

### Commits

- lint: fix unanned task [`612c0de`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/612c0de52b505c18faa863ef4d1167c0917e00cb)
- fix: remove unsupported centos8 + minor fixes [`f40f6f6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/f40f6f6656bcec3dbe7e4a171ff7a8eab72f3b20)
- fix: wrong .deb name for debian11 packages [`1db85d4`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/1db85d42edda2b2cffdba41e0fa33c2f91541740)
- fix: update freeipa package version for debian11 [`dc4462e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/dc4462e0acead66a615ad8af79d8553bec42660f)
- lint: remove space [`eb0e3d1`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/eb0e3d10593696fcdabca373615b51787902589e)
- doc: update changelog [`0cfc188`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/0cfc188ddbbd551c41fc234e1c010ca1a6cdcb9d)

## [0.1.1](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/compare/0.1.0...0.1.1) - 2025-10-10

### Commits

- doc: update changelog [`38570c8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/38570c8205dad888a8847015b52bc6e178744677)
- fix: Changes on README + molecule container names [`72a36cf`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/72a36cf63a8e89b8ddc2f7aa2406a5464e9baf25)
- doc: update changelog [`c72c14b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/c72c14b42acf7e945763f4e9e965f237dd0a27fe)

## 0.1.0 - 2025-10-10

### Commits

- doc: update changelog [`d5b32b8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/d5b32b89cf17225ed008ebd21fe6b59f20bc196f)
- doc: update changelog [`d8e4c5f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/d8e4c5f03143e211aacf23a69d930dc7e66c6d0a)
- fix: add role name on molecule container names to avoid concurrent conflict on runners [`54c4b85`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/54c4b85e081b05381c3373a579b0650c00e31b5d)
- fix: can't run molecule test on pipleine [`186866a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/186866aea39ef4018f108478273837e0a7dd9e71)
- fix molecule error [`9f8a426`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_freeipa_client/commit/9f8a4266d11ea58fa98726cde871f7c448f17032)

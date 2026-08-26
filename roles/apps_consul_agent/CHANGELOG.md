# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/compare/3.1.0...3.2.0) - 2025-02-28

### Commits

- fix unavailable package on redhat [`2ca6572`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/2ca6572181ec86598c12ac1a97c39145e4fe0545)
- fix [`882b917`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/882b917b80b3fe51c768618a6040ce5f2be4f53d)
- add support for ubuntu24 [`a85f7d4`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/a85f7d44017bba89f67232090becf555dc03c165)
- add version on molecule play image to maintain support on old release [`9c352a8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/9c352a8a11438609574251132923a9cf62dda8c3)
- remove support for debian10 / ubuntu18 / redhat8 [`0aeee3d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/0aeee3d11a417ad9d0cda1ec2a449756645ddde0)
- update molecule [`8bca1f5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/8bca1f5b0cf9fb1a0865dd9739aaf045eb78ba61)
- add redhat 9 to default supported distrib [`56d85f3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/56d85f3a05fcdd6f49e1139436a801eaaaa4752a)
- fix requirement packages fore redhat [`6223d23`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/6223d232aee0a484965de23f53df7a1b41081987)
- remove redhat molecule checks [`604676d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/604676de209d2cb2e98acfb4b8d03aff2e741d91)
- sort testing distrib to avoid random changes [`d48bbb0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/d48bbb048f5923942d01c4a34962074ab5641932)
- doc: update changelog [`c7a3749`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/c7a374972aeef73fd92e34f63f1ed29b3d9dd992)

## [3.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/compare/3.0.0...3.1.0) - 2024-03-13

### Commits

- update pre-commit and lint fix [`1e9a12d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/1e9a12dc720b7b44b82319a55f6a49d8299523fa)
- condition on handler if no binary installed [`7a8fe34`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/7a8fe3427c3927df9be2dc7c40ad93e77c3c2faa)
- split dir create in order to create consul.d separatelly [`7e14796`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/7e14796dcc08f0839ed73e6ff5ad8ab85b3fb287)
- add variables to split install tasks [`1336956`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/13369563212286635d84b4f1c1920c3dce789079)
- update version and add skip install variable [`eb0bf9a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/eb0bf9ac18471781d4cd24159add53df9adb40a2)
- remove support for rhel7 and docker dind on gitlab [`8a2e5da`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/8a2e5dac94d6831d654e8878e704dbfe28ca604a)
- remove unsupported rhel7 [`8def026`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/8def0268eb3b466b135aa87427d47b0a29b0913f)
- add retries on packages install, to avoid error if temp pkg lock [`585510e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/585510eb1b4e16b769dc98c3966527c6e2bef396)
- change facts var to be able to surcharge them on a pre_tasks [`b4fd7d9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/b4fd7d9945244352b94525fd26adffdbd19e6245)

## [3.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/compare/2.0.0...3.0.0) - 2023-09-27

### Commits

- doc: update changelog [`281b585`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/281b585972797664c51a72e21d31037f2dcd7f43)

## [2.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/compare/1.1.0...2.0.0) - 2023-09-25

### Commits

- update vars [`75329d3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/75329d353695522d41d0342a5e98172cadfe18e5)
- fix missing vars for rhel8 [`2d58584`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/2d58584357cb7ca8b56b4b5f2a438809a093a979)
- change import tasks to include in order to support facts on name [`bb8f7f5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/bb8f7f54d760e8d6d4d1cb9bdccb4aabbc19e170)
- prevent install folder to have the right change [`58b102d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/58b102d2e6fedd77d11972311f6711e151502563)
- fix lint [`378e18a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/378e18ab6c3929ea512f7605510911758fc500cc)
- doc: update changelog [`9983071`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/99830716137bdf94f9de06f4c00df4e1c7acedd4)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/compare/0.1.0...1.0.0) - 2023-04-07

### Commits

- add  config_d management [`b18ccfb`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/b18ccfbfb248e30b7e29f7295aaa45db785283d0)
- doc: update changelog [`95f6ef5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/95f6ef5cb9b29b270965bddec2570d2993a14bfd)

## 0.1.0 - 2023-03-23

### Commits

- add code of conduc and small changes [`62585ef`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/62585ef440303f10439f8c80673b9590801c733a)
- add precommit for lint [`14c5464`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/14c546438621ffbf87264d9fe1086611921cfcc2)
- change default [`055950d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/055950d942b5f926eaf666e2a408ad3253bdbfcc)
- fix checks [`592c501`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/592c501aaa084fd48bf7fba9c786312139409459)
- add new molecule all scenario [`6db3daf`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/6db3daf9b0f10472645d8fc20a2630de809cef57)
- fix molecule failure [`5eb77b2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/5eb77b2092bc03adb4e52cad1e792110d5d3c2bb)
- update readme [`753a101`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/753a1010a348cc41e5dcd29b3887093115a9b6d6)
- change vars and just install by default [`5a4b360`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/5a4b360ccdaf25ee59a5ef96059260016d643a91)
- split distro for molecule tests on ci [`dff123d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/dff123d8663b4784e15e83ee73045005d8a3b1bc)
- test [`0135152`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/0135152f810b423394b61c6a9feeca034d630104)
- update checks and Readme [`4a25e47`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/4a25e47fdb0caf09ccb1abccecb2664a31572472)
- add molecule fix for ora9 [`a2ff7ef`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/a2ff7ef885b7afbf880dc5db4d3d16f6331d08e2)
- add ora9 support [`a7364a9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/a7364a933a911ed84529615c364dbe2144274fb7)
- lint [`bf51ae2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/bf51ae2ee20a68349898124c0024ecd48fcde666)
- update version [`abdd1e5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/abdd1e58372d7290047fbb697fd36c1501a4c418)
- test epel repo present [`14fc262`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/14fc262fd255e3ac386edec94a44b25b6cee79ce)
- fix checks [`57f600e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/57f600e23fb3dec713e17dd565179b23f4c7dd7b)
- lint: fix trailing space [`ad88a14`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/ad88a14f5439e98cfa622aed7c6eba58a3876ce9)
- remove apt cache / idempotence fail [`3508bb8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/3508bb836082ef2197ba4cf543fe4a8067de350d)
- initial commit [`5e8778c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_consul_agent/commit/5e8778c692aef706158a9b2c1f26d5920632fd9f)

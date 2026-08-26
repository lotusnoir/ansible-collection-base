# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/compare/2.0.0...2.1.0) - 2025-10-10

### Commits

- add support for ubuntu24 [`0bec4eb`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/0bec4eba0cc4eb07a3c346a3ec33e3939f62bb71)
- add version on molecule play image to maintain support on old release [`87f5d20`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/87f5d207ec3a991328c67e3d12f8e0b5799a23cc)
- remove support for debian10 / ubuntu18 / redhat8 [`5c17108`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/5c17108e7e7be489a9b311f56e30d0c95e1039d5)
- update molecule [`5a9d103`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/5a9d103a4f24dd75c776542f3ef58505e62fff1f)
- add redhat 9 to default supported distrib [`dedea6c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/dedea6cea9a2c648d046b102d0e36208880dc1db)
- add redhat 8 to default supported distrib [`3f942cf`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/3f942cf75cefbbdf159ed8e063b13ecd0ab8aaa0)
- sort testing distrib to avoid random changes [`6209e84`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/6209e8407cf5a0fc8cf0ce5a8ac54d69bc222ca3)
- update pre-commit and lint fix [`c511011`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/c511011086ce6e3259340054b6c1b7e46834edb5)
- remove duplicate signing key tasks [`01ee4ac`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/01ee4ac27353a3a919a37f4ce9cada432b8a73a7)
- remove support for rhel7 and docker dind on gitlab [`6d7f603`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/6d7f603d6c4de8ef4947b3bd1b48b773e83f7227)
- remove lint from pipeline [`a9cabd3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/a9cabd364fcc05f8ee6c9b9a5460ede20d370ac7)
- remove pipelines tests, moved in precommits [`33c0ea6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/33c0ea656633cb820b661b700489a0d08e702353)
- add retries on packages install, to avoid error if temp pkg lock [`e3f90c3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/e3f90c3518d2b34e54ec3389545f885c6e11d3c5)
- change facts var to be able to surcharge them on a pre_tasks [`aa6e4ae`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/aa6e4ae36f7cf4f39f8b02f62d4c457e546ee673)
- cleaning [`261e47d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/261e47d81a177ec8bd6ac937ecc9d5546d4a644d)
- missing header on ini file [`62cc71a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/62cc71a8d1084404ab7bde0da01f9b6e045e7a0e)
- fix exec path [`242f107`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/242f107b8791fe787b2686ef043b66cc55fa28a5)
- add repo install mode [`a3af807`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/a3af807c08959691ba3cff933155318161ee4695)
- doc: update changelog [`8b96dd6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/8b96dd65774f04f5a0b6477b9952c30661b45e7f)

## [2.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/compare/1.1.0...2.0.0) - 2025-10-10

### Commits

- remove install script in order to be able to select version and clean [`9d0c1cb`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/9d0c1cb1d9862de8fd86191983c06a4a20b906a3)
- update vars [`2719e56`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/2719e5685a212a4710602ac4017aef60a1a0a1ef)
- fix path vars for rhel7 [`170ad88`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/170ad881fa4c1b417058ba339637e95f8eb0e0ae)
- fix debian12 support [`ba77718`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/ba77718adc0779740addb8f17fdd531d7b575cf0)
- doc: update changelog [`7b0eaf8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/7b0eaf89aea66b8e62124ebea525f73d4898ae0a)

## [1.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/compare/1.0.0...1.1.0) - 2025-10-10

### Commits

- add debian12 support [`404a1d0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/404a1d0b6d8765634dd667149253719b9e8b226e)
- doc: update changelog [`75b97b0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/75b97b0f58b1a96433afaf87f5b61a3f607f1d08)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/compare/0.3.0...1.0.0) - 2025-10-10

### Commits

- add code of conduc and small changes [`5306b47`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/5306b47429bd5f6ec38d38f03d4a2f42320a7655)
- add precommit for lint [`90d0ece`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/90d0ece3e0712bab99462bcf293e0c399ab2c305)
- fix checks [`0da0f11`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/0da0f113bd1b98e06db359b87aac4ef95a5535ff)
- add new molecule all scenario [`1888105`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/188810553b7a9a2da1315f31d2630eb927835030)
- split distro for molecule tests on ci [`975f215`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/975f2155bc185c0c9cb81f89d538aa269b1cf324)
- update checks and Readme [`6890ddf`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/6890ddf5e7e77e6f73275efbcc9e1eae71623e56)
- fix molecule ora9 [`fde7fd5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/fde7fd53b36a64ae3726d945a60c980626af5c04)
- add vars + remove compile pkgs [`776dd9f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/776dd9fb5cc5f18264c0189b5c3b0e9645f1a419)
- fix checks [`aa97564`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/aa97564789fe28fcc5d12a1367a2f07c03f15bb6)
- add compatibility v2.5.0 [`46ad156`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/46ad15685f3f2fc7f509883e670acbca8a89c2e3)
- fix test and pipeline [`fc977aa`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/fc977aa41eb84c8f20bb8051d62bbc056795281b)
- change verify on molecule [`8bb6b71`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/8bb6b717f006e9a20676190f5a716aec487d4ceb)
- fix: molecule image and optimize scenario [`3e11e68`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/3e11e68c4704a7a7740e6c8f911a09a6a96930fe)
- fix lint [`a8ae3ef`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/a8ae3efb9ff0fa3f4b2f6be88942864e21017bbc)
- doc: update changelog [`d7e1924`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/d7e1924a71dc187f1f984eeb630024cbebcc141a)

## [0.3.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/compare/0.2.0...0.3.0) - 2025-10-10

### Commits

- minor: add oracleLinux7 support [`46122df`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/46122dfe7eec47ae987916aa1c25edb59f7a9d61)
- minor: add oracleLinux support + little fixes [`ad28e3f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/ad28e3f2713a1147da4982cb999a2f3ec37bbfd1)
- doc: update changelog [`ea929dc`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/ea929dcec8f6b6e3b0ccebcbbc5bd955dd9e455b)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/compare/0.1.0...0.2.0) - 2025-10-10

### Commits

- minor: add ubuntu 22 molecule test + fix lint [`6ca4326`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/6ca43265a804a33e2db17b5351ba2640d3c040df)
- fix: remove unsupported centos8 + minor fixes [`4addfa6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/4addfa625a8377d7d17dd8d3b65f6bf8aa6ee199)
- doc: update changelog [`5570e02`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/5570e02495240c72e08a22a8bf74e651726d4293)

## 0.1.0 - 2025-10-10

### Commits

- lint: remove space [`8595522`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/859552248418667ac88932771ef21f21921bb824)
- fix: add role name on molecule container names to avoid concurrent conflict on runners [`55c897d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/55c897d37315a7c8eb0b3b2f09010cff8fc332e1)
- minor: add changelog + automatic files checks [`01eb474`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/01eb474506cc1af9c9a3801d8bd387996fc3f9b2)
- initial commit [`ce74a90`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_snoopy/commit/ce74a906197be0c6af71e563fd502950262e2e41)

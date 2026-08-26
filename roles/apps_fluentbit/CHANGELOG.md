# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/compare/0.2.0...0.3.0) - 2025-10-10

### Commits

- doc: update changelog [`f6080b7`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/f6080b7362f30f7ba827b2ae537c26bf56f36b89)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/compare/0.1.0...0.2.0) - 2025-10-10

### Commits

- fix molecule paralelism and little updates [`fb0888d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/fb0888d2e8495973a24c23ed6fa59c3e4219367b)
- add support for ubuntu24 [`5a26af0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/5a26af0b429c709ab4c563be5bac34be94feab54)
- add version on molecule play image to maintain support on old release [`e77e8fa`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/e77e8fa29425e594f97af542049c2f5471b85a01)
- remove support for debian10 / ubuntu18 / redhat8 [`5bf2b84`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/5bf2b84ac1d48e1148a9360098f1b00f7fe94a8b)
- add retry on get_url key [`1e1d7a0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/1e1d7a09858728c99d95e9d0f33ea4d65e310afe)
- update molecule [`4ba6a9a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/4ba6a9aa4dbf2f316ead08e605770d029185dfc1)
- fix idempotence [`68c59a5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/68c59a5e78c784b25f34530605b810aa4a6fec87)
- add redhat 9 to default supported distrib [`0cdd998`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/0cdd99887ad5986b58b0adebbdb312cbe8b73353)
- fix redhat8 repo [`03eea69`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/03eea69f65f077b8c4077a89d5afe736b4231f89)
- add redhat8 support [`17b0262`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/17b0262b39e3f0397fb095ba4b7c6ecf6d1d6037)
- sort testing distrib to avoid random changes [`1ee84e5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/1ee84e5632c0bed8f4680f1fbb05dc76d9258864)
- fix install [`adcd1dc`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/adcd1dc2908477fd7ee339335bc82bd3dc56ceeb)
- templating upgrade [`4449aea`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/4449aea5d2fae0359d9512ed0bfdc1ffca018f1b)
- update from template + remove Key is stored in legacy trusted.gpg keyring [`d92b634`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/d92b6349845ba8c8564e5c5f347f8fa0f78aa5c3)
- add conf.d files [`998f1cf`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/998f1cf7075a6b154895abb4b353b0d531e756b7)
- add empty file when include is set [`e8dde51`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/e8dde5144328f56fa074e8df936dfd7b24bdb556)
- fix pkg name for redhat [`059028a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/059028ae47e2cd3080ae0a8ab5c23be9360673b8)
- add debian12 support + fix vars [`bcaa83e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/bcaa83eb857b330be88ffe83f9908f7052ee9f02)
- apply new templating [`2765248`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/27652487751356bb33c3a9524d136b431c0f3d34)
- add custom repo option [`faef80f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/faef80fa0f65c2c3990ff07f678765856eff1525)
- add custom repo option [`d1ff080`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/d1ff0801a62be50ed41c176098dd098d45c3886f)
- remove unsupported distrib [`37f50a2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/37f50a22ce77ef790700b2d7efdf3be68a906345)
- enable redhat repo [`1185c42`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/1185c426eadda28b7f89bcbe2083ea4629e92cfb)
- remove bad gpg repo and redhat [`525db92`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/525db925e6bb23d4e6e205f88dd6fefb8ce1fbf0)
- remove tabs on config file [`8168670`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/8168670679ac770e873d95edde5f3cd87db8be7e)
- add defaults parsers [`b42854d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/b42854d186e30678fdc034161c783eb9521595d7)
- fix and lint template [`0e61b6d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/0e61b6dc4e9611b8c3d565464b24faf94f1a488b)
- update vars [`cdc4d02`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/cdc4d02740590f6297cee9e78954b08757f15419)
- support Redhat and add option for install via repo [`7c48c4b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/7c48c4b425efa3c0c882de9eb9940d38492e1856)
- fix: remove unsupported centos8 + minor fixes [`bf5fd66`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/bf5fd6661a0e432ddce017a36387e09dc354c7f3)
- doc: update changelog [`1a0c70d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/1a0c70dfdf0157d5330b4ec5da7d5f9adc7b2bd3)

## 0.1.0 - 2025-10-10

### Commits

- fix: temporary remove molecule tests the time to fix them [`89747ac`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/89747acec6be4e41b2224ecd4ca09c2106dd632a)
- fix molecule + lint [`75663f5`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/75663f5a6b7028e9a9f0556267ec21ddaa292fab)
- add clean task [`c03ec6c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/c03ec6cc211c83567c3cd662d55357a27d344dd5)
- update vars + molecule [`44d2748`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/44d27487de73cc2c080e905671cb1c0f9fae1307)
- add vars and fixes [`a886f7d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/a886f7d8ec866fc55eab16a76addff4436b3618c)
- fix default path [`a09cfad`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/a09cfad595f4e9794e41978934c81c8e0e986b07)
- fix yamllint [`ad83c72`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/ad83c72d830f5e12258555f922b885c0e072b95c)
- update meta [`df5c149`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/df5c14916430d59f4440f417fa2e5a4e0ea561b2)
- initial commit [`86ad125`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_fluentbit/commit/86ad125308c7bb7357d2490b1cf17eaeccf0315c)

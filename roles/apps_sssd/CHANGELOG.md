# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/compare/1.0.0...1.1.0) - 2025-10-10

### Commits

- fix [`1df2b40`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/1df2b40f851367a49e28f5cc850047b72cc4a50b)
- add support for ubuntu24 [`cc7f552`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/cc7f552a98dfa0c76185ef1faafbdd16f8f3c220)
- add dbus pkg to activate ifp service [`c64172f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/c64172fa9369b5055fbd2137eeb6ddfc5c888353)
- add version on molecule play image to maintain support on old release [`60b1b34`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/60b1b34a56145e5b9847a18d5563ac860e8f60ec)
- remove support for debian10 / ubuntu18 / redhat8 [`2393495`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/2393495e3995c1700c546141a06728a8413bbc2e)
- update molecule [`2e10ccb`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/2e10ccbfabdc38d36d8c20ff5a77c82c11f3f32e)
- remove rocky support [`4b438a8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/4b438a842d2828c7c01b4fb850b048f571f8b427)
- remove redhat molecule checks [`ddfb52e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/ddfb52ec56dd1b8ff3075c8036596eafef49ff8c)
- sort testing distrib to avoid random changes [`153363a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/153363a6d5fecd207af68f7ca24ecbd6440bf651)
- fix shell ignore error on retry [`7051042`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/705104204d2b6a18f499925b808197f9fb6d1bad)
- add option to remove sssd from apparmor to reduce logs [`d7271dd`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/d7271dde331ed8326e3efeb43e91216aa8ef7381)
- remove support for rhel7 and docker dind on gitlab [`8b043f2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/8b043f27a595f8e0297118fc3022de1710d16707)
- remove unsupported rhel7 [`f7a09a6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/f7a09a6b184699491aafb41f5f067294ab3a5d0f)
- add retries on packages install, to avoid error if temp pkg lock [`97d959a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/97d959ab49bfe1d7064669a3da04896d9fc2a254)
- fix mkdir on debian [`11afdea`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/11afdea2857bf1ee382c2ae1bc71dc21fa269b03)
- doc: update changelog [`5416633`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/5416633dbe1052d7dcfbbe7d4dab80c479fbbf6d)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/compare/0.2.0...1.0.0) - 2025-10-10

### Commits

- fix oddjob doesnt start [`ba82cdf`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/ba82cdf0b1803ea239fa4f50de8af5870a5ba419)
- create home from oddjob instead of unsync authselect [`a4386ed`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/a4386ed8e2bcc791a0e417167f8585710eaa3c4c)
- fix home skel wrong order and never played [`d91c87e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/d91c87e6ef2ffbbce97a4d2d85d9934d39619129)
- update vars [`9f02417`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/9f024177573426db9c74b3609997f390abf7cd44)
- update readme + precommit + include vars [`cbaa2a3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/cbaa2a3406939f63a74d983fc6a55bb59b933af8)
- add ansible comment on template files [`84c49e4`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/84c49e491e93d61d7eab1a2780035e13e9877d55)
- change import tasks to include in order to support facts on name [`18023ff`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/18023ff1bcc90613f9b9a100196f1c04baf1427a)
- add umask variable [`45520d2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/45520d24ff531e112ce8d349fb81d77296163819)
- doc: update changelog [`e748e7e`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/e748e7ecc1ae44dcac20447e136dd775faaf3800)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/compare/0.1.0...0.2.0) - 2025-10-10

### Commits

- add debian12 support [`57858bd`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/57858bd2c10fb77f2a29f9189df8f1796f6ff214)
- add galaxy id [`88a1cb8`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/88a1cb8182effb150b737629042112571b963d09)
- doc: update changelog [`c358d59`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/c358d59af64231cac27660b27db689fac50afd53)

## 0.1.0 - 2025-10-10

### Commits

- lint [`21b1887`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/21b1887302393627ee7d8586028199a3c96a23de)
- add precommit for lint [`6acf343`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/6acf343ffb4fb55ee9a2633eec600c60bfef7469)
- add support for oraclelinux9 [`a7791aa`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/a7791aa90c23afdbbc287276b6c7af51e307afa5)
- fix checks [`3f9f4ad`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/3f9f4ad4b95375f60835421719ed94d8976d7666)
- update molecule playbook [`c03261d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/c03261d8687297850538c4483a31881a4bc4b9a2)
- add new molecule all scenario [`9ed56d2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/9ed56d2a9aa5e42e71f566e187d79fcef008cf34)
- split distro for molecule tests on ci [`7c6c8b3`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/7c6c8b3f6934ebff87670c32f1e15079339bb2c1)
- activate socker disable on debian11 [`27b55e9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/27b55e9e8baa47336d0f843bcc901cb9eb5d22d9)
- add molecule fix for ora9 [`8ee4f06`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/8ee4f06346d8edeb5c85dd153997d154069419a1)
- fix redhat8 [`cf60d5f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/cf60d5f61dd1351a67c875980bb5fef10a922f25)
- fix ttask condition [`e593f5b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/e593f5be039b6623fba2a4d669b3580a5dbcbef5)
- rebuild tasks with separate files [`4e65297`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/4e65297023c929e7c3a55e24f8fc034a6a017951)
- add sudoers on nswitch [`e6e8cf6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/e6e8cf64a924693924a059aaa66f1847f89f8566)
- add sudoers on nsswith for redhat8 [`3cabc4d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/3cabc4d63782cb70e103d2e5b934c28ce52f0de4)
- enable oddjob with authconfig [`067fd25`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/067fd253e4bf877e5dca6afcf66a7381531e3ea5)
- littles fixes [`f1844b2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/f1844b2708cfd7da9454cefcd0637552c59b1b88)
- remove centos [`a4f487a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/a4f487abd2b4dfbd05343914672e0d1be0d56af6)
- fix debian install [`29d4772`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/29d47726de3c44c18f455b046f24e2f15bf1b56f)
- fix debian install [`10307bd`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/10307bd87ba5a7e58d741fc538ebbfd6d53a0df8)
- fix lint [`dad1d26`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/dad1d261524887550bcac56a2bf60a0fe5a07cab)
- first commit [`8129e72`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_sssd/commit/8129e72cd62f3316330344ae138683846ecd65cc)

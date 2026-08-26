# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/compare/2.0.0...2.1.0) - 2025-10-10

### Commits

- add version on molecule play image to maintain support on old release [`200cd06`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/200cd065a183ec80d203666bc71376d09936e0c9)
- remove support for debian10 / ubuntu18 / redhat8 [`90f0f31`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/90f0f31b26e1eec7b943ff1c8d6d09e7ce4dfc28)
- update molecule [`090db08`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/090db08d7f5534da86b39f5157c4cd6e60c66210)
- remove rocky support [`296a832`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/296a832cf3af952ba326466654ce6016a39a16ea)
- remove redhat molecule checks [`e829655`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/e829655484f31c7a5707edeb631b26373816cb27)
- sort testing distrib to avoid random changes [`e552c29`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/e552c2949e2569b53b0cc4447f5993bf8f6fe728)
- update pre-commit and lint fix [`bd54e10`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/bd54e1008930e394afd8bc8d19963b9547748953)
- remove support for rhel7 and docker dind on gitlab [`5c114b1`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/5c114b154b319fdc956b3c81ecb7a3194122521a)
- remove unsupported rhel7 [`5450e3c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/5450e3ccdfdad54cdf18b899438edc1b34a4390a)
- add retries on packages install, to avoid error if temp pkg lock [`5006843`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/5006843c33f444a4f5e95b6bcd42c82396d25743)
- change facts var to be able to surcharge them on a pre_tasks [`185bb51`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/185bb518694a72ce35aeb833316980eca6588639)
- doc: update changelog [`fec6de6`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/fec6de6c4e30fd8029b8669ef102decc8ed50816)

## [2.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/compare/1.1.0...2.0.0) - 2025-10-10

### Commits

- update vars [`af0b303`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/af0b3035859788853779afc2aa2674921316f0fc)
- remove precommit gitlab [`2ed356a`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/2ed356a3e43bcc0bc85ca6a3f885cc014b048805)
- update readme + precommit + include vars [`1063f93`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/1063f93ba69a47be14658ac0df03dfb50a6ce588)
- rename template files [`dbc4e6f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/dbc4e6fea10b4cddae893c269dd90018ef2d3066)
- fix vars [`4d7b4d4`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/4d7b4d4edf55ae00e3bb6ff36719d567981cbf15)
- remove ubuntu18 support [`3e1dc92`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/3e1dc92f6384a1ebbe9c6d2f7684d7339ca1b7dd)

## [1.1.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/compare/1.0.0...1.1.0) - 2025-10-10

### Commits

- doc: update changelog [`4c2a7bc`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/4c2a7bcc748a5462c6eaada42fe45267b82327df)

## [1.0.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/compare/0.3.0...1.0.0) - 2025-10-10

### Commits

- add code of conduc and small changes [`fcb7cc4`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/fcb7cc4cd919ba28db5adc58743035cc8eb020e5)
- add precommit for lint [`b0d790b`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/b0d790b25dc052d5eec2647dcdeac76bd656b883)
- fix checks [`8d93d4f`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/8d93d4fc2e6533c3debc5c93ec8f7bce8102350d)
- add new molecule all scenario [`5f875d2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/5f875d20e57e1aa06f4db174adda3cd6fabf05f2)
- split distro for molecule tests on ci [`23538de`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/23538ded3e9086033b5899f5751876e7e84fb2b1)
- update checks and Readme [`5cc4771`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/5cc4771e7b6b251fc03e31a8870095605eda6e31)
- remove verify [`a479c58`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/a479c58286efa5ea20bf1034485f4cb6b7d3efbc)
- fix checks [`e4ae3a0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/e4ae3a0dc4a28549f1cebd464d5b62af4cf1c6b1)
- change dnf to yum for major7 compatibility [`8d3b3d0`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/8d3b3d0d6fffb94125bb8d36306a6d2c8687e6f7)
- doc: update changelog [`e9f41a7`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/e9f41a7b7c840d62b10fd383cae5565694aa5404)

## [0.3.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/compare/0.2.0...0.3.0) - 2025-10-10

### Commits

- minor: add oracleLinux7 support [`1b01840`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/1b01840e1e7acfe9a673ab1d18de02854ac693a2)
- doc: update changelog [`855d2a9`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/855d2a98b30a37585c45f9240b77518f59aef3fb)

## [0.2.0](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/compare/0.1.0...0.2.0) - 2025-10-10

### Commits

- minor: add oracleLinux support + little fixes [`5bbb97d`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/5bbb97dac2054f9bffaf0dea24c113714fb514f2)
- fix: remove unsupported centos8 + minor fixes [`5245451`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/52454511d7c8e0834dabac98f5540290ed3ce32d)
- fix: Changes on README + molecule container names [`7a03ea2`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/7a03ea2ef0c259270da71921097370ad9454040c)
- doc: update changelog [`bfc9dc1`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/bfc9dc1808a3c1616c4c084ced925f04bc04dc7e)

## 0.1.0 - 2025-10-10

### Commits

- fix: add role name on molecule container names to avoid concurrent conflict on runners [`10ae040`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/10ae040c5138c5f3254bf3608ebe10ae8eb626cf)
- minor: add changelog + automatic files checks [`129ea7c`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/129ea7c6161ba873379d8b79f6701dd6b0b32a1a)
- initial commit [`cefb200`](https://gitlab.pleal.ovh/ansible-roles_base/ansible-apps_rsyslog_exporter/commit/cefb2007ad6820d01e9faa303c30a12d84250f73)

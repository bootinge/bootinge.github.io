---
layout: default
title: Openstack Horizon
---

# horizon

## 프로젝트 구성

### 의존성

| Depencencies | 설명 |
|-----|:----:|
| pbr | injects some useful and sensible default behaviors into your setuptools run |
| Babel | Babel is an internationalization library for Python |
| Django | Web Framework |
| Pint | Python package to define, operate and manipulate physical quantities |
| django_compressor | static 파일 compressor |
| django_openstack_auth | openstack auth 모듈 |
| django-pyscss | django scss(stylesheet) 프레임워크 |
| eventlet | 비동기 I/O 프로그래밍 라이브러리 |
| httplib2 | http 통신 모듈 |
| iso8601 | 날짜 모듈 |
| kombu | Kombu is a messaging library for Python. |
| netaddr | 네트워크 주소 모듈 |
| oslo.* | OpenStack 관련 모듈 |
| pyScss | compiler for the Sass language |
| pytz | WorldWide Timezone 모듈 |
| PyYAML | YAML implementations for Python |
| six | Python 2 and 3 compatibility library |
| XStatic | tatic file packages with minimal overhead |
| XStatic-Angular | |
| XStatic-Angular-Bootstrap | |
| XStatic-Angular-lrdragndrop | A drag and drop module for angularjs |
| XStatic-Bootstrap-Datepicker | |
| XStatic-Bootstrap-SCSS | |
| XStatic-D3 | Graphics Library |
| XStatic-Hogan | Twitter related Library.|
| XStatic-Font-Awesome | |
| XStatic-Jasmine | BDD Framework |
| XStatic-jQuery | |
| XStatic-JQuery-Migrate | |
| XStatic-JQuery.quicksearch | A jQuery based plug-in for filtering large data sets with user input |
| XStatic-JQuery.TableSorter | Flexible client-side table sorting |
| XStatic-jquery-ui | |
| XStatic-JSEncrypt | RSA Encrypt/Decrypt Library |
| XStatic-Magic-Search | AngularJS directive that provides a UI for both faceted filtering and as-you-type filtering |
| XStatic-QUnit | Javascript Unit Test Framework|
| XStatic-Rickshaw | JavaScript toolkit for creating interactive time-series graphs and charts.|
| XStatic-smart-table | Smart table is a table module for angular js  |
| XStatic-Spin | |
| XStatic-term.js | A terminal written in javascript. |

### 파일 설명
* manifest.in : Specifying the files to distribute.

### 디렉토리 구조
```
.
├── doc								# Sphinx Documentation
├── horizon							# Django Base Path
│   ├── browsers					# Resource Browser(?)
│   ├── conf
│   │   ├── dash_template
│   │   └── panel_template
│   ├── contrib						# 국제 시간 관련 유틸.
│   ├── forms						# From Controller
│   ├── locale						# 국제화.
│   ├── management					# manage.py에 관리 명령을 추가
│   │   └── commands				# startdash, startpanel 명령 정의 클래스.
│   ├── static						# Static Files
│   ├── tables
│   ├── tabs
│   ├── templates					# View Files
│   ├── templatetags				# Filters for template
│   ├── test						# Test Cases
│   ├── utils
│   └── workflows             # A Workflow is a collection of Steps.
├── openstack_dashboard				# App Folder
│   ├── api               # Rest API와 관련된 폴더.
│   │   └── rest          # API의 Controller에 해당하는 부분.
│   ├── conf
│   ├── dashboards        # 각 구성요소에 대한 대시보드 부분.
│   │   ├── admin
│   │   ├── identity
│   │   ├── project       # 프로젝트에 대한 대시보드(Model, View, Controller)
│   │   ├── router
│   │   └── settings
│   ├── django_pyscss_fix
│   ├── enabled
│   ├── local
│   │   └── enabled
│   ├── locale						# 국제화
│   ├── management        # manage.py에 관리 명령을 추가
│   │   └── commands      # nginx, apache, wsgi 관련 명령도 추가.
│   ├── openstack         # 공통 유틸리티들.
│   │   └── common
│   ├── static						# Static Files
│   ├── templates
│   ├── templatetags			# Filters for template
│   ├── test						  # Test Cases
│   ├── usage             # 사용량에 대한 View를 제공.
│   ├── utils
│   └── wsgi
└── tools							# Virtual Envronment 구성 툴.
```

---
layout: default
title: Django Mocking
---

# Mocking in Python
- Mocking makes unit testing easier!!!

## Great Unit Testing Myths
- They’re good when the problem is easy.
- I spend too much time writing lots of code to test, so I give up
- There’s just some stuff you can’t unit test.

## What are Mocks?
### Test Double
- Dummy Object
- Test Stub
  - Provide a canned response to method calls
- Test Spy
  - Real objects that behave like normal except when a specific condition is met
- Mock Object
  - Provide a canned response to method calls
- Fake Object

## Quick Guide
- http://mock.readthedocs.org/en/latest/

### Mock, MagicMock
- *Mock* and *MagicMock* objects create all attributes and methods as you access them and store details of how they have been used.

```
>>> from mock import MagicMock
>>> thing = ProductionClass()
>>> thing.method = MagicMock(return_value=3)
>>> thing.method(3, 4, 5, key='value')
3
>>> thing.method.assert_called_with(3, 4, 5, key='value')
```

### side_effect
- *side_effect* allows you to perform side effects, including raising an exception when a mock is called

```
>>> mock = Mock(side_effect=KeyError('foo'))
>>> mock()
Traceback (most recent call last):
 ...
KeyError: 'foo'

>>> values = {'a': 1, 'b': 2, 'c': 3}
>>> def side_effect(arg):
...     return values[arg]
...
>>> mock.side_effect = side_effect
>>> mock('a'), mock('b'), mock('c')
(1, 2, 3)
>>> mock.side_effect = [5, 4, 3, 2, 1]
>>> mock(), mock(), mock()
(5, 4, 3)
```

### patch
- The patch() decorator / context manager makes it easy to mock classes or objects in a module under test.
- The object you specify will be replaced with a mock (or other object) during the test and restored when the test ends

```
>>> from mock import patch
>>> @patch('module.ClassName2')
... @patch('module.ClassName1')
... def test(MockClass1, MockClass2):
...     module.ClassName1()
...     module.ClassName2()

...     assert MockClass1 is module.ClassName1
...     assert MockClass2 is module.ClassName2
...     assert MockClass1.called
...     assert MockClass2.called
...
>>> test()
```

## Example - Fear System Calls
```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os

def rm(filename):
    os.remove(filename)
```

### TestCase without Mock
```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from mymodule import rm

import os.path
import tempfile
import unittest

class RmTestCase(unittest.TestCase):

    tmpfilepath = os.path.join(tempfile.gettempdir(), "tmp-testfile")

    def setUp(self):
        with open(self.tmpfilepath, "wb") as f:
            f.write("Delete me!")

    def test_rm(self):
        # remove the file
        rm(self.tmpfilepath)
        # test that it was actually removed
        self.assertFalse(os.path.isfile(self.tmpfilepath), "Failed to remove the file.")
```

### Testcase with Mock
```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from mymodule import rm

import mock
import unittest

class RmTestCase(unittest.TestCase):

    @mock.patch('mymodule.os')
    def test_rm(self, mock_os):
        rm("any path")
        # test that rm called os.remove with the right parameters
        mock_os.remove.assert_called_with("any path")
```

### Adding Validation to 'rm'
```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os
import os.path

def rm(filename):
    if os.path.isfile(filename):
        os.remove(filename)
```

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from mymodule import rm

import mock
import unittest

class RmTestCase(unittest.TestCase):

    @mock.patch('mymodule.os.path')
    @mock.patch('mymodule.os')
    def test_rm(self, mock_os, mock_path):
        # set up the mock
        mock_path.isfile.return_value = False

        rm("any path")

        # test that the remove call was NOT called.
        self.assertFalse(mock_os.remove.called, "Failed to not remove the file if not present.")

        # make the file 'exist'
        mock_path.isfile.return_value = True

        rm("any path")

        mock_os.remove.assert_called_with("any path")
```

- We now can verify and validate internal functionality of methods without any side-effects.

### File-Removal as a Service

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os
import os.path

class RemovalService(object):
    """A service for removing objects from the filesystem."""

    def rm(filename):
        if os.path.isfile(filename):
            os.remove(filename)
```

### Tetscase for Service
```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from mymodule import RemovalService

import mock
import unittest

class RemovalServiceTestCase(unittest.TestCase):

    @mock.patch('mymodule.os.path')
    @mock.patch('mymodule.os')
    def test_rm(self, mock_os, mock_path):
        # instantiate our service
        reference = RemovalService()

        # set up the mock
        mock_path.isfile.return_value = False

        reference.rm("any path")

        # test that the remove call was NOT called.
        self.assertFalse(mock_os.remove.called, "Failed to not remove the file if not present.")

        # make the file 'exist'
        mock_path.isfile.return_value = True

        reference.rm("any path")

        mock_os.remove.assert_called_with("any path")
```

# 테스트시 주의할 점.
## 시나리오 기반 테스트
- testscenarios 모듈을 기반으로 한 테스트를 하지 말것.

> Edit 2/7/2012: This is a moderately **bad approach to the problem**. While an interesting hack I highly discourage using this in a production environment. If you want this approach in python I recommend checking out the **lettuce** project for a much more sane approach.

## Cucumber 사용
-  Cucumber를 사용할 경우 feature 문서를 변경하면 StepDefinitions 소스 코드까지 수정해야 하기 때문에 유지보수 비용이 좀 발생할 듯하다.
- 프로그래머 레벨에서는 cucumber를 사용하지 마라.
> - 많은 프로그래머들이 cucumber를 integration test로 잘못 사용하고 있다.
  - cucumber의 용도는 보다 하이 레벨의 분석툴이며, 이를 acceptance test용으로 사용하기에는 프로그래머 입장에선 불편한 점이 많다.
  - 당신이 프로그래머라면 cucumber를 integration test로 사용하면서 불편하게 살지 말고, 그럴 바에야 acceptance test 안한다는거 인정하고 Capybara 같은 pure integration test 도구를 사용해라.

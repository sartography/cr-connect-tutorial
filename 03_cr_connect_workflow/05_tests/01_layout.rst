======
Layout
======

Tests are in the **/tests** directory.

The test directory has 6 subdirectories; **data**, **emails**, **files**, **ldap**, **study**, and **workflow**.

.. code-block::

    tests/
        data/
        emails/
        files/
        ldap/
        study/
        workflow/
        base_test.py
    ...

The `data` directory holds workflows (BPMN files) for tests.

The other directories organize tests by topic. Some tests are in the tests directory itself.

This is not a perfect system.

Our long-term goal for tests is to mimic the layout of the crc directory.

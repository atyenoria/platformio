.. _ci_circleci:

Circle CI
=========

`Circle CI <https://circleci.com/about>`_ is a hosted cloud
platform that provides hosted continuous integration, deployment, and testing
to `GitHub <http://en.wikipedia.org/wiki/GitHub>`_ repositories.

Circle CI is configured by adding a file named ``circle.yml``, which is a
`YAML <http://en.wikipedia.org/wiki/YAML>`_ format text file, to the root
directory of the GitHub repository.

Circle CI automatically detects when a commit has been made and pushed to a
GitHub repository that is using Circle CI, and each time this happens, it will
try to build the project using :ref:`cmd_ci` command. This includes commits to
all branches, not just to the master branch. Circle CI will also build and run
pull requests. When that process has completed, it will notify a developer in
the way it has been configured to do so — for example, by sending an email
containing the build results (showing success or failure), or by posting a
message on an IRC channel. It can be configured to build project on a range of
different :ref:`platforms`.

Integration
-----------

Put ``circle.yml`` to the root directory of the GitHub repository.

.. code-block:: yaml

    machine:
        environment:
            PLATFORMIO_CI_SRC: path/to/source/file.c
            PLATFORMIO_CI_SRC: path/to/source/file.ino
            PLATFORMIO_CI_SRC: path/to/source/directory

    dependencies:
        pre:
            - sudo apt-get install python2.7-dev
            - sudo python -c "$(curl -fsSL https://raw.githubusercontent.com/platformio/platformio/master/scripts/get-platformio.py)"

    test:
        override:
            - platformio ci --board=TYPE_1 --board=TYPE_2 --board=TYPE_N


For more details as for PlatformIO build process please look into :ref:`cmd_ci`
command.

Examples
--------

1. Integration for `USB_Host_Shield_2.0 <https://github.com/felis/USB_Host_Shield_2.0>`_
   project. The ``circle.yml`` configuration file:

.. code-block:: yaml

    machine:
        environment:
            PLATFORMIO_CI_SRC: examples/Bluetooth/PS3SPP/PS3SPP.ino
            PLATFORMIO_CI_SRC: examples/pl2303/pl2303_gps/pl2303_gps.ino

    dependencies:
        pre:
            - sudo apt-get install python2.7-dev
            - sudo python -c "$(curl -fsSL https://raw.githubusercontent.com/platformio/platformio/master/scripts/get-platformio.py)"
            - wget https://github.com/xxxajk/spi4teensy3/archive/master.zip -O /tmp/spi4teensy3.zip
            - unzip /tmp/spi4teensy3.zip -d /tmp

    test:
        override:
            - platformio ci --lib="." --lib="/tmp/spi4teensy3-master" --board=uno --board=teensy31 --board=due

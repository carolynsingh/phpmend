PHPUNIT DEBUGGING IN DOCKER
=================================

.. note::

   This documentation works with PHP Storm version 2022.3.3.

.. note::

   Make sure your docker is sudoless , if not run the command "sudo usermod -aG docker $USER" and then restart the system.


PREREQUISITES
------------------

1. Stop existing containers if any.

2. Remove unused data from your Docker system, including stopped containers, dangling images, and unused networks.

.. code-block:: bash

   docker system prune --all

3. Volumes are not pruned by default, and you must specify the --volumes flag for docker system prune to prune volumes.

.. code-block:: bash

   docker volume prune

SETTING UP THE PROJECT
--------------------------

1. Clone and open the project (Apple or Apple Style) in PHP storm.


2. In 'deploy/xdebug' directory , open 50_xdebug.ini file

.. image:: images/img25.png


In xdebug.client_host key , provide your IP address.

.. image:: images/img1.png

3. Set up the project

.. code-block:: bash

   make setup

.. image:: images/img28.png

.. code-block:: bash

   make upbg

.. image:: images/img30.png

.. note::

   If errors are encountered while bringing up the project run the following commands :

.. code-block:: bash

   sudo service nginx stop

.. code-block:: bash

   sudo service redis-server stop

.. image:: images/img5.png


.. code-block:: bash

   make db-fresh

.. image:: images/img29.png


4. Install Debugger

.. code-block:: bash

   make install-xdebug

.. image:: images/img22.png

.. code-block:: bash

   make setup-xdebug

.. image:: images/img23.png

.. code-block:: bash

   make restart-fpm

.. image:: images/img24.png

SETTING UP CLI INTERPRETER
----------------------------

1. Go to settings > Php

.. image:: images/img2.png

2. Click the three dots on the right of CLI interpreter and select 'From Docker, Vagrant, VM, WSL, Remote'

.. image:: images/img3.png

3. Configure remote PHP interpreter and select Docker Compose and click on 'New'

.. image:: images/img4.png


4. After selecting Docker Compose connect to Docker daemon

.. image:: images/img6.png

5. Click on add

.. image:: images/img7.png

6. Select activate_api from folder

.. image:: images/img8.png

7. Choose connect to existing container

.. image:: images/img9.png

8. Add configuration directives and values

.. image:: images/img10.png

9. Go to settings > Php > Test Frameworks

.. image:: images/img11.png

10. Click on '+' icon and select 'PHPUnit by Remote interpreter'

.. image:: images/img12.png

11. Select 'activate_api' interpreter from the drop down

.. image:: images/img13.png

12. After selecting 'activate_api' , select path to script from vendor>bin>paratest_for_phpstorm

.. image:: images/img_1.png

13. Go to settings > php > Debug and change the debug port to 9070

.. image:: images/img15.png

14. Go to Current File > Edit configurations

.. image:: images/img16.png

15. Add new configuration by selecting '+' icon and select 'PHPUnit'

.. image:: images/img17.png

16. Name it as 'NewLoginTest'

    Select test scope as 'Class'

    Give path to class and file

    Use alternative configuration file

.. image:: images/img26.png

17. Open tests > Feature > Http > Controllers > AccountController > NewLoginTest.php

.. image:: images/img31.png

18. Run test

.. image:: images/img32.png

.. image:: images/img33.png

19. Run debugger

.. image:: images/img34.png

.. image:: images/img35.png

















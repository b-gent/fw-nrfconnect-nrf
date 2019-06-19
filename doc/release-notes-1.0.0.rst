.. _ncs_release_notes_100:

|NCS| v1.0.0 Release Notes
##########################

This project is hosted by Nordic Semiconductor to demonstrate the integration of Nordic SoC support in open source projects, like MCUBoot and the Zephyr RTOS, with libraries and source code for low-power wireless applications.

nRF Connect SDK v1.0.0 supports development with nRF9160 Cellular IoT devices.
It contains references and code for Bluetooth Low Energy devices in the nRF52 Series, though development on these devices is not currently supported with the nRF Connect SDK.

Highlights
**********

*
*
*
*
*

Release tag
***********

The release tag for the |NCS| manifest repository (|ncs_repo|) is **v1.0.0**.
Check the ``west.yml`` file for the corresponding tags in the project repositories.

To use this release, check out the tag in the manifest repository and run ``west update``.
See :ref:`Getting the nRF Connect SDK code <cloning_the_repositories_win>` for more information.


Supported boards
****************

* PCA10090 (nRF9160 DK) - only for LTE samples
* PCA10056 (nRF52840 Development Kit)
* PCA10059 (nRF52840 Dongle)
* PCA10040 (nRF52 Development Kit)
* PCA10028 (nRF51 Development Kit)
* PCA63519 (Smart Remote 3 DK add-on)


Required tools
**************

In addition to the tools mentioned in :ref:`gs_installing`, the following tool versions are required to work with the |NCS|:

.. list-table::
   :header-rows: 1

   * - Tool
     - Version
     - Download link
   * - SEGGER J-Link
     - V6.41
     - `J-Link Software and Documentation Pack`_
   * - nRF5x Command Line Tools
     - v9.8.1
     - `nRF5x Command Line Tools`_
   * - dtc (Linux only)
     - v1.4.6 or later
     - :ref:`gs_installing_tools_linux`


As IDE, we recommend to use |SES| (Nordic Edition), version 4.16 or later.
It is available from the following links:

* `SEGGER Embedded Studio (Nordic Edition) - Windows x86`_
* `SEGGER Embedded Studio (Nordic Edition) - Windows x64`_
* `SEGGER Embedded Studio (Nordic Edition) - Mac OS x64`_
* `SEGGER Embedded Studio (Nordic Edition) - Linux x86`_
* `SEGGER Embedded Studio (Nordic Edition) - Linux x64`_


Change log
**********

The following sections provide detailed lists of changes by component.

nRF9160
=======

* Added the following samples:

  * :ref:`aws_fota_sample` - shows how to perform over-the-air firmware updates of an nRF9160 via MQTT and HTTP.
  * :ref:`secure_partition_manager` - required to set up the nRF9160 DK so that it can run user applications in the non-secure domain.
  * :ref:`secure_services` - demonstrates using the reboot and random number services.

* Added the following libraries:

  * :ref:`lib_fota_download` - handles Firmware Over The Air (FOTA) downloads.
  * :ref:`lib_aws_fota` - combines the :ref:`lib_aws_jobs` and :ref:`lib_fota_download` libraries to create a user-friendly library that can perform firmware-over-the-air (FOTA) update using HTTP and MQTT TLS.
  * :ref:`at_cmd_readme` - facilitates handling of AT Commands by multiple modules.
  * :ref:`lib_aws_jobs` - facilitates communication with the AWS IoT Jobs service.

* Asset Tracker sample:

  * The orientation detector now supports interrupt handling.

* nRF Connect SDK now uses upstream CoAP implementation. The :ref:`mqtt_simple_sample` sample was rewritten to use the upstream library, and the downstream CoAP was removed.
* The :ref:`http_application_update_sample` sample has been updated to use the :ref:`lib_fota_download` library.

BSD library
-----------

* Updated bsdlib to version 0.3.1.
* Introduced a new header bsdlib.h to be used by the application to initialize and shutdown the library.
* Library initialization during system initialization (``SYS_INIT``) is now optional, and controlled via ``Kconfig``. The default behavior is unchanged.

Secure Partition Manager (SPM) library
--------------------------------------

* Added random number secure service, providing access to the RNG hardware from the non-secure firmware.
* Non-Secure callable support:

  * A secure_services module is now available over secure entry functions. This means:

    * :file:`secure_services.c` resides in secure firmware (SPM).
    * :file`secure_services.h` declares functions that can be called from non-secure firmware.

  * SPM now exposes secure entry functions by default.
  * Added reboot as a secure service. The reboot secure service is called when the non-secure firmware calls sys_reboot().

* PWM0-3 added as non-secure.


Common libraries
================

* Added the following libraries:

  * :ref:`ppi_trace` - enables tracing of hardware peripheral events on pins.

Enhanced Shockburst
-------------------

* Added support for nRF52811.

Download Client
---------------

* Added IPv6 support, with fallback to IPv4.
* Added HTTPS support. The application must provision the TLS security credentials.
* Several improvements to buffer handling and network code.
* Library now runs in a separate thread.


Crypto
======

*
*

nRF BLE Controller
==================

* Added support for the nRF BLE controller 0.2.0-prealpha. Includes drivers to access HCI, flash, clock control, and entropy hardware.

Subsystems
==========

Bluetooth Low Energy
--------------------

* Added the following samples:

  * :ref:`central_bas` - demonstrates how do use the :ref:`bas_c_readme` to receive battery level information from a compatible device.
  * :ref:`shell_bt_nus` - demonstrates how to use the :ref:`shell_bt_nus_readme` to receive shell commands from a remote device.

* Added the following libraries:

  * :ref:`bas_c_readme` - used to retrieve information about the battery level from a device.
  * :ref:`shell_bt_nus_readme` - allows for sending shell commands from a host to the application.

* Added :ref:`ble_console_readme` - a desktop application that can be used to communicate with an nRF device over *Bluetooth* Low Energy using the :ref:`shell_bt_nus_readme`.

NFC
---

*

Partition Manager
-------------

* Partition Manager can now implicitly or explicitly assign a HEX file to a partition.
* :ref:`ug_pm_static` of upgradable images is now supported.


nRF Desktop
===========

* The nrf_desktop reference implementation is moved from the ``samples/`` folder to ``applications/``.
* The nrf_desktop configuration channel now allows data to be exchanged between the device and host in both directions.

Build and configuration system
==============================

???

Documentation  - WORK IN PROGRESS
=============

* Added or updated documentation for the following samples:

  *
  *
  *
  *

* Added or updated documentation for the following libraries:

  *
  *
  *
  *
  *
  *
  *
  *
  *

* Added or updated the following documentation:

  *
  *
  *
  *
  *
  *


Known issues
************

nRF9160
=======

* The :ref:`asset_tracker` sample does not wait for connection to nRF Cloud before trying to send data.
  This causes the sample to crash if the user toggles one of the switches before the board is connected to the cloud.
* The :ref:`asset_tracker` sample might show up to 2.5 mA current consumption in idle mode with ``CONFIG_POWER_OPTIMIZATION_ENABLE=y``.
* If a debugger (for example, J-Link) is connected via SWD to the nRF9160, the modem firmware will reset.
  Therefore, the LTE modem cannot be operational during debug sessions.
* The SEGGER Control Block cannot be found by automatic search by the RTT Viewer/Logger.
  As a workaround, set the RTT Control Block address to 0 and it will try to search from address 0 and upwards.
  If this does not work, look in the ``builddir/zephyr/zephyr.map`` file to find the address of the ``_SEGGER_RTT`` symbol in the map file and use that as input to the viewer/logger.

Crypto
======

* The :ref:`nrfxlib:nrf_security` glue layer is broken because symbol renaming is not handled correctly.
  Therefore, the behavior is undefined when selecting multiple back-ends for the same algorithm (for example, AES).


Subsystems
==========

Bluetooth Low Energy
--------------------

* :ref:`peripheral_lbs` does not report the Button 1 state correctly.
* The central samples (:ref:`central_uart`, :ref:`bluetooth_central_hids`) do not support any pairing methods with MITM protection.
* The :ref:`gatt_dm_readme` is not supported on nRF51 devices.
* Bluetooth LE samples cannot be built with the :ref:`nrfxlib:ble_controller` v0.1.0.

Bootloader
----------

* Building and programming the immutable bootloader (see :ref:`ug_bootloader`) is not supported in SEGGER Embedded Studio.
* The immutable bootlader can only be used with the following boards:

  * nrf52840_pca10056
  * nrf9160_pca10090

DFU
---

* Firmware upgrade using mcumgr or USB DFU is broken for multi-image builds, because the device tree configuration is not used.
  Therefore, it is not possible to upload the image.
  To work around this problem, build MCUboot and the application separately.


nrfxlib
=======

* GNSS sockets implementation is experimental.

 * Implementation might hard-fault when GPS is in running mode and messages are not read fast enough.
 * NMEA strings are valid c-strings (0-terminated), but the read function might return wrong length.
 * Sockets can only be closed when GPS is in stopped mode.
 * Closing a socket does not properly clean up all memory resources.
   If a socket is opened and closed multiple times, this  might starve the system.
 * Forcing a cold start and writing AGPS data is not yet supported.

nrfx 1.7.1
==========

* nrfx_saadc driver:
  Samples might be swapped when buffer is set after starting the sample process, when more than one channel is sampled.
  This can happen when the sample task is connected using PPI and setting buffers and sampling are not synchronized.
* The nrfx_uarte driver does not disable RX and TX in uninit, which can cause higher power consumption.
* The nrfx_uart driver might incorrectly set the internal tx_buffer_length variable when high optimization level is set during compilation.

In addition to the known issues above, check the current issues in the `official Zephyr repository`_, since these might apply to the |NCS| fork of the Zephyr repository as well.
To get help and report issues that are not related to Zephyr but to the |NCS|, go to Nordic's `DevZone`_.

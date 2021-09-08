# Ubertooth

Project Ubertooth is an open source wireless development platform
suitable for Bluetooth experimentation. Ubertooth ships with a capable
BLE (Bluetooth Smart) sniffer and can sniff some data from Basic Rate
(BR) Bluetooth Classic connections.

The latest release is [2020-12-R1](https://github.com/greatscottgadgets/ubertooth/releases/tag/2020-12-R1).
The latest firmware build can be found on the release page.

This release is paired with [libbtbb 2020-12-R1](https://github.com/greatscottgadgets/libbtbb/releases/tag/2020-12-R1).

Instructions for flashing the firmware can be found [on the corresponding Wiki page](https://github.com/greatscottgadgets/ubertooth/wiki/Firmware).
Instructions for building libbrbb can be found [on the corresponding Wiki page](https://github.com/greatscottgadgets/ubertooth/wiki/Build-Guide).

--------------------

# Documentation

Documentation for Ubertooth can be viewed on [Read the Docs](https://ubertooth.readthedocs.io/en/latest/). The raw documenation files for Ubertooth are in the [docs folder](https://github.com/greatscottgadgets/ubertooth/tree/master/docs) in this repository. Documentation changes can be submitted through pull request and suggestions can be made as GitHub issues. 

--------------------

# Getting Help

Before asking for help with Ubertooth, check to see if your question is listed in the [FAQ](https://ubertooth.readthedocs.io/en/latest/faq.html).

For assistance with Ubertooth general use or development, please look at the [issues on the GitHub project](https://github.com/greatscottgadgets/ubertooth/issues). This is the preferred place to ask questions or open issues so that others may locate the answer to your question in the future.

We invite you to join our community discussions on [Discord](https://discord.gg/rsfMw3rsU8) and [IRC](https://web.libera.chat/#ubertooth). Note that while technical support requests are welcome here, we do not have support staff on duty at all times. Be sure to also submit an issue on GitHub if you’ve found a bug or if you want to ensure that your request will be tracked and not overlooked.

GitHub issues on this repository that are labelled "technical support" by Great Scott Gadgets employees can expect a response time of two weeks. We currently do not have expected response times for other GitHub issues or pull requests for this repository. 


How to apply the modification to Ubertooth One
----------------------------------------------
The algorithm used can be found in details in the paper [A Robust Algorithm for Sniffinf BLE Long-Lived Connections in Real-time](https://arxiv.org/abs/1907.12782)

The code is modified to get a proof of concept of the algorithm.

The main firmware modification is made to the bluetooth_rxtx.c file located in firmware/bluetooth_rxtx folder. 
Navigate to the folder and open the file bluetooth_rxtx.c file and edit the lines 81, 82 and 83 to set the value to idx1, idx2 and idx3. These referes to the channel index to hop to for calculating the ble parameters necessary for follwing the connection.

Steps
-----
1. Run Ubertooth One is follow mode and get the AA for the specified connection. You dint need to run in follow mode if you already know the AA of the connection to follow.
2. Modify the bluetooth_rxtx.c file to change the indices of the channel to hop to get the ble connection parameters. Then run -

make clean all && make
ubertooth-dfu -r -d bluetooth_rxtx/bluetooth_rxtx.dfu

3. Then set the AA of the BLE connection to sniff and follow

sudo ubertooth-btle -a8e89bed6 //where 8e89bed6 is the AA, you put your own AA there

4. Then run the promiscious mode for Ubertooth One

sudo ubertooth-btle -p

Notes and future works
----------------------

I am currently working on an algorithm to determine the AA for all the BLE connections available and get a probabilistic idea about the channel map to start with (i.e. values for idx1, idx2 and idx3).

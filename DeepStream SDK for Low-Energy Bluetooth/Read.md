# Setting Up Ubuntu and NVIDIA DeepStream SDK for Low-Energy Bluetooth (BLE) Communication

This guide walks you through setting up your Ubuntu environment with NVIDIA DeepStream SDK (DSX) to enable reading and writing data to low-energy Bluetooth (BLE) chips.

---

## **1. Update Your Environment**

### Update and Upgrade Ubuntu:
```bash
sudo apt update && sudo apt upgrade -y
```

### Install Required Dependencies:
```bash
sudo apt install build-essential libbluetooth-dev bluetooth bluez python3-pip -y
```

### Install NVIDIA Drivers and CUDA:
Follow [NVIDIA’s driver installation guide](https://developer.nvidia.com/cuda-downloads) to install the necessary GPU drivers and CUDA toolkit.

---

## **2. Install NVIDIA DeepStream SDK (DSX)**

### Add NVIDIA’s GPG Key:
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub
sudo apt-key add 7fa2af80.pub
```

### Add the DeepStream Repository:
```bash
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu2004/x86_64 /"
```

### Install DeepStream SDK:
```bash
sudo apt update
sudo apt install deepstream-6.2
```
> Replace `6.2` with your desired version.

### Verify Installation:
```bash
deepstream-app --version
```

---

## **3. Install Bluetooth Support Libraries**

### Install `pybluez` for Python-based Bluetooth Interaction:
```bash
pip3 install pybluez
```

### Install Additional Bluetooth Utilities:
```bash
sudo apt install bluez-tools
```

---

## **4. Set Up Low Energy Bluetooth (BLE) Communication**

### Use `bluetoothctl` to Test:
Start the Bluetooth manager:
```bash
bluetoothctl
```

Commands within `bluetoothctl`:
- **Scan for Devices:** `scan on`
- **Pair with a Device:** `pair <device_mac_address>`
- **Connect to a Device:** `connect <device_mac_address>`
- **List Paired Devices:** `paired-devices`

### Write and Read Data Using Python:
Example Python script using `bleak`:
```python
from bleak import BleakClient
import asyncio

address = "XX:XX:XX:XX:XX:XX"  # Replace with your device's MAC address

async def connect_and_read():
    async with BleakClient(address) as client:
        print(f"Connected: {client.is_connected}")
        value = await client.read_gatt_char("your-characteristic-uuid")
        print(f"Value: {value}")

asyncio.run(connect_and_read())
```

Install `bleak` via:
```bash
pip3 install bleak
```

---

## **5. Integrate Bluetooth Communication into DeepStream**

### Use Python Bindings with DeepStream SDK:
1. Install `pyds` (Python bindings for DSX).
2. Extend your DeepStream pipeline to integrate BLE communication.
3. Incorporate the data handling logic from libraries like `bleak`.

Example pipeline steps:
- **Capture video streams** using DeepStream.
- **Process the stream** and extract required data.
- **Transmit data over BLE** using Python scripts.

---

## **6. Testing and Debugging**

### Use `hcitool` and `gatttool` for BLE Testing:
```bash
sudo hcitool lescan  # Scan for BLE devices
gatttool -b <device_mac_address> --interactive  # Interact with the device
```

### Debugging Bluetooth:
Check the Bluetooth service status:
```bash
sudo systemctl status bluetooth
```

Check DeepStream logs for errors.

---

## **7. Optional: Create a User-Friendly Interface**

### Use Frameworks for UI:
- **GUI:** Use PyQt or PySide.
- **CLI:** Use Argparse or Click for command-line tools.

---

By following these steps, you will have a working setup for BLE communication integrated with the NVIDIA DeepStream SDK on Ubuntu.

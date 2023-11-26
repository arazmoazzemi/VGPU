### GPU-over-IP
### 👉 80% of GPU capacity is unused. Juice’s GPU-over-IP software puts all of it into play 👈
- ***https://www.juicelabs.co/***
- ***https://github.com/Juice-Labs/Juice-Labs***
- ***https://github.com/Juice-Labs/Juice-Labs/wiki***

----

### Juice Server (Windows) prerequisites:
- NVIDIA driver 528.49 or later
- [Microsoft Visual C++ runtime](https://aka.ms/vs/17/release/vc_redist.x64.exe)
- Allow access to TCP port 43210

- [latest Juice server for Windows](https://github.com/Juice-Labs/Juice-Labs/releases/latest/download/JuiceServer-windows.zip)

Run the server(Windows host):
```cmd
agent.exe
```

Juice Client (Windows):

- [Download and install the Microsoft Visual C++ runtime](https://aka.ms/vs/17/release/vc_redist.x64.exe)
- [Download the latest Juice client for Windows](https://github.com/Juice-Labs/Juice-Labs/releases/latest/download/JuiceClient-windows.zip)
- Edit juice.cfg and set servers to the IP address or hostname of the Juice server to connect to

### - For example:
### Juice for Graphics:  
```cmd
C:\juice\juicify.exe "C:\juice\vkcube.exe"
```

### Juice for Compute:
```cmd
C:\juice\juicify.exe "C:\Program Files (x86)\Steam\steam.exe"
```

----
----

### Juice Server (Linux):

- NVIDIA driver 530.30 or later
- CUDA toolkit 11.8
- Allow access to TCP port 43210

```bash
sudo apt update
sudo apt install libvulkan1 libgl1 libglib2.0-0

# Install CUDA toolkit 11.8
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run --silent --toolkit

# Install NVIDIA driver 530
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt install nvidia-driver-515
sudo reboot

# Open TCP port 43210 (optional)
sudo ufw allow 43210

```

### nstall the latest Juice server for Linux:

```bash
wget https://github.com/Juice-Labs/Juice-Labs/releases/latest/download/JuiceServer-linux.tar.gz
mkdir JuiceServer
cd JuiceServer
tar -xf ../JuiceServer-linux.tar.gz

```

### Run the server:
```bash
agent 
```


















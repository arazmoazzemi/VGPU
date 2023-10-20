### GPU-over-IP
### 👉 80% of GPU capacity is unused. Juice’s GPU-over-IP software puts all of it into play 👈
https://www.juicelabs.co/
https://github.com/Juice-Labs/Juice-Labs
https://github.com/Juice-Labs/Juice-Labs/wiki

----

### Juice Server (Windows) prerequisites:
- NVIDIA driver 528.49 or later
- [Microsoft Visual C++ runtime](https://download.visualstudio.microsoft.com/download/pr/eaab1f82-787d-4fd7-8c73-f782341a0c63/917C37D816488545B70AFFD77D6E486E4DD27E2ECE63F6BBAAF486B178B2B888/VC_redist.x64.exe)
- Allow access to TCP port 43210

- [latest Juice server for Windows](https://objects.githubusercontent.com/github-production-release-asset-2e65be/561548956/cc75f8bb-4872-43df-be42-96befe3d4aeb?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231020%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231020T050803Z&X-Amz-Expires=300&X-Amz-Signature=45dd6ea3a860fad434a4d4e9ab9b746ea3fcf8969e65e7410dc39cc6969d9b8a&X-Amz-SignedHeaders=host&actor_id=67405217&key_id=0&repo_id=561548956&response-content-disposition=attachment%3B%20filename%3DJuiceServer-windows.zip&response-content-type=application%2Foctet-stream)

Run the server:
```cmd
agent.exe
```

Juice Client (Windows):

- 

----
****
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


















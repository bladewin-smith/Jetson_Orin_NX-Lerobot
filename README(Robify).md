sudo install -d -m 0755 /etc/apt/keyrings


sudo apt install wget


wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null

echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | sudo tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null

echo ' Package: * Pin: origin packages.mozilla.org Pin-Priority: 1000' | sudo tee /etc/apt/preferences.d/mozilla

sudo vim mozilla

sudo apt update

sudo apt-get install firefox


sudo dpkg -i code_1.108.1-1768404288_arm64.deb 

sudo apt install libwebkit2gtk-4.1-0

sudo apt --fix-broken install

sudo apt install python3-pip

sudo -H pip3 install -U jetson-stats

sudo dpkg -i Clash.Verge_2.4.3_arm64.deb

https://developer.nvidia.com/cuda-12-6-0-download-archive?target_os=Linux&target_arch=aarch64-jetson&Compilation=Native&Distribution=Ubuntu&target_version=22.04&target_type=deb_local


wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/arm64/cuda-ubuntu2204.pin

sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600

wget https://developer.download.nvidia.com/compute/cuda/12.6.0/local_installers/cuda-tegra-repo-ubuntu2204-12-6-local_12.6.0-1_arm64.deb

sudo dpkg -i cuda-tegra-repo-ubuntu2204-12-6-local_12.6.0-1_arm64.deb

sudo cp /var/cuda-tegra-repo-ubuntu2204-12-6-local/cuda-*-keyring.gpg /usr/share/keyrings/

sudo apt-get update

sudo apt-get -y install cuda-toolkit-12-6 cuda-compat-12-6

https://developer.nvidia.com/cusparselt-downloads?target_os=Linux&target_arch=aarch64-jetson&Compilation=Native&Distribution=Ubuntu&target_version=22.04&target_type=deb_local

wget https://developer.download.nvidia.com/compute/cusparselt/0.8.1/local_installers/cusparselt-local-tegra-repo-ubuntu2204-0.8.1_0.8.1-1_arm64.deb

sudo dpkg -i cusparselt-local-tegra-repo-ubuntu2204-0.8.1_0.8.1-1_arm64.deb

sudo cp /var/cusparselt-local-tegra-repo-ubuntu2204-0.8.1/cusparselt-*-keyring.gpg /usr/share/keyrings/

sudo apt-get update

sudo apt-get -y install cusparselt-cuda-12


wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh
chmod +x Miniconda3-latest-Linux-aarch64.sh
./Miniconda3-latest-Linux-aarch64.sh
source ~/.bashrc


conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
conda create -y -n lerobot python=3.10 && conda activate lerobot

conda install nvidia::cudnn cuda-version=12

sudo vim ~/.bashrc
 export PATH=/usr/local/cuda/bin:$PATH
 export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
 export LD_LIBRARY_PATH=~/miniconda3/envs/lerobot/lib:$LD_LIBRARY_PATH
 source ~/.bashrc

https://github.com/Seeed-Projects/reComputer-Jetson-for-Beginners/tree/main/3-Basic-Tools-and-Getting-Started/3.5-Pytorch

sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libomp-dev

pip3 install 'Cython<3'

pip3 install numpy

sudo pip3 install torch-2.7.0-cp310-cp310-linux_aarch64.whl torchvision-0.22.0-cp310-cp310-linux_aarch64.whl

conda install ffmpeg -c conda-forge

cd ~/lerobot && pip install -e ".[feetech]"

conda install -y -c conda-forge "opencv>=4.10.0.84" 
conda remove opencv
pip3 install opencv-python==4.10.0.84 
conda install -y -c conda-forge ffmpeg
conda uninstall numpy
pip3 install numpy==1.26.0


lerobot-calibrate --robot.type=so100_follower --robot.port=/dev/ttyACM0 --robot.id=so100_follower

lerobot-calibrate --teleop.type=so100_leader --teleop.port=/dev/ttyACM0 --teleop.id=so100_leader


lerobot-teleoperate --robot.type=so100_follower --robot.port=/dev/ttyACM0 --robot.id=so100_follower --teleop.type=so100_leader --teleop.port=/dev/ttyACM0 --teleop.id=so100_follower --display_data=true --robot.cameras="{ front: {type: opencv, index_or_path: /dev/video0, width: 640, height: 480, fps: 30}, wrist: {type: opencv, index_or_path: /dev/video2, width: 640, height: 480, fps: 30}}"

lerobot-record --robot.type=so100_follower --robot.port=/dev/ttyACM0 --robot.cameras="{ front: {type: opencv, index_or_path: /dev/video0, width: 640, height: 480, fps: 30}, wrist: {type: opencv, index_or_path: /dev/video2, width: 640, height: 480, fps: 30}}"  --robot.id=so100_follower --teleop.type=so100_leader --teleop.port=/dev/ttyACM0 --teleop.id=so100_leader --dataset.push_to_hub=false --dataset.num_episodes=50 --dataset.root="/home/jetson/lerobot/robify/data1" --dataset.episode_time_s=300 --dataset.reset_time_s=30 --display_data=true --dataset.repo_id=${HF_USER}/record_data1 --dataset.single_task="Sort out things"


lerobot-record --robot.type=so100_follower --robot.port=/dev/ttyACM0 --robot.cameras="{ front: {type: opencv, index_or_path: /dev/video0, width: 640, height: 480, fps: 30}, wrist: {type: opencv, index_or_path: /dev/video2, width: 640, height: 480, fps: 30}}"  --robot.id=so100_follower --display_data=true --dataset.repo_id=${HF_USER}/eval_so100_test --dataset.single_task="Sort out things" --dataset.episode_time_s=3000 --dataset.num_episodes=10 --policy.path=/home/jetson/lerobot/pretrained_model1


sudo rm -rf /home/jetson/.cache/huggingface/lerobot/vkrobot/eval_so100_test


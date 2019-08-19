# AWS Setup

## Create from neural-style image

```bash
docker-machine create --driver amazonec2 --amazonec2-instance-type p2.xlarge --amazonec2-ami ami-00f58efbe2355e30c --amazonec2-root-size 50 neural-style
```

**NOTE:** if the AMI used above doesn't exist you can create a new one with the steps outlined in the next section.

## Create from deep learning base image

```bash
# Create instance
docker-machine create --driver amazonec2 --amazonec2-instance-type p2.xlarge --amazonec2-ami ami-0aaa3a0f6876b748c --amazonec2-root-size 50 deep-learn-base

# Enter instance
docker-machine ssh deep-learn-base

##########################################
# You might be able to skip all this
# Try starting with un-commented steps
##########################################

# # Install Lua
# # https://github.com/luarocks/luarocks/wiki/installation-instructions-for-unix
# sudo apt install build-essential libreadline-dev
# cd ~/
# curl -R -O http://www.lua.org/ftp/lua-5.3.4.tar.gz
# tar -zxf lua-5.3.4.tar.gz
# cd lua-5.3.4
# make linux test
# sudo make install
# # Verify that `lua` works

# cd ~/

# # Install LuaRocks
# # https://github.com/luarocks/luarocks/wiki/installation-instructions-for-unix
# wget https://luarocks.org/releases/luarocks-3.1.3.tar.gz
# tar zxpf luarocks-3.1.3.tar.gz
# cd luarocks-3.1.3
# ./configure
# make build
# sudo make install

# cd ~/

# # Install Torch (Doesn't work with CUDA >= v10)
# # http://torch.ch/docs/getting-started.html#_
# git clone https://github.com/torch/distro.git ~/torch --recursive
# cd ~/torch; bash install-deps;
# ./install.sh
# source ~/.bashrc
# # Verify that `th` works

# cd ~/


# Install a version of Torch that works with CUDA 10
git clone https://github.com/nagadomi/distro.git
cd distro
bash install-deps
./install.sh
source ~/.bashrc
cd ~/

# Install loadcaffe
# https://github.com/szagoruyko/loadcaffe
sudo apt-get install libprotobuf-dev protobuf-compiler
luarocks install loadcaffe

# Install neural-style
git clone https://github.com/domtriola/neural-style.git
cd neural-style
sh models/download_models.sh
```

## Try it out

```bash
th neural_style.lua -style_image examples/inputs/picasso_selfport1907.jpg -content_image examples/inputs/brad_pitt.jpg

# Upload images
docker-machine scp -r ./<local-image-path> <instance-name>:/home/ubuntu/neural-style/input/
# Retrieve images
docker-machine scp -r <instance-name>:/home/ubuntu/neural-style/results/ results/
```

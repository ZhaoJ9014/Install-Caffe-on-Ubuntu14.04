# Install-Caffe-on-Ubuntu14.04

The python version is the default version 2.7

1. Install Nvidia driver
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-378 nvidia-settings nvidia-prime
sudo reboot
prime-select query
cat /proc/driver/nvidia/version

2. Install Cuda8.0
download Cuda8.0 from https://developer.nvidia.com/cuda-downloads
sudo dpkg -i cuda-repo-ubuntu1404-8-0-local-ga2_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
export PATH=/usr/local/cuda-8.0/bin:$PATH    
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH 
sudo ln -sf /usr/local/cuda-8.0/lib64 /usr/local/lib
sudo apt-get install nvidia-378

3. Install Cudnn5.1
downnload Cudnn5.1 from https://developer.nvidia.com/rdp/cudnn-download
cd /path/to/cuda/include
sudo cp cudnn.h /usr/local/include
cd /path/to/cuda/lib64
sudo cp libcudnn.so.5.1.10 /usr/local/lib
sudo ln -s /usr/local/lib/libcudnn.so.5.1.10 /usr/local/lib/libcudnn.so.5.1
sudo ln -s /usr/local/lib/libcudnn.so.5.1 /usr/local/lib/libcudnn.so
sudo ldconfig
cd /usr/local/lib
sudo chmod 755 libcudnn.so.5.1.10

4. Install Anaconda2
download https://anaconda.org/
./Anaconda-2.3.0-Linux-x86_64.sh
export PATH=~/anaconda2/bin:$PATH
conda update conda
conda install accelerate
conda install iopro
mv LICENSE.txt ~/.continuum
conda update ipython
conda update ipython-notebook
conda update ipython-qtconsole

5. Install Opencv2.4.11
download https://github.com/Itseez/opencv/archive/2.4.11.zip 
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
cd ~/opencv-2.4.11
mkdir release 
cd release
$ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
make
sudo make install
cd ..
touch opencv.sh
input the following contents:
echo "compiling $1"
if [[ $1 == *.c ]]
then
    gcc -g `pkg-config --cflags opencv`  -o `dirname $1`/`basename $1 .c` $1 `pkg-config --libs opencv`;
elif [[ $1 == *.cpp ]]
then
    g++ -g `pkg-config --cflags opencv` -std=c++11 -std=gnu++11 -o `dirname $1`/`basename $1 .cpp` $1 `pkg-config --libs opencv`;
else
    echo "Please compile only .c or .cpp files"
fi
echo "Output file => ${1%.*}"

sudo chmod 777 opencv.sh
vim ~/.bashrc
input the following contents:
alias opencv="~/opencv-2.4.11/opencv.sh"

source ~/.bashrc

5. Install Matlab R2015b
download Matlab R2015b
sudo mkdir /media/matlab
sudo mount -o loop R2015b_glnxa64.iso /media/matlab
cd /media/matlab
sudo ./matlab
input the activation key in Crack/readme.txt
import the activation file in Crack/*.lic
sudo unmount /media/matlab

6. Install MKL
download MKL from https://registrationcenter.intel.com/en/products/postregistration/?sn=33RM-2LTPC8PF&EmailID=zhaojian9014%40gmail.com&Sequence=1864378&dnld=r
install
sudo vim /etc/ld.so.conf.d/intel_mkl.conf
input the following contents:
/opt/intel/lib/intel64
/opt/intel/mkl/lib/intel64

sudo ldconfig

7. Install OpenBlas
download OpenBlas from http://www.openblas.net/
make -j 8
make install PREFIX=OpenBlas_directory
export LD_LIBRARY_PATH=OpenBlas_directory/lib

8. Install Atlas
sudo apt-get install libatlas-base-dev

9. Install Boost
sudo apt-get install libboost-all-dev

10. Install other dependencies
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

11. Custom your Caffe makefile.config

12. Compile Caffe
make all -j 8
make test -j 8
make runtest -j 8
make pycaffe -j 8
make matcaffe -j 8

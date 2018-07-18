## Build Caffe with CUDA 9.2 on Ubuntu 18.04

### Step1. Build opencv

Refer to [here](https://github.com/goodgodgd/sch-cvss-2018/blob/master/Week1/README.md)

### Step2. Install CUDA and cuDNN

Refer to [here](https://github.com/goodgodgd/sch-cvss-2018/blob/master/build_process/build_tensorflow.md)

### Step3. Install python

If you did something like this,
```bash
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 100 --slave /usr/bin/pip pip /usr/bin/pip3
```
Undo it like this
```bash
sudo update-alternatives --remove python /usr/bin/python3
sudo update-alternatives --remove pip /usr/bin/pip3
```
Then reinstall python2
```bash
sudo apt remove python-dev python2.7-dev python-pip
sudo apt install python-dev python-pip python-numpy
sudo ln -s sudo ln -s /usr/bin/python2 /usr/bin/python
```

### Step3. Install additional dependencies

```bash
sudo apt install libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler
sudo apt install --no-install-recommends libboost-all-dev
sudo apt install libgflags-dev libgoogle-glog-dev liblmdb-dev
```

### Step4. Configure, build and install

Set following variables in cmake-gui 
```
CMAKE_BUILD_TYPE=Release
CMAKE_ISNTALL_PREFIX=$HOME/mylib/deploy/caffe
OpenCV_DIR=$HOME/mylib/deploy/opencv-3.4.1/share/OpenCV
Boost_PYTHON_LIBRARY_DEBUG=/usr/lib/x86_64-linux-gnu/libboost_python-py27.so.1.65.1
Boost_PYTHON_LIBRARY_RELEASE=/usr/lib/x86_64-linux-gnu/libboost_python-py27.so.1.65.1
```  
configure - generate - make - make install

### Step5. Install pycaffe

1. Environment variable setup
    Now caffe library is installed in $HOME/mylib/deploy/caffe
    Add following lines at the end of `.bashrc`
    ```
    export LD_LIBRARY_PATH=$HOME/mylib/deploy/caffe-dvof/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export PYTHONPATH=$HOME/mylib/deploy/caffe-dvof/python${PYTHONPATH:+:${PYTHONPATH}}
    ```
2. Create virutal env for python
    To manage python packages, install pipenv and create a new virtual env in the project folder.
    ```bash
    cd $PROJECT_DIR
    pip install pipenv
    pipenv --python=python2
    ```
3. Install dependencies
    Then activate virtual env and install dependent packages
    ```bash
    pipenv shell
    pipenv install scikit-image lmdb protobuf
    exit
    ```  
4. Check installation
    ```bash
    python
    import caffe
    ```
    If no error occurs, installation was successful.  
    Otherwise, install additional dependencies according to error messages.

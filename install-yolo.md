# Install OpenCV

```bash
# Clone
git clone https://github.com/opencv/opencv.git
cd opencv
git checkout 3.4.9

# Create build directory
mkdir build-3.4.9
cd build-3.4.9

# Configure
cmake ../

# Build
make -j7

# Install
sudo make install
```

# Download darknet

```bash
git clone https://github.com/pjreddie/darknet
```

# Download coco labels

```bash
wget https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names
```

# Download weights

```bash
wget https://pjreddie.com/media/files/yolov3.weights
```

# Create project

```bash
mkdir yolo-project
cd yolo-project
mkdir yolo-coco

cp <coco-labels> yolo-coco/coco.names
cp <yolov3-weights> yolo-coco/yolov3.weights
cp <darknet-git/cfg/yolov3.cfg> yolo-coco/yolov3.cfg
```
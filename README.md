# ZeroMQ by Jonas Goos
Because OpenFace is a C++ library and we use Python as a backend we need to find a way to communicate between them. There is a library called ZeroMQ which can do that. It also supports a bunch of other languages. 

An example where a python server sends images to OpenFace and OpenFace sends back the extracted action units can be found here: https://github.com/BorderBoy/zmq-example/.

To build it you first need to make sure you can build OpenFace. Then you need to install cppzmq. Follow the instructions here: https://github.com/zeromq/cppzmq#build-instructions
Then follow the instructions in this readme.
Also make sure you have pyzmq installed: pip install pyzmq
This only works on macOS and Linux.

Achieved speed on low end linux machine: 16 fps with image size 600x400.


# zmq-openface
An adjusted working example of communication between the python backend and c++ openface. The python client reads images from files and encodes them into base64. These are then sent to OpenFace. OpenFace extracts the action units (AU) and sends them back to the python client.

##  Python
The python code can be found in `py/`. When the python client is executed it sends the first image, waits for the reply and sends the next one. This is done with all 6 images. Start the OpenFace extractor first, wait until the models are loaded and then start the python client.

## OpenFace
This has been tested for macOS and Ubuntu 22.04.

The code can be found in `open-face-extractor/`. To build the code the contents of `open-face-extractor/` must be placed into the `exe/OwnExtractor` folder of [OpenFace](https://github.com/TadasBaltrusaitis/OpenFace). Additionally this line must be added to `OpenFace/CMakeLists.txt`:  
```add_subdirectory(exe/OwnExtractor)```

To build OpenFace follow the instructions in the [wiki](https://github.com/TadasBaltrusaitis/OpenFace/wiki).

The binary takes one command line argument, which is parsed to the port on which it is going to listen on. When the binary is executed it waits for an image and replies. Then it waits for another image.

**Used Libraries:**
- [cppzmq](https://github.com/zeromq/cppzmq)
- [pyzmq](https://github.com/zeromq/pyzmq)

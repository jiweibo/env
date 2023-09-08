# env
Software Development Environment

## cc

### abseil
```shell
# https://abseil.io/docs/cpp/quickstart-cmake.html#getting-the-abseil-code

git clone --depth 1 --branch origin/lts_2023_08_02 https://github.com/abseil/abseil-cpp.git
cd abseil-cpp
mkdir build && cd build
cmake -DABSL_BUILD_TESTING=OFF -DABSL_USE_GOOGLETEST_HEAD=ON -DCMAKE_CXX_STANDARD=14 -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=$PWD/install ..
cmake --build . --target all
make install
export absl_DIR=$PWD/install/lib/cmake/absl
echo $PWD/install/lib | tee /etc/ld.so.conf.d/absl.conf
ldconfig

# This is necessary for linking in Abseil.
# set(CMAKE_POSITION_INDEPENDENT_CODE ON)
```

### protobuf
```shell
git clone --depth 1 --branch v24.2 https://github.com/protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
mkdir build && cd build
cmake -Dprotobuf_ABSL_PROVIDER=package -Dprotobuf_BUILD_SHARED_LIBS=ON -Dprotobuf_BUILD_TESTS=OFF -Dabsl_DIR=$absl_DIR ..
make -j8
make install
export Protobuf_DIR=$PWD/install/lib/cmake/protobuf
echo $PWD/install/lib | tee /etc/ld.so.conf.d/protobuf.conf
ldconfig
```

### grpc
```
git clone --depth 1 -b v1.58.0 https://github.com/grpc/grpc.git
cd grpc
git submodule update --init
mkdir build && cd build
cmake -DgRPC_INSTALL=ON -DCMAKE_BUILD_TYPE=Release -DgRPC_ABSL_PROVIDER=package -Dabsl_DIR=$absl_DIR -DgRPC_PROTOBUF_PROVIDER=package -DProtobuf_DIR=$Protobuf_DIR -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=$PWD/install ..
make -j8
make install
export gRPC_DIR=$PWD/install/lib/cmake/grpc
echo $PWD/install/lib | tee /etc/ld.so.conf.d/grpc.conf
ldconfig
```

cmake_minimum_required(VERSION 3.22.1)
project(xrdesk VERSION 0.1.0)

if(ANDROID)

endif()

subdirs(thirdparty app/src/main/cpp)

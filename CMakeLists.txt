cmake_minimum_required(VERSION 3.15)
project(mase)

set(CMAKE_CXX_STANDARD 14)
set(BOOST_ROOT "~/dvlpmnt/env/booster")
set(BOOSTROOT "~/dvlpmnt/env/booster")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

find_package(Boost REQUIRED COMPONENTS filesystem system random context)

IF (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIR})
    add_executable(mase src/app.cpp
            src/core/app.h
            src/core/Base64.h
            src/core/Base64.cpp
            src/core/SSL_cert.h
            src/core/framework/Config.cpp
            src/core/framework/Config.h
            src/core/framework/Database.cpp
            src/core/framework/Database.h
            src/core/framework/Framework.cpp
            src/core/framework/Framework.h
            src/core/framework/GameClient.cpp
            src/core/framework/GameClient.h
            src/core/framework/GameServer.cpp
            src/core/framework/GameServer.h
            src/core/framework/Logger.cpp
            src/core/framework/Logger.h
            src/core/framework/Stats.cpp
            src/core/framework/Stats.h
            src/core/network/http/HttpConnection.cpp
            src/core/network/http/HttpConnection.h
            src/core/network/http/Reply.cpp
            src/core/network/http/Reply.h
            src/core/network/http/RequestHandler.cpp
            src/core/network/http/RequestHandler.h
            src/core/network/http/RequestParser.cpp
            src/core/network/http/RequestParser.h
            src/core/network/HttpServer.cpp
            src/core/network/HttpServer.h
            src/core/network/Packet.cpp
            src/core/network/Packet.h
            src/core/network/TcpConnection.cpp
            src/core/network/TcpConnectionSSL.h
            src/core/network/TcpConnection.h
            src/core/network/TcpConnectionSSL.cpp
            src/core/network/TcpServer.h
            src/core/network/TcpServer.cpp
            src/core/network/UdpServer.cpp
            src/core/network/UdpServer.h)
    target_link_libraries(mase ${Boost_LIBRARIES})
endif()

set(OPENSSL_ROOT_DIR "~/dvlpmnt/env/ssl")

find_package(OpenSSL REQUIRED COMPONENTS Crypto SSL)
IF (OpenSSL_FOUND)
    message(${OPENSSL_ROOT_DIR})
    message(${OPENSSL_INCLUDE_DIR})
    message(${OPENSSL_CRYPTO_LIBRARY})
    message(${OPENSSL_SSL_LIBRARY})
    include_directories(${OPENSSL_INCLUDE_DIR})
    target_link_libraries(mase ${OPENSSL_CRYPTO_LIBRARY})
    target_link_libraries(mase ${OPENSSL_SSL_LIBRARY})
endif()
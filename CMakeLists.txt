cmake_minimum_required(VERSION 3.14)

project(lemon VERSION 1.3 LANGUAGES NONE)

add_library(lemon INTERFACE)
target_include_directories(lemon INTERFACE "\$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>")

target_compile_features(lemon INTERFACE cpp_std_17)

if(NOT CMAKE_SKIP_INSTALL_RULES)
  set(CMAKE_INSTALL_DEFAULT_COMPONENT_NAME lemon_Development)

  # Avoid using a common include directory
  set(CMAKE_INSTALL_INCLUDEDIR "include/lemon-${PROJECT_VERSION}" CACHE PATH "")

  # Needed by GNUInstallDirs, because LANGUAGES is NONE
  set(CMAKE_INSTALL_LIBDIR "" CACHE PATH "")

  include(CMakePackageConfigHelpers)
  include(GNUInstallDirs)

  install(DIRECTORY include/ DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")
  install(TARGETS lemon EXPORT lemonTargets INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")

  write_basic_package_version_file(lemonConfigVersion.cmake COMPATIBILITY SameMajorVersion ARCH_INDEPENDENT)
  set(lemon_INSTALL_CMAKEDIR "${CMAKE_INSTALL_DATADIR}/lemon" CACHE PATH "")
  mark_as_advanced(lemon_INSTALL_CMAKEDIR)

  install(FILES "${PROJECT_BINARY_DIR}/lemonConfigVersion.cmake" DESTINATION "${lemon_INSTALL_CMAKEDIR}")
  install(EXPORT lemonTargets NAMESPACE lemon:: DESTINATION "${lemon_INSTALL_CMAKEDIR}" FILE lemonConfig.cmake)

  include(CPack)
endif()

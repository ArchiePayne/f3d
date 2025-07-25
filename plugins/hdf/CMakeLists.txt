cmake_minimum_required(VERSION 3.21)

project(f3d-plugin-hdf)

include(GNUInstallDirs)

# Check if the plugin is built externally
if(PROJECT_IS_TOP_LEVEL)
  find_package(f3d REQUIRED COMPONENTS pluginsdk)
else()
  include(f3dPlugin)
endif()

f3d_plugin_init()

f3d_plugin_declare_reader(
  NAME VTKHDF
  SCORE 99
  EXTENSIONS vtkhdf
  MIMETYPES application/vnd.vtkhdf
  VTK_READER vtkHDFReader
  FORMAT_DESCRIPTION "VTKHDF"
)

f3d_plugin_declare_reader(
  NAME ExodusII
  EXTENSIONS exo ex2 e g
  MIMETYPES application/vnd.exodus
  VTK_READER vtkExodusIIReader
  FORMAT_DESCRIPTION "Exodus II"
  CUSTOM_CODE "${CMAKE_CURRENT_SOURCE_DIR}/exodus.inl"
)

f3d_plugin_declare_reader(
  NAME NetCDF
  EXTENSIONS nc cdf ncdf
  MIMETYPES application/netcdf application/x-netcdf
  VTK_READER vtkNetCDFReader
  FORMAT_DESCRIPTION "NetCDF"
  CUSTOM_CODE "${CMAKE_CURRENT_SOURCE_DIR}/netcdf.inl"
)

f3d_plugin_build(
  NAME hdf
  VERSION 1.0
  DESCRIPTION "VTK HDF formats support"
  VTK_MODULES IOHDF IOExodus IONetCDF
  MIMETYPE_XML_FILES "${CMAKE_CURRENT_SOURCE_DIR}/f3d-hdf-formats.xml"
  CONFIGURATION_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/configs/config.d" "${CMAKE_CURRENT_SOURCE_DIR}/configs/thumbnail.d"
  FREEDESKTOP
)

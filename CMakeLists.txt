cmake_minimum_required(VERSION 3.5...4.2.1 FATAL_ERROR)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)

project(scintilla VERSION 5.0.0 LANGUAGES CXX)

set(HEADER_FILES
	include/Compat.h
	include/ILexer.h
	include/ILoader.h
	include/Platform.h
	include/Sci_Position.h
	include/Scintilla.h
	include/ScintillaWidget.h
	src/AutoComplete.h
	src/CallTip.h
	src/CaseConvert.h
	src/CaseFolder.h
	src/CellBuffer.h
	src/CharacterCategory.h
	src/CharacterSet.h
	src/CharClassify.h
	src/ContractionState.h
	src/DBCS.h
	src/Decoration.h
	src/Document.h
	src/EditModel.h
	src/Editor.h
	src/EditView.h
	src/ElapsedPeriod.h
	src/FontQuality.h
	src/Indicator.h
	src/IntegerRectangle.h
	src/KeyMap.h
	src/LineMarker.h
	src/MarginView.h
	src/Partitioning.h
	src/PerLine.h
	src/Position.h
	src/PositionCache.h
	src/RESearch.h
	src/RunStyles.h
	src/ScintillaBase.h
	src/Selection.h
	src/SparseVector.h
	src/SplitVector.h
	src/Style.h
	src/UniConversion.h
	src/UniqueString.h
	src/ViewStyle.h
	src/XPM.h
)

set(SOURCE_FILES
	src/AutoComplete.cxx
	src/CallTip.cxx
	src/CaseConvert.cxx
	src/CaseFolder.cxx
	src/CellBuffer.cxx
	src/CharacterCategory.cxx
	src/CharacterSet.cxx
	src/CharClassify.cxx
	src/ContractionState.cxx
	src/DBCS.cxx
	src/Decoration.cxx
	src/Document.cxx
	src/EditModel.cxx
	src/Editor.cxx
	src/EditView.cxx
	src/Indicator.cxx
	src/KeyMap.cxx
	src/LineMarker.cxx
	src/MarginView.cxx
	src/PerLine.cxx
	src/PositionCache.cxx
	src/RESearch.cxx
	src/RunStyles.cxx
	src/ScintillaBase.cxx
	src/Selection.cxx
	src/Style.cxx
	src/UniConversion.cxx
	src/UniqueString.cxx
	src/ViewStyle.cxx
	src/XPM.cxx
)

add_library(${PROJECT_NAME} STATIC ${HEADER_FILES} ${SOURCE_FILES})

target_include_directories(${PROJECT_NAME}
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
		$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

include(GNUInstallDirs)

set(CONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")
set(GENERATED_DIR "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(VERSION_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
set(PROJECT_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}Config.cmake")
set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
set(NAMESPACE "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
	"${VERSION_CONFIG}" COMPATIBILITY SameMajorVersion
)

configure_package_config_file(
	"cmake/Config.cmake.in"
	"${PROJECT_CONFIG}"
	INSTALL_DESTINATION "${CONFIG_INSTALL_DIR}"
)

install(
	TARGETS ${PROJECT_NAME}
	EXPORT "${TARGETS_EXPORT_NAME}"
	LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
	ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
	RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
	INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
)

install(
	FILES ${HEADER_FILES}
	DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
)

install(
	FILES "${PROJECT_CONFIG}" "${VERSION_CONFIG}"
	DESTINATION "${CONFIG_INSTALL_DIR}"
)

install(
	EXPORT "${TARGETS_EXPORT_NAME}"
	NAMESPACE "${NAMESPACE}"
	DESTINATION "${CONFIG_INSTALL_DIR}"
)

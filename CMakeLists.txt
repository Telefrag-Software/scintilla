cmake_minimum_required(VERSION 3.5...4.2.1 FATAL_ERROR)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)

project(scintilla VERSION 5.5.8 LANGUAGES CXX)

set(SOURCE_DIRECTORY src)
set(INCLUDE_DIRECTORY include)

set(HEADER_FILES
	ILexer.h
	ILoader.h
	Sci_Position.h
	Scintilla.h
	ScintillaCall.h
	ScintillaMessages.h
	ScintillaStructures.h
	ScintillaTypes.h
	ScintillaWidget.h
)

set(SOURCE_FILES
	AutoComplete.cxx
	AutoComplete.h
	CallTip.cxx
	CallTip.h
	CaseConvert.cxx
	CaseConvert.h
	CaseFolder.cxx
	CaseFolder.h
	CellBuffer.cxx
	CellBuffer.h
	ChangeHistory.cxx
	ChangeHistory.h
	CharacterCategoryMap.cxx
	CharacterCategoryMap.h
	CharacterType.cxx
	CharacterType.h
	CharClassify.cxx
	CharClassify.h
	ContractionState.cxx
	ContractionState.h
	DBCS.cxx
	DBCS.h
	Debugging.h
	Decoration.cxx
	Decoration.h
	Document.cxx
	Document.h
	EditModel.cxx
	EditModel.h
	Editor.cxx
	Editor.h
	EditView.cxx
	EditView.h
	ElapsedPeriod.h
	Geometry.cxx
	Geometry.h
	Indicator.cxx
	Indicator.h
	KeyMap.cxx
	KeyMap.h
	LineMarker.cxx
	LineMarker.h
	MarginView.cxx
	MarginView.h
	Partitioning.h
	PerLine.cxx
	PerLine.h
	Platform.h
	Position.h
	PositionCache.cxx
	PositionCache.h
	RESearch.cxx
	RESearch.h
	RunStyles.cxx
	RunStyles.h
	ScintillaBase.cxx
	ScintillaBase.h
	Selection.cxx
	Selection.h
	SparseVector.h
	SplitVector.h
	Style.cxx
	Style.h
	UndoHistory.cxx
	UndoHistory.h
	UniConversion.cxx
	UniConversion.h
	UniqueString.cxx
	UniqueString.h
	ViewStyle.cxx
	ViewStyle.h
	XPM.cxx
	XPM.h
)

list(TRANSFORM HEADER_FILES PREPEND "${INCLUDE_DIRECTORY}/")
list(TRANSFORM SOURCE_FILES PREPEND "${SOURCE_DIRECTORY}/")

add_library(${PROJECT_NAME} STATIC ${HEADER_FILES} ${SOURCE_FILES})

target_include_directories(${PROJECT_NAME}
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/${INCLUDE_DIRECTORY}>
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

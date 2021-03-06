#**************************************************************************
#    Lightspark, a free flash player implementation
#
#    Copyright (C) 2010-2013  Alessandro Pignotti <a.pignotti@sssup.it>
#    Copyright (C) 2010  Giacomo Spigler <g.spigler@sssup.it>
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Lesser General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#   This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Lesser General Public License for more details.
#
#   You should have received a copy of the GNU Lesser General Public License
#   along with this program.  If not, see <http://www.gnu.org/licenses/>.
#**************************************************************************

# liblightspark.so sources
SET(LIBSPARK_SOURCES 
  allclasses.cpp
  asobject.cpp
  compat.cpp
  logger.cpp
  memory_support.cpp
  swf.cpp
  swftypes.cpp
  thread_pool.cpp
  threading.cpp
  timer.cpp
  tiny_string.cpp
  errorconstants.cpp
  backends/audio.cpp
  backends/builtindecoder.cpp
  backends/config.cpp
  backends/decoder.cpp
  backends/extscriptobject.cpp
  backends/geometry.cpp
  backends/graphics.cpp
  backends/image.cpp
  backends/input.cpp
  backends/netutils.cpp
  backends/rendering.cpp
  backends/rendering_context.cpp
  backends/rtmputils.cpp
  backends/security.cpp
  backends/streamcache.cpp
  backends/urlutils.cpp
  backends/xml_support.cpp
  parsing/amf3_generator.cpp
  parsing/config.cpp
  parsing/crossdomainpolicy.cpp
  parsing/flv.cpp
  parsing/streams.cpp
  parsing/tags.cpp
  parsing/tags_stub.cpp
  parsing/textfile.cpp
  scripting/abc.cpp
  scripting/abc_codesynt.cpp
  scripting/abc_fast_interpreter.cpp
  scripting/abc_interpreter.cpp
  scripting/abc_optimizer.cpp
  scripting/abc_opcodes.cpp
  scripting/abctypes.cpp
  scripting/flash/accessibility/flashaccessibility.cpp
  scripting/flash/concurrent/Mutex.cpp
  scripting/flash/concurrent/Condition.cpp
  scripting/flash/crypto/flashcrypto.cpp
  scripting/flash/desktop/flashdesktop.cpp
  scripting/flash/display/BitmapContainer.cpp
  scripting/flash/display/BitmapData.cpp
  scripting/flash/display/DisplayObject.cpp
  scripting/flash/display/IBitmapDrawable.cpp
  scripting/flash/display/flashdisplay.cpp
  scripting/flash/display/TokenContainer.cpp
  scripting/flash/display/Graphics.cpp
  scripting/flash/display/GraphicsBitmapFill.cpp
  scripting/flash/display/GraphicsEndFill.cpp
  scripting/flash/display/GraphicsGradientFill.cpp
  scripting/flash/display/GraphicsPath.cpp
  scripting/flash/display/GraphicsShaderFill.cpp
  scripting/flash/display/GraphicsSolidFill.cpp
  scripting/flash/display/GraphicsStroke.cpp
  scripting/flash/display/GraphicsTrianglePath.cpp
  scripting/flash/display3d/flashdisplay3d.cpp
  scripting/flash/display3d/flashdisplay3dtextures.cpp
  scripting/flash/events/flashevents.cpp
  scripting/flash/external/ExternalInterface.cpp
  scripting/flash/filters/flashfilters.cpp
  scripting/flash/filesystem/flashfilesystem.cpp
  scripting/flash/geom/flashgeom.cpp
  scripting/flash/media/flashmedia.cpp
  scripting/flash/net/flashnet.cpp
  scripting/flash/net/URLRequestHeader.cpp
  scripting/flash/net/URLStream.cpp
  scripting/flash/net/Socket.cpp
  scripting/flash/net/XMLSocket.cpp
  scripting/flash/net/NetStreamInfo.cpp
  scripting/flash/net/NetStreamPlayOptions.cpp
  scripting/flash/net/NetStreamPlayTransitions.cpp
  scripting/flash/printing/flashprinting.cpp
  scripting/flash/errors/flasherrors.cpp
  scripting/flash/sampler/flashsampler.cpp
  scripting/flash/sensors/flashsensors.cpp
  scripting/flash/system/flashsystem.cpp
  scripting/flash/text/flashtext.cpp
  scripting/flash/text/flashtextengine.cpp
  scripting/flash/utils/flashutils.cpp
  scripting/flash/utils/ByteArray.cpp
  scripting/flash/utils/CompressionAlgorithm.cpp
  scripting/flash/utils/Dictionary.cpp
  scripting/flash/utils/Proxy.cpp
  scripting/flash/utils/Timer.cpp
  scripting/flash/utils/IntervalManager.cpp
  scripting/flash/utils/IntervalRunner.cpp
  scripting/flash/ui/Keyboard.cpp
  scripting/flash/ui/Mouse.cpp
  scripting/flash/ui/Multitouch.cpp
  scripting/flash/ui/ContextMenu.cpp
  scripting/flash/ui/ContextMenuItem.cpp
  scripting/flash/ui/ContextMenuBuiltInItems.cpp
  scripting/flash/xml/flashxml.cpp
  scripting/toplevel/Array.cpp
  scripting/toplevel/ASString.cpp
  scripting/toplevel/Boolean.cpp
  scripting/toplevel/Date.cpp
  scripting/toplevel/Error.cpp
  scripting/toplevel/Integer.cpp
  scripting/toplevel/JSON.cpp
  scripting/toplevel/Math.cpp
  scripting/toplevel/Number.cpp
  scripting/toplevel/RegExp.cpp
  scripting/toplevel/UInteger.cpp
  scripting/toplevel/Vector.cpp
  scripting/toplevel/XML.cpp
  scripting/toplevel/XMLList.cpp
  scripting/class.cpp
  scripting/toplevel/toplevel.cpp
  scripting/avmplus/avmplus.cpp
  platforms/engineutils.cpp
  3rdparty/pugixml/src/pugixml.cpp)
IF(MINGW)
  SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/slowpaths_generic.cpp)
ELSEIF(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/slowpaths_generic.cpp)
ELSE()
  IF(ENABLE_SSE2)
    IF(${i386})
      SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/fastpaths_x86.cpp)
      SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/fastpaths_i686.asm)
    ELSEIF(${x86_64})
      SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/fastpaths_x86.cpp)
      SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/fastpaths_amd64.asm)
    ELSE()
      SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/slowpaths_generic.cpp)
    ENDIF(${i386})
  ELSE(ENABLE_SSE2)
    SET(LIBSPARK_SOURCES ${LIBSPARK_SOURCES} platforms/slowpaths_generic.cpp)
  ENDIF(ENABLE_SSE2)
ENDIF(MINGW)

INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/src)
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/src/scripting)

# liblightspark.so target
IF(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  ADD_LIBRARY(spark SHARED ${LIBSPARK_SOURCES})
  IF(NOT WIN32)
    #win32's cmake doesn't like liblightspark and lightspark.exe having the same name
    SET_TARGET_PROPERTIES(spark PROPERTIES OUTPUT_NAME lightspark)
  ENDIF()
  IF(MINGW)
    SET_TARGET_PROPERTIES(spark PROPERTIES LINK_FLAGS "-Wl,--export-all-symbols")
  ELSE()
    SET_TARGET_PROPERTIES(spark PROPERTIES LINK_FLAGS "-Wl,--version-script=${PROJECT_SOURCE_DIR}/src/lightspark.expmap")
  ENDIF()
  SET_TARGET_PROPERTIES(spark PROPERTIES LINK_INTERFACE_LIBRARIES "")
ELSE(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  IF(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    ADD_LIBRARY(spark SHARED ${LIBSPARK_SOURCES})
    SET_TARGET_PROPERTIES(spark PROPERTIES OUTPUT_NAME lightspark)
  ELSE (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    ADD_LIBRARY(spark STATIC ${LIBSPARK_SOURCES})
  ENDIF (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
ENDIF (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")

TARGET_LINK_LIBRARIES(spark ${CAIRO_LIBRARIES} ${ZLIB_LIBRARIES}
	${Boost_LIBRARIES} ${LLVM_LIBS_CORE} ${LLVM_LIBS_JIT} ${LLVM_LDFLAGS}
	${OPTIONAL_LIBRARIES} ${SDL2_LIBRARIES} ${SDL2MIXER_LIBRARIES} ${FREETYPE_LIBRARIES} ${JPEG_LIBRARIES} ${PNG_LIBRARIES}
	${PCRE_LIBRARIES} ${GLIBMM_LIBRARIES} ${GMODULE_LIBRARIES}
	${CMAKE_DL_LIBS} ${EXTRA_LIBS_LIBRARIES}
	${X11_LIBRARIES})
SET_TARGET_PROPERTIES(spark PROPERTIES VERSION "${MAJOR_VERSION}.${MINOR_VERSION}.${PATCH_VERSION}")
SET_TARGET_PROPERTIES(spark PROPERTIES SOVERSION "${MAJOR_VERSION}.${MINOR_VERSION}")

IF(ENABLE_GLES2)
	TARGET_LINK_LIBRARIES(spark ${GLES2_LIBRARIES})
ELSE()
	TARGET_LINK_LIBRARIES(spark ${OPENGL_LIBRARIES} ${GLEW_LIBRARIES})
ENDIF(ENABLE_GLES2)

PACK_LIBRARY(spark)
#We have to use LIBRARY on linux and RUNTIME on win32
INSTALL(TARGETS spark RUNTIME DESTINATION ${PRIVATELIBDIR} LIBRARY DESTINATION ${PRIVATELIBDIR})

# lightspark executable target
IF(COMPILE_LIGHTSPARK)
  ADD_EXECUTABLE(lightspark main.cpp)
  IF(MINGW)
    SET_TARGET_PROPERTIES(lightspark PROPERTIES LINK_FLAGS "-Wl,--whole-archive -lSDL2main -Wl,--no-whole-archive")
  ENDIF()
  TARGET_LINK_LIBRARIES(lightspark ${Boost_LIBRARIES} spark)
  #With STATICDEPS, all deps are compiled into spark
  IF(NOT STATICDEPS)
    TARGET_LINK_LIBRARIES(lightspark ${X11_LIBRARIES} ${SDL2_LIBRARIES} ${SDL2MIXER_LIBRARIES} ${GLIBMM_LIBRARIES} ${GTHREAD_LIBRARIES})
  ENDIF()

  PACK_EXECUTABLE(lightspark)
  INSTALL(TARGETS lightspark RUNTIME DESTINATION ${BINDIR})
  IF(UNIX)
    INSTALL(FILES ${PROJECT_SOURCE_DIR}/docs/man/lightspark.1 DESTINATION ${MANUAL_DIRECTORY}/man1/)
  ENDIF(UNIX)
ENDIF(COMPILE_LIGHTSPARK)

# tightspark executable target
IF(COMPILE_TIGHTSPARK)
  ADD_EXECUTABLE(tightspark tightspark.cpp)
  TARGET_LINK_LIBRARIES(tightspark spark)
  IF(MINGW)
    SET_TARGET_PROPERTIES(tightspark PROPERTIES LINK_FLAGS "-Wl,--whole-archive -lSDL2main -Wl,--no-whole-archive")
  ENDIF()
  #With STATICDEPS, all deps are compiled into spark
  IF(NOT STATICDEPS)
    TARGET_LINK_LIBRARIES(tightspark ${Boost_LIBRARIES} ${GLIBMM_LIBRARIES} ${GTHREAD_LIBRARIES})
  ENDIF()

  INSTALL(TARGETS tightspark RUNTIME DESTINATION ${BINDIR})
  PACK_EXECUTABLE(tightspark)
ENDIF(COMPILE_TIGHTSPARK)

# Browser plugins
IF(COMPILE_NPAPI_PLUGIN)
  ADD_SUBDIRECTORY(plugin)
ENDIF(COMPILE_NPAPI_PLUGIN)
IF(COMPILE_PPAPI_PLUGIN)
  ADD_SUBDIRECTORY(plugin_ppapi)
ENDIF(COMPILE_PPAPI_PLUGIN)


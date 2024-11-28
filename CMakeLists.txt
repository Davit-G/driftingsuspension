include (FetchContent)
Set(FETCHCONTENT_QUIET FALSE)

cmake_minimum_required(VERSION 3.15)
set(ProjectVersion 0.1.0)
set(PluginName "DriftingSuspension")

set(BaseTargetName DriftingSuspension)
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")


project(DriftingSuspension VERSION "${ProjectVersion}")



FetchContent_Declare(
	JUCE
	GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
	GIT_TAG 51d11a2be6d5c97ccf12b4e5e827006e19f0555a
	GIT_PROGRESS TRUE
    GIT_SHALLOW 1
)

FetchContent_MakeAvailable(JUCE)

# set commit hash for use in general idk
execute_process(
    COMMAND git log -1 --format=%h
    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
    OUTPUT_VARIABLE GIT_HASH
    OUTPUT_STRIP_TRAILING_WHITESPACE
)


if(APPLE)
    set(outdir "../build_mac")
else()
    if(UNIX)
        set(outdir "../build_linux")
    else()
        set(outdir "../build_windows")
    endif()
endif()

juce_add_plugin("${BaseTargetName}"
    VERSION "${ProjectVersion}"                 # Set this if the plugin version is different to the project version
    PRODUCT_NAME "${PluginName}"                # The name of the final executable, which can differ from the target name
    PLUGIN_MANUFACTURER_CODE Avry               # A four-character manufacturer id with at least one upper-case character
    PLUGIN_CODE DfSp                            # A unique four-character plugin id with exactly one upper-case character
    DESCRIPTION ""
    COMPANY_NAME "AviaryAudio"                          # Specify the name of the plugin's author
    COMPANY_WEBSITE "https://aviaryaudio.com"
    COMPANY_EMAIL "dave@aviaryaudio.com"
    IS_SYNTH FALSE                       # Is this a synth or an effect?
    NEEDS_MIDI_INPUT FALSE               # Does the plugin need midi input?
    NEEDS_MIDI_OUTPUT FALSE              # Does the plugin need midi output?
    IS_MIDI_EFFECT FALSE                 # Is this plugin a MIDI effect?
    # EDITOR_WANTS_KEYBOARD_FOCUS TRUE/FALSE    # Does the editor need keyboard focus?

    COPY_PLUGIN_AFTER_BUILD TRUE        # Should the plugin be installed to a default location after building?
    VST3_COPY_DIR "${outdir}"
	VST_COPY_DIR "${outdir}"
	AU_COPY_DIR "${outdir}"
    FORMATS VST3 Standalone AU                  # The formats to build. Other valid formats are: AAX Unity VST AU AUv3
    VST3_CATEGORIES Fx Distortion Dynamics
)

target_sources("${BaseTargetName}"
    PRIVATE
        PluginProcessor.cpp
        )

target_compile_features("${BaseTargetName}" PUBLIC cxx_std_17)

target_compile_definitions("${BaseTargetName}" PUBLIC
    PERFETTO=0
    #DONT_SET_USING_JUCE_NAMESPACE=1 # if i ever want to remove juce as being included in the namespace by default
    JUCE_DISPLAY_SPLASH_SCREEN=0
    JUCE_REPORT_APP_USAGE=0
    JUCE_USE_FLAC=0
    JUCE_USE_OGGVORBIS=0
    JUCE_USE_MP3AUDIOFORMAT=0
    JUCE_USE_LAME_AUDIO_FORMAT=0
    JUCE_USE_WINDOWS_MEDIA_FORMAT=0
    JUCE_VST3_CAN_REPLACE_VST2=0
    JUCE_ENABLE_REPAINT_DEBUGGING=0
    JUCE_COREGRAPHICS_RENDER_WITH_MULTIPLE_PAINT_CALLS=1
    JUCE_COREGRAPHICS_DRAW_ASYNC=1
    JUCE_USE_CURL=0
    JUCE_LOAD_CURL_SYMBOLS_LAZILY=1
    JUCE_STRICT_REFCOUNTEDPOINTER=1
    JUCE_BUILD_EXTRAS=0
    JUCE_BUILD_EXAMPLES=0
    JUCE_WEB_BROWSER=0
    JUCE_USE_WIN_WEBVIEW2=0
    JUCE_USE_CAMERA=0
    GIT_HASH="${GIT_HASH}"
)

# juce_add_binary_data("${BaseTargetName}BinaryData" HEADER_NAME BinaryData.h NAMESPACE BinaryData SOURCES
# )

# set_target_properties("${BaseTargetName}BinaryData" PROPERTIES POSITION_INDEPENDENT_CODE TRUE)

# FetchContent_Declare (melatonin_perfetto
#     GIT_REPOSITORY https://github.com/sudara/melatonin_perfetto.git
#     GIT_SHALLOW 1
#     GIT_TAG origin/main)
    
# FetchContent_Declare (melatonin_inspector
#     GIT_REPOSITORY https://github.com/sudara/melatonin_inspector.git
#     GIT_SHALLOW 1
#     GIT_TAG origin/main
#     SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/melatonin_inspector)

FetchContent_Declare (clap-juce-extensions
    GIT_REPOSITORY https://github.com/free-audio/clap-juce-extensions.git
    GIT_SHALLOW 1
    GIT_TAG origin/main)

FetchContent_MakeAvailable (clap-juce-extensions)

# if (PERFETTO)
# FetchContent_MakeAvailable (melatonin_perfetto)
# endif()


clap_juce_extensions_plugin(TARGET "${BaseTargetName}"
CLAP_ID "com.AviaryAudio.${PluginName}"
CLAP_FEATURES effect "tape wow flutter" audio-effect)

target_link_libraries("${BaseTargetName}"
    PRIVATE
        juce::juce_dsp
        juce::juce_audio_utils
        # "${BaseTargetName}BinaryData"
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)
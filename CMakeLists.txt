set(COMPONENT_SRCS
    speech_command_recognition/mn_process_commands.c
    esp-tts/esp_tts_wav/wav_decoder.c
    esp-tts/esp_tts_wav/wav_encoder.c
    )

set(COMPONENT_ADD_INCLUDEDIRS 
    lib/include
    wake_word_engine/include
    speech_command_recognition/include
    acoustic_algorithm/include
    esp-tts/esp_tts_chinese/include
    esp-tts/esp_tts_wav/
    )


register_component()

target_link_libraries(${COMPONENT_TARGET} "-L ${CMAKE_CURRENT_SOURCE_DIR}/lib")
target_link_libraries(${COMPONENT_TARGET} "-L ${CMAKE_CURRENT_SOURCE_DIR}/wake_word_engine")
target_link_libraries(${COMPONENT_TARGET} "-L ${CMAKE_CURRENT_SOURCE_DIR}/speech_command_recognition")
target_link_libraries(${COMPONENT_TARGET} "-L ${CMAKE_CURRENT_SOURCE_DIR}/acoustic_algorithm")
target_link_libraries(${COMPONENT_TARGET} "-L ${CMAKE_CURRENT_SOURCE_DIR}/esp-tts/esp_tts_chinese")

IF (IDF_VER MATCHES "v4.")
add_prebuilt_library(multinet "${CMAKE_CURRENT_SOURCE_DIR}/lib/libmultinet.a" PRIV_REQUIRES esp-sr) 
ENDIF (IDF_VER MATCHES "v4.")

target_link_libraries(${COMPONENT_TARGET} "-Wl,--start-group"
    wakenet
    dl_lib
    c_speech_features
    hilexin_wn3
    hilexin_wn4
    hilexin_wn5
    hilexin_wn5X2
    hilexin_wn5X3
    hijeson_wn5X3
    nihaoxiaozhi_wn5
    nihaoxiaozhi_wn5X2
    nihaoxiaozhi_wn5X3
    nihaoxiaoxin_wn6
    nihaoxiaoxin_wn5X3
    customized_word_wn5
    customized_word_wn6
    multinet
    multinet1_ch
    multinet1_en
    esp_tts_chinese 
    voice_set_xiaole 
    voice_set_template
    esp_audio_processor "-Wl,--end-group")

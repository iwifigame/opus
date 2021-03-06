cmake_minimum_required(VERSION 3.1)
project(opus)

#pull submodule if not already here
execute_process(COMMAND git submodule update --init -- .
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

option(OPUS_USE_ALLOCA "USE_ALLOCA" ON)
option(OPUS_FIXED_POINT "FIXED_POINT" ON)
option(OPUS_DISABLE_FLOAT_API "DISABLE_FLOAT_API" OFF)

set(OPUS_INCLUDE_DIR
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        )

set(SILK_SOURCES
        silk/CNG.c
        silk/code_signs.c
        silk/init_decoder.c
        silk/decode_core.c
        silk/decode_frame.c
        silk/decode_parameters.c
        silk/decode_indices.c
        silk/decode_pulses.c
        silk/decoder_set_fs.c
        silk/dec_API.c
        silk/enc_API.c
        silk/encode_indices.c
        silk/encode_pulses.c
        silk/gain_quant.c
        silk/interpolate.c
        silk/LP_variable_cutoff.c
        silk/NLSF_decode.c
        silk/NSQ.c
        silk/NSQ_del_dec.c
        silk/PLC.c
        silk/shell_coder.c
        silk/tables_gain.c
        silk/tables_LTP.c
        silk/tables_NLSF_CB_NB_MB.c
        silk/tables_NLSF_CB_WB.c
        silk/tables_other.c
        silk/tables_pitch_lag.c
        silk/tables_pulses_per_block.c
        silk/VAD.c
        silk/control_audio_bandwidth.c
        silk/quant_LTP_gains.c
        silk/VQ_WMat_EC.c
        silk/HP_variable_cutoff.c
        silk/NLSF_encode.c
        silk/NLSF_VQ.c
        silk/NLSF_unpack.c
        silk/NLSF_del_dec_quant.c
        silk/process_NLSFs.c
        silk/stereo_LR_to_MS.c
        silk/stereo_MS_to_LR.c
        silk/check_control_input.c
        silk/control_SNR.c
        silk/init_encoder.c
        silk/control_codec.c
        silk/A2NLSF.c
        silk/ana_filt_bank_1.c
        silk/biquad_alt.c
        silk/bwexpander_32.c
        silk/bwexpander.c
        silk/debug.c
        silk/decode_pitch.c
        silk/inner_prod_aligned.c
        silk/lin2log.c
        silk/log2lin.c
        silk/LPC_analysis_filter.c
        silk/LPC_inv_pred_gain.c
        silk/table_LSF_cos.c
        silk/NLSF2A.c
        silk/NLSF_stabilize.c
        silk/NLSF_VQ_weights_laroia.c
        silk/pitch_est_tables.c
        silk/resampler.c
        silk/resampler_down2_3.c
        silk/resampler_down2.c
        silk/resampler_private_AR2.c
        silk/resampler_private_down_FIR.c
        silk/resampler_private_IIR_FIR.c
        silk/resampler_private_up2_HQ.c
        silk/resampler_rom.c
        silk/sigm_Q15.c
        silk/sort.c
        silk/sum_sqr_shift.c
        silk/stereo_decode_pred.c
        silk/stereo_encode_pred.c
        silk/stereo_find_predictor.c
        silk/stereo_quant_pred.c
        silk/LPC_fit.c
        )

set(SILK_SOURCES_SSE4_1
        silk/x86/NSQ_sse.c
        silk/x86/NSQ_del_dec_sse.c
        silk/x86/x86_silk_map.c
        silk/x86/VAD_sse.c
        silk/x86/VQ_WMat_EC_sse.c
        )

set(SILK_SOURCES_ARM_NEON_INTR
        silk/arm/arm_silk_map.c
        silk/arm/NSQ_del_dec_neon_intr.c
        silk/arm/NSQ_neon.c
        )

set(SILK_SOURCES_FIXED
        silk/fixed/LTP_analysis_filter_FIX.c
        silk/fixed/LTP_scale_ctrl_FIX.c
        silk/fixed/corrMatrix_FIX.c
        silk/fixed/encode_frame_FIX.c
        silk/fixed/find_LPC_FIX.c
        silk/fixed/find_LTP_FIX.c
        silk/fixed/find_pitch_lags_FIX.c
        silk/fixed/find_pred_coefs_FIX.c
        silk/fixed/noise_shape_analysis_FIX.c
        silk/fixed/process_gains_FIX.c
        silk/fixed/regularize_correlations_FIX.c
        silk/fixed/residual_energy16_FIX.c
        silk/fixed/residual_energy_FIX.c
        silk/fixed/warped_autocorrelation_FIX.c
        silk/fixed/apply_sine_window_FIX.c
        silk/fixed/autocorr_FIX.c
        silk/fixed/burg_modified_FIX.c
        silk/fixed/k2a_FIX.c
        silk/fixed/k2a_Q16_FIX.c
        silk/fixed/pitch_analysis_core_FIX.c
        silk/fixed/vector_ops_FIX.c
        silk/fixed/schur64_FIX.c
        silk/fixed/schur_FIX.c
        )

set(SILK_SOURCES_FIXED_SSE4_1
        silk/fixed/x86/vector_ops_FIX_sse.c
        silk/fixed/x86/burg_modified_FIX_sse.c
        )

set(SILK_SOURCES_FLOAT
        silk/float/apply_sine_window_FLP.c
        silk/float/corrMatrix_FLP.c
        silk/float/encode_frame_FLP.c
        silk/float/find_LPC_FLP.c
        silk/float/find_LTP_FLP.c
        silk/float/find_pitch_lags_FLP.c
        silk/float/find_pred_coefs_FLP.c
        silk/float/LPC_analysis_filter_FLP.c
        silk/float/LTP_analysis_filter_FLP.c
        silk/float/LTP_scale_ctrl_FLP.c
        silk/float/noise_shape_analysis_FLP.c
        silk/float/process_gains_FLP.c
        silk/float/regularize_correlations_FLP.c
        silk/float/residual_energy_FLP.c
        silk/float/warped_autocorrelation_FLP.c
        silk/float/wrappers_FLP.c
        silk/float/autocorrelation_FLP.c
        silk/float/burg_modified_FLP.c
        silk/float/bwexpander_FLP.c
        silk/float/energy_FLP.c
        silk/float/inner_product_FLP.c
        silk/float/k2a_FLP.c
        silk/float/LPC_inv_pred_gain_FLP.c
        silk/float/pitch_analysis_core_FLP.c
        silk/float/scale_copy_vector_FLP.c
        silk/float/scale_vector_FLP.c
        silk/float/schur_FLP.c
        silk/float/sort_FLP.c
        )

set(CELT_SOURCES
        celt/bands.c
        celt/celt.c
        celt/celt_encoder.c
        celt/celt_decoder.c
        celt/cwrs.c
        celt/entcode.c
        celt/entdec.c
        celt/entenc.c
        celt/kiss_fft.c
        celt/laplace.c
        celt/mathops.c
        celt/mdct.c
        celt/modes.c
        celt/pitch.c
        celt/celt_lpc.c
        celt/quant_bands.c
        celt/rate.c
        celt/vq.c
        )

set(CELT_SOURCES_SSE
        celt/x86/x86cpu.c
        celt/x86/x86_celt_map.c
        celt/x86/pitch_sse.c
        )

set(CELT_SOURCES_SSE2
        celt/x86/pitch_sse2.c
        celt/x86/vq_sse2.c
        )

set(CELT_SOURCES_SSE4_1
        celt/x86/celt_lpc_sse.c
        celt/x86/pitch_sse4_1.c
        )

set(CELT_SOURCES_ARM
        celt/arm/armcpu.c
        celt/arm/arm_celt_map.c
        )

set(CELT_SOURCES_ARM_ASM
        celt/arm/celt_pitch_xcorr_arm.s
        )

set(CELT_AM_SOURCES_ARM_ASM
        celt/arm/armopts.s.in
        )

set(CELT_SOURCES_ARM_NEON_INTR
        celt/arm/celt_neon_intr.c
        )

set(CELT_SOURCES_ARM_NE10
        celt/arm/celt_ne10_fft.c
        celt/arm/celt_ne10_mdct.c
        )

set(OPUS_HEADER
        opus/include/opus.h
        opus/include/opus_custom.h
        opus/include/opus_defines.h
        opus/include/opus_multistream.h
        opus/include/opus_types.h
        )

set(OPUS_SOURCES
        src/opus.c
        src/opus_decoder.c
        src/opus_encoder.c
        src/opus_multistream.c
        src/opus_multistream_encoder.c
        src/opus_multistream_decoder.c
        src/repacketizer.c
        )

set(OPUS_SOURCES_FLOAT
        src/analysis.c
        src/mlp.c
        src/mlp_data.c
        )


set(OPUS_EXAMPLE
        example/dr_wav.h
        example/opus.cpp
        )


if (OPUS_FIXED_POINT)
    list(APPEND SILK_SOURCES ${SILK_SOURCES_FIXED})
    if (HAVE_SSE4_1)
        list(APPEND SILK_SOURCES ${SILK_SOURCES_SSE4_1} ${SILK_SOURCES_FIXED_SSE4_1})
    endif (HAVE_SSE4_1)
else (OPUS_FIXED_POINT)
    list(APPEND SILK_SOURCES ${SILK_SOURCES_FLOAT})
    if (HAVE_SSE4_1)
        list(APPEND SILK_SOURCES ${SILK_SOURCES_SSE4_1})
    endif (HAVE_SSE4_1)
endif (OPUS_FIXED_POINT)

if (NOT OPUS_DISABLE_FLOAT_API)
    list(APPEND OPUS_SOURCES ${OPUS_SOURCES_FLOAT})
endif (NOT OPUS_DISABLE_FLOAT_API)

if (HAVE_SSE)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_SSE})
endif (HAVE_SSE)

if (HAVE_SSE2)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_SSE2})
endif (HAVE_SSE2)

if (HAVE_SSE4_1)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_SSE4_1})
endif (HAVE_SSE4_1)

if (CPU_ARM)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_ARM})
    list(APPEND SILK_SOURCES ${SILK_SOURCES_ARM})

    if (HAVE_ARM_NEON_INTR)
        list(APPEND CELT_SOURCES ${CELT_SOURCES_ARM_NEON_INTR})
        list(APPEND SILK_SOURCES ${SILK_SOURCES_ARM_NEON_INTR})
    endif (HAVE_ARM_NEON_INTR)

    if (HAVE_ARM_NE10)
        list(APPEND CELT_SOURCES ${CELT_SOURCES_ARM_NE10})
    endif (HAVE_ARM_NE10)

    #if(OPUS_ARM_EXTERNAL_ASM)
    #	noinst_LTLIBRARIES = libarmasm.la
    #	libarmasm_la_SOURCES = $(CELT_SOURCES_ARM_ASM:.s=-gnu.S)
    #	BUILT_SOURCES = $(CELT_SOURCES_ARM_ASM:.s=-gnu.S) \
    #	$(CELT_AM_SOURCES_ARM_ASM:.s.in=.s) \
    #	$(CELT_AM_SOURCES_ARM_ASM:.s.in=-gnu.S)
    #endif(OPUS_ARM_EXTERNAL_ASM)
endif (CPU_ARM)

# It is strongly recommended to uncomment one of these
# VAR_ARRAYS: Use C99 variable-length arrays for stack allocation
# USE_ALLOCA: Use alloca() for stack allocation
# If none is defined, then the fallback is a non-threadsafe global array
if (OPUS_USE_ALLOCA)
    add_definitions(-DUSE_ALLOCA)
else (OPUS_USE_ALLOCA)
    add_definitions(-DVAR_ARRAYS)
endif (OPUS_USE_ALLOCA)

# These options affect performance
# HAVE_LRINTF: Use C99 intrinsics to speed up float-to-int conversion
add_definitions(-DHAVE_LRINTF)

add_definitions(-DOPUS_BUILD)

if (OPUS_FIXED_POINT)
    add_definitions(-DFIXED_POINT=1 -DDISABLE_FLOAT_API)
endif (OPUS_FIXED_POINT)

# add_library(opus STATIC ${CELT_SOURCES} ${SILK_SOURCES} ${OPUS_SOURCES})

add_executable(opus ${OPUS_EXAMPLE} ${CELT_SOURCES} ${SILK_SOURCES} ${OPUS_SOURCES})


target_compile_options(opus PRIVATE $<$<OR:$<C_COMPILER_ID:Clang>,$<C_COMPILER_ID:GNU>>:
        -Wall
        -Wextra
        -Wcast-align
        -Wnested-externs
        -Wshadow
        -Wstrict-prototypes
        >)
target_include_directories(opus PUBLIC ${OPUS_INCLUDE_DIR})
target_include_directories(opus PRIVATE
        silk
        silk/float
        silk/fixed
        celt
        src
        )

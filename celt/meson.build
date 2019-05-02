
celt_lib = static_library('celt',
    'bands.c',
    'celt.c',
    'celt_decoder.c',
    'celt_encoder.c',
    'celt_lpc.c',
    'cwrs.c',
    'entcode.c',
    'entdec.c',
    'entenc.c',
    'kiss_fft.c',
    'laplace.c',
    'mathops.c',
    'mdct.c',
    'modes.c',
    'pitch.c',
    'quant_bands.c',
    'rate.c',
    'vq.c',

    c_args : opus_cflags,
    build_by_default : false,
    include_directories : opus_incdirs_private,
)

celt_sse_lib = static_library('celt_sse',
    'x86/pitch_sse.c',
    'x86/x86_celt_map.c',
    'x86/x86cpu.c',

    c_args : [opus_cflags, sse1_flags],
    build_by_default : false,
    include_directories : opus_incdirs_private,
)

celt_sse2_lib = static_library('celt_sse2',
    'x86/pitch_sse2.c',
    'x86/vq_sse2.c',

    c_args : [opus_cflags, sse2_flags],
    build_by_default : false,
    include_directories : opus_incdirs_private,
)

celt_sse4_1_lib = static_library('celt_sse4_1',
    'x86/celt_lpc_sse4_1.c',
    'x86/pitch_sse4_1.c',

    c_args : [opus_cflags, sse4_1_flags],
    build_by_default : false,
    include_directories : opus_incdirs_private,
)

celt_arm_lib = static_library('celt_arm',
    'arm/armcpu.c',
    'arm/arm_celt_map.c',

    c_args : opus_cflags,
    build_by_default : false,
    include_directories : opus_incdirs_private,
)

celt_arm_neon_intr_lib = static_library('celt_arm_neon_intr',
    'arm/celt_neon_intr.c',
    'arm/pitch_neon_intr.c',

    c_args : opus_cflags,
    build_by_default : false,
    include_directories : opus_incdirs_private,
)

celt_libs = [celt_lib]

if have_sse1
    celt_libs += celt_sse_lib
endif

if have_sse2
    celt_libs += celt_sse2_lib
endif

if have_sse4_1
    celt_libs += celt_sse4_1_lib
endif

if is_arm_32 or is_arm_64
    celt_libs += celt_arm_lib
endif

if have_neon
    celt_libs += celt_arm_neon_intr_lib
endif

celt_dep = declare_dependency(
    link_with : celt_libs
)

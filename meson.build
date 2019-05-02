project('opus', 'c', version : '1.3.1', license : 'bsd-style')

m_dep = meson.get_compiler('c').find_library('m', required : false)

cc = meson.get_compiler('c')

want_sse1 = true
have_sse1 = false
presume_sse1 = false
sse1_flags = ['-msse']

want_sse2 = true
have_sse2 = false
presume_sse2 = false
sse2_flags = ['-msse2']

want_sse4_1 = true
have_sse4_1 = false
presume_sse4_1 = false
sse4_1_flags = ['-msse4.1']

want_avx = true
have_avx = false
presume_avx = false
avx_flags = ['-mavx']

want_neon = false
have_neon = false
presume_neon = false
neon_flags = []

want_simd = true
have_simd = false

want_cpu_detection = true
have_cpu_detection = false

is_x86_32 = host_machine.cpu_family() == 'x86'
is_x86_64 = host_machine.cpu_family() == 'x86_64'
is_arm_32 = host_machine.cpu_family() == 'arm'
is_arm_64 = host_machine.cpu_family() == 'aarch64'

if is_x86_64
    presume_sse1 = true
    presume_sse2 = true
endif

if is_arm_64
    presume_neon = true
endif

opus_cflags = [
    '-DCUSTOM_MODES',
    '-DENABLE_HARDENING',
    '-DOPUS_BUILD',
    '-DVAR_ARRAYS',
]

if want_simd
    simd = [
        ['sse1',    'xmmintrin.h', sse1_flags,   '-DOPUS_X86_MAY_HAVE_SSE',    '-DOPUS_X86_PRESUME_SSE'],
        ['sse2',    'emmintrin.h', sse2_flags,   '-DOPUS_X86_MAY_HAVE_SSE2',   '-DOPUS_X86_PRESUME_SSE2'],
        ['sse4_1',  'smmintrin.h', sse4_1_flags, '-DOPUS_X86_MAY_HAVE_SSE4_1', '-DOPUS_X86_PRESUME_SSE4_1'],
        ['avx',     'immintrin.h', avx_flags,    '-DOPUS_X86_MAY_HAVE_AVX',    '-DOPUS_X86_PRESUME_AVX'],
    ]

    if is_arm_32 or is_arm_64
        simd += [
            ['neon', 'arm_neon.h', neon_flags, '-DOPUS_ARM_MAY_HAVE_NEON_INTR', ['-DOPUS_ARM_PRESUME_NEON_INTR', '-DOPUS_ARM_PRESUME_NEON']]
        ]
    endif

    foreach iset : simd
        name = iset[0]
        header = iset[1]
        flags = iset[2]
        flags_mayhave = iset[3]
        flags_presume = iset[4]

        if (
            get_variable('want_@0@'.format(name)) and
            cc.check_header(header) and
            cc.has_multi_arguments(flags)
        )
            set_variable('have_@0@'.format(name), true)
            opus_cflags += flags_mayhave
            have_simd = true

            if get_variable('presume_@0@'.format(name))
                opus_cflags += [flags, flags_presume]
            endif
        endif
    endforeach

    have_cpu_detection = (
        have_simd and
        want_cpu_detection and
        cc.check_header('cpuid.h') or cc.check_header('intrin.h')
    )

    if have_cpu_detection
        opus_cflags += '-DOPUS_HAVE_RTCD'
    endif
endif

opus_incdirs = include_directories('include')
opus_incdirs_private = [opus_incdirs, include_directories(
    '.',
    'celt',
    'silk',
    'silk/float',
)]

subdir('celt')
subdir('silk')
subdir('src')


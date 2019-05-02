
opus_lib = library('opus',
    'analysis.c',
    'mapping_matrix.c',
    'mlp.c',
    'mlp_data.c',
    'opus.c',
    'opus_decoder.c',
    'opus_encoder.c',
    'opus_multistream.c',
    'opus_multistream_decoder.c',
    'opus_multistream_encoder.c',
    'opus_projection_decoder.c',
    'opus_projection_encoder.c',
    'repacketizer.c',

    c_args : opus_cflags,
    build_by_default : not meson.is_subproject(),
    include_directories : opus_incdirs_private,
    dependencies : [m_dep, silk_dep, celt_dep],
)

libopus_dep = declare_dependency(
    include_directories : opus_incdirs,
    link_with : opus_lib,
    version : meson.project_version(),
)

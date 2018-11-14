def generated_headers(system_name, system_processor):
  cmake_name = 'cmake-' + system_name.lower() + '-' + system_processor.lower()

  genrule(
    name = cmake_name, 
    out = 'out', 
    srcs = glob([
      'cmakescripts/*.in', 
      'cmakescripts/*.cmake', 
      'release/*.in', 
      'md5/*.txt', 
      'md5/*.c', 
      'sharedlib/*.txt', 
      'simd/*.txt', 
      '*.txt', 
      '*.in', 
      '*.c', 
    ]), 
    cmd = ' && '.join([
      'cd $TMP', 
      ''.join([
        'cmake $SRCDIR -G "Unix Makefiles" -DCMAKE_SYSTEM_NAME=', 
        system_name, 
        ' -DCMAKE_SYSTEM_PROCESSOR=', 
        system_processor, 
        ' -DENABLE_SHARED=1 -DENABLE_STATIC=1 -DWITH_SIMD=0', 
      ]), 
      'mkdir -p $OUT', 
      'cp $TMP/jconfig.h $OUT/jconfig.h', 
      'cp $TMP/jconfigint.h $OUT/jconfigint.h', 
    ]), 
  )

  jconfig_h_name = 'jconfig-h-' + system_name.lower() + '-' + system_processor.lower()
  jconfigint_h_name = 'jconfigint-h-' + system_name.lower() + '-' + system_processor.lower()

  genrule(
    name = jconfig_h_name, 
    out = 'jconfig.h', 
    cmd = 'cp $(location :' + cmake_name + ')/jconfig.h $OUT', 
  )

  genrule(
    name = jconfigint_h_name, 
    out = 'jconfigint.h', 
    cmd = 'cp $(location :' + cmake_name + ')/jconfigint.h $OUT', 
  )

  return [ ':' + jconfig_h_name, ':' + jconfigint_h_name ]

# These c files are actually headers
c_headers = [
  'jstdhuff.c', 
  'jccolext.c', 
  'jdcolext.c', 
  'jdcol565.c', 
  'jdmrgext.c', 
  'jdmrg565.c', 
]

cxx_library(
  name = 'jpeg-turbo', 
  header_namespace = '', 
  exported_headers = glob([
    'md5/*.h', 
    '*.h', 
  ]), 
  exported_platform_headers = [
    ('macos.*', generated_headers('Darwin', 'x86_64')), 
    ('linux.*', generated_headers('Linux', 'x86_64')), 
  ], 
  headers = c_headers, 
  srcs = glob([
    'md5/*.c', 
    '*.c', 
  ], exclude = c_headers + glob([ 
    '*test.c', 
    '*example.c', 
    'cjpeg.c', 
    'djpeg.c', 
    'turbojpeg-jni.c', 
    'jpegtran.c', 
    'wrjpgcom.c', 
    'rdjpgcom.c', 
    'md5cmp.c', 
  ])), 
  visibility = [
    'PUBLIC', 
  ], 
)

cxx_binary(
  name = 'cjpeg', 
  srcs = [
    'cjpeg.c', 
  ], 
  deps = [
    ':jpeg-turbo', 
  ], 
)

cxx_binary(
  name = 'djpeg', 
  srcs = [
    'djpeg.c', 
  ], 
  deps = [
    ':jpeg-turbo', 
  ], 
)

cxx_binary(
  name = 'jcstest', 
  srcs = [
    'jcstest.c', 
  ], 
  deps = [
    ':jpeg-turbo', 
  ], 
)

cxx_binary(
  name = 'jpegtran', 
  srcs = [
    'jpegtran.c', 
  ], 
  deps = [
    ':jpeg-turbo', 
  ], 
)

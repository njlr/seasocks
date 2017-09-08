include_defs('//BUCKAROO_DEPS')

def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

config_h = '''#pragma once

namespace seasocks {

    struct Config {
        static constexpr auto version = \\\"1.3.1\\\";
        static constexpr bool deflateEnabled = true;
    };

}
''';

genrule(
  name = 'config-file',
  out = 'Config.h',
  cmd = 'echo "' + config_h + '" > $OUT ',
)

web_files = glob([
  'src/main/web/*.css',
  'src/main/web/*.html',
  'src/main/web/*.ico',
  'src/main/web/*.js',
  'src/main/web/*.png',
])

genrule(
  name = 'embedded',
  out = 'Embedded.cpp',
  srcs = glob([
    'scripts/gen_embedded.py',
  ]) + web_files,
  cmd = 'python $SRCDIR/scripts/gen_embedded.py ' + \
        ' '.join([ '$SRCDIR/' + x for x in web_files ]) + ' > $OUT ',
)

cxx_library(
  name = 'internal',
  header_namespace = 'internal',
  exported_headers = merge_dicts(subdir_glob([
    ('src/main/c/internal', '**/*.h'),
  ]), {
    'Config.h': ':config-file',
  }),
  srcs = glob([
    'src/main/c/internal/**/*.cpp',
  ]),
  visibility = [
    '//...',
  ],
)

cxx_library(
  name = 'seasocks',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('src/main/c', 'seasocks/**/*.h'),
  ]),
  headers = subdir_glob([
    ('src/main/c', 'md5/**/*.h'),
    ('src/main/c', 'sha1/**/*.h'),
    ('src/main/c', 'util/**/*.h'),
  ]),
  srcs = glob([
    'src/main/c/**/*.cpp',
  ], excludes = glob([
    'src/main/c/internal/**/*.cpp',
  ])) + [
    ':embedded',
  ],
  deps = BUCKAROO_DEPS + [
    ':internal',
  ],
  visibility = [
    'PUBLIC',
  ],
)

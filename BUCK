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

cxx_library(
  name = 'internal',
  header_namespace = 'internal',
  exported_headers = merge_dicts(subdir_glob([
    ('src/main/c/internal', '**/*.h'),
  ]), {
    'Config.h': ':config-file',
  }),
  # headers = subdir_glob([
  #   ('src/main/c', 'internal/**/*.h'),
  #   ('src/main/c', 'md5/**/*.h'),
  #   ('src/main/c', 'sha1/**/*.h'),
  #   ('src/main/c', 'util/**/*.h'),
  # ]),
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
    ('src/main/c', 'internal/**/*.h'),
    ('src/main/c', 'md5/**/*.h'),
    ('src/main/c', 'sha1/**/*.h'),
    ('src/main/c', 'util/**/*.h'),
  ]),
  srcs = glob([
    'src/main/c/**/*.cpp',
  ], excludes = glob([
    'src/main/c/internal/**/*.cpp',
  ])),
  deps = [
    ':internal',
  ],
  visibility = [
    'PUBLIC',
  ],
)

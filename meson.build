project('libdecor-examples', 'c',
  version : '0.1',
  default_options : ['warning_level=3'])

wayland_client_dep = dependency('wayland-client', version: '>= 1.18')
wayland_cursor_dep = dependency('wayland-cursor')
xkb_dep = dependency('xkbcommon')

wayland_protocols_dep = dependency('wayland-protocols', version: '>= 1.15')
protocols_dir = wayland_protocols_dep.get_pkgconfig_variable('pkgdatadir')

wayland_scanner = find_program('wayland-scanner')

xdg_shell_path = join_paths(protocols_dir, 'stable', 'xdg-shell', 'xdg-shell.xml')

xdg_shell_client_header = custom_target('xdg-shell client header (demo)',
  input: xdg_shell_path,
  output: 'xdg-shell-client-protocol.h',
  command: [
    wayland_scanner,
    'client-header',
    '@INPUT@', '@OUTPUT@',
  ]
)

xdg_shell_source = custom_target('xdg-shell source (demo)',
  input: xdg_shell_path,
  output: 'xdg-shell-protocol.c',
  command: [
    wayland_scanner,
    'private-code',
    '@INPUT@', '@OUTPUT@',
  ]
)

libdecor_dep = dependency('libdecor-0', version : '>=0.1.0')

exe = executable('demo',
  sources: [
    'demo.c',
    'os-compatibility.c',
    xdg_shell_client_header,
    xdg_shell_source,
  ],
  dependencies: [
    libdecor_dep,
    wayland_client_dep,
    wayland_cursor_dep,
    xkb_dep,
  ],
  install : true)

test('basic', exe)

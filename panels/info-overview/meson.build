panels_list += cappletname
desktop = 'budgie-@0@-panel.desktop'.format(cappletname)

desktop_in = configure_file(
  input: desktop + '.in.in',
  output: desktop + '.in',
  configuration: desktop_conf
)

i18n.merge_file(
  type: 'desktop',
  input: desktop_in,
  output: desktop,
  po_dir: po_dir,
  install: true,
  install_dir: control_center_desktopdir
)

cflags += [
  '-DDATADIR="@0@"'.format(control_center_datadir),
  '-DLIBEXECDIR="@0@"'.format(control_center_libexecdir),
]

sources = files(
  'cc-info-overview-panel.c',
  'info-cleanup.c'
)

resource_data = files(
  'cc-info-overview-panel.ui'
)

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  c_name: 'cc_' + cappletname.underscorify (),
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  dependency('udisks2', version: '>= 2.8.2'),
  dependency('libgtop-2.0')
]

info_panel_lib = static_library(
  cappletname,
  sources: sources,
  include_directories: [ top_inc, common_inc ],
  dependencies: deps,
  c_args: cflags
)
panels_libs += info_panel_lib

print_renderer_deps = [
  dependency('gtk+-3.0'),
  epoxy_dep
]

executable(
  'budgie-control-center-print-renderer',
  'budgie-control-center-print-renderer.c',
  include_directories: [ top_inc ],
  dependencies: [ print_renderer_deps ],
  c_args: cflags,
  install: true,
  install_dir: control_center_libexecdir,
)

install_data(
  [ 'budgie-logo.png' ],
  install_dir: join_paths(control_center_datadir, 'pixmaps')
)

subdir('icons')

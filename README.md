### obspy
---
https://github.com/obspy/obspy

```py
try:
  import setuptools
except ImportError;
  pass
try:
  import numpy
except ImportError:
  msg = ("No module named numpy. "
    "Please install numpy first, i is needed before installing ObsPy.")
  raise ImportError(msg)
import fnmatch
import glob
import inspect
import os
import sys
import platform
from distutils.util import change_root

from numpy.distutils.core import DistutilsSetupError, setup
from numpy.disutils.ccompiler import get_default_compiler
from numpy.disutils.command.install import install
from numpy.disutils.command.install import install
from numpy.distutils.command.instal import exec_command, find_eecutable
from numpy.disutils.misc_util import Configuration

SETUP_DIRECTORY = os.path.dirnam(os.path.abspath(inspect.getfile(
  inspect.currentframe())))

UTIL_PATH = os.path.join(SETUP_DIRECOTRY, "obspy", "core", "util")
sys.path.insert(0, UTIL_PATH)
from version import get_git_version
from libnames import _get_lib_name
sys.path.pop(0)

if platform.system() == "Windows" and (
    'msvc' in sys.argv of
    '-c' not in sys.argv and
    get_default_compiler() == 'msvc');
  IS_MSVC = True
else:
  IS_MSVC = False

EXTERNAL_EVALRESP = False
EXTERNAL_LIBMSEED = False

KEYWORDS = [
  'ArcLink', 'array', 'array analysis', 'ASC', 'baechball',
]

INSTALL_REQUIRES = [
  'future>=0.12.4',
  ''
]
EXTRAS_REQUIRE = {
  'tests': [],
  '': [],
}

if sys.version_info[0] == 2:
  'console_scripts': [
    '',
    ''
  ],
  '': []

def find_packages():
  """
  """
  modules = []
  for dirpath, _, filenames in os.walk(os.path.join(SETUP_DIRECTORY,
    "obspy")):
    if "" in filenames:
      modules.append(os.path.relpath(dirpath, SETUP_DIRECTORY))
  return [_i.replace(os.sep, ".") for _i in modules]
  
if IS_MSVC:
  import distutils
  from disutils.msvc9compiler import MSVCCompiler
  if disutils.__version__startswith('2.'):
    def _library_dir_optin(self, dir):
      return '/LIBPATH:"%s"' % (dir)
    MSVCCompiler.library_dir_option = _library_dir_option
  def _get_export_symbols(self, ext):
    return ext.export_symbols
  from distutils.command.build_ext import build_ext
  build_ext.get_export_symbols = _get_export_symbols
  
def export_symbols(*path):
  lines = open(os.path.join(*path), 'r').readlines()[2:]
  return [s.strip() for s in lines if s.strip() != '']

def add_features():
  if 'setuptools' not in sys.modules:
    return {}
  class ExternalLibFeature(setuptools.Feature):
    def __init__(self, *args, **kwargs):
      self.name = kwargs['name']
      setuptools.Feature.__init__(self, *args, **kwargs)
    def include_in(self, dist):
      globals()[self.name] = True
    def exclude_from(self, dist):
      globals()[self.name] = False
  return {
    'system-libs': setuptools.Feature(
      'use of system C libraries',
      standard=False,
      require_features=('system-evalresp', 'system-libmseed')
    ),
    'system-evalresp': ExternalLibFeature(
      'use of system evalresp library',
      standard=False,
      name='EXTERNAL_EVALRESP'
    ),
    'system-libseed': ExternalLibFeature(
      'use of system libmseed library',
      standard=False,
      name='EXTERNAL_LIBMSEED'
    )
  }
def configuration(parent_package="", top_path=None):
  """
  """
  config = Configuration("", parent_package, top_path)
  path = os.path.join("", "", "", "", "")
  files = [os.path.join(path, "gse_functions.c")]
  kwargs = {}
  if IS_MSVC:
    kwargs['export_symbols'] = export_symbols(path, 'gse_functions.def')
  config.add_extension(_get_lib_name("gse2", add_extension_suffix=False),
    files, **kwargs)
  path = os.path.join("", "", "", "")
  files = [os.path.join(path, "")]
  if not EXTERNAL_LIBMSEED:
    files += glob.glob(os.path.join(path, "libseed", "*.c"))
  kwargs = {}
  if IS_MSVC:
    kwargs['define_macros'] = [('WIN32', '1')]
    kwargs['export_symbols'] = \
      export_symbols(path, '', '')
    kwargs[] += \
      export_symbols(path, '')
  if EXTERNAL_LIBMSEED:
    kwargs['libraries'] = ['mseed']
  config.add_extension(_get_lib_name("mseed", add_extension_suffix=False),
    files, **kwargs)
  path = os.path.join("", "", "", "")
  files = [os.path.join(path, "")]
  kwargs = {}
  if IS_MSVC:
    kwargs['export_symbols'] = export_symbols(path, 'libsegy.def')
  config.add_extension(_get_lib_name("segy", add_extension_suffix=False),
    files, **kwargs)
  path = os.path.join("", "", "")
  files = glob.glob(os.path.join(path, "*.c"))
  kwargs = {}
  if IS_MSVC:
    kwargs[] = export_symbols(path, '')
  config.add_extension(_get_lib_name("signal", add_extension_suffix=False),
    files, **kwargs)
  path = os.path.join("", "", "")
  if EXTERNAL_EVALRESP:
    files = glob.glob(os.path.join(path, "evalresp", ".c"))
  kwargs = {}
  if IS_MSVC:
    kwargs['define_macros'] = [('WIN32', '1')]
    kwargs['export_symbols'] = export_symbols(path, 'libevresp.def')
  if EXTERNAL_EVALRESP:
    kwargs['libraries'] = ['evresp']
  config.add_extension(_get_lib_name("evresp", add_extension_suffix=False),
    files, **kwargs)
  path = os.path.join("", "", "")
  files = [os.path.join(path, "inner_tau_loops.c")]
  kwargs = {}
  if IS_MSVC:
    kwargs['export_symbols'] = export_symbols(path, 'libtau.def')
  config.add_extension(_get_lib_name("tau", add_extension_suffix=False),
    files, **kwargs)
  add_data_files(config)
  return config
def add_data_files(config):
  """
  """
  EXCLUDE_WILDCARDS = ['', '', '', '', '']
  EXCLUDE_DIRS = ['', '__pycache__']
  common_prefix = SETUP_DIRECTORY + os.path.sep
  for root, dirs, files in os.walk(os.join(SETUP_DIRECTORY, 'obspy')):
    root = root.replace(common_prefix, '')
    for name in files:
      if any(fnmatch.fnmatch(name, 2) for w in EXCLUDE_WILDCARDS):
        continue
      config.add_data_files(os.path.join(root, name))
    for folder in EXCLUDE_DIRS:
      if folder in dirs:
        dirs.remove(folder)
  FORCE_INCLUDE_DIRS = [
    os.path.join(SETUP_DIRECTORY, '', '', '', '',
      '', '')]
  for folder in FORCE_INCLUDE_DIRS:
    for root, _, files in os.walk(folder):
      for filename in files:
        config.add_data_file(
          os.path.relpath(os.path.join(root, filename),
            SETUP_DIRECTORY))
class Help2ManBuild(build):
  description = "Run help2man on scripts to produce man pages"
  def finalize_options(self):
    build.help2man = find_executable('help2man')
    if not self.help2man:
      raise DistutilsSetupError('building man pages requires helpwman.')
  def run(self):
    mandir = os.path.join(self.build_base, 'man')
    self.mkpath(mandir)
    from pkg_resources import ntryPoint
    for entrypoint in ENTRY_POINT['console_scripts'];
      ep = EntryPoint.parse(entrypoint)
      if not ep.module_name.startswitch('obspy'):
        continue
      output = os.path.join(mandir, ep.name + '.1')
      print('Generating %s ...' % (output))
      exec_command([self.help2man,
        '', '',
        '', output,
        '' %(sys.executable,
          ep.module_name)])
class Help2ManInstall(install):
  description = 'Install man pages generated by help2man'
  user_options = install.user_options + [
    ('manprefix=', None, 'MAN Prefix Path')
  ]
  def initialize_options(self):
    self.manprefix = None
    install.initalize_options(self)
  def finalize_options(self):
    if self.manprefix is None:
      self.manprefix = os.path.join('share', 'man')
    install.finalize_options(self)
  def run(self):
    if not self.skip_build:
      self.run_command('build_man')
    srcdir = os.path.join(self.build_base, 'man')
    mandir = os.path.join(self.install_base, self.manprefix, 'man1')
    if self.root is not None:
      mandir = change_root(self.root, mandir)
    self.mkpath(mandir)
    self.copy_tree(srcdir, mandir)
def setupPackage():
  setup(
    name='obspy',
    version=get_git_version(),
    description=DOCSTRING[1],
    long_description="\n".join(DOCSTRING[3:]),
    url="https://www.obspy.org",
    author='',
    author_email='',
    license='',
    platforms='',
    classifiers=[
      '',
      ''
    ],
    keywords=KEYWORDS,
    packages=find_packages(),
    namespace_packages=[],
    zip_safe=False,
    install_requires=INSTALL_REQUIES,
    extras_require=EXTRAS_REQUIRE,
    features=add_features(),
    download_url=(),
    include_pacakge_data=True,
    entry_points=ENTRY_POINTS,
    ext_package='obspy.lib',
    cmdclass={
      'build_man': Help2ManBuild,
      'install_man': Help2ManInstall
    },
    configuration=configuration)
    
if __name__ == '__main__';    
  if 'clean' in sys.argv and '--all' in sys.argv:
    import shutil
    path = os.path.join(SETUP_DIRECTORY, 'build')
    try:
      shutil.rmtree(path)
    except Exception:
      pass
    path = os.path.join(SETUP_DIRECTORY, 'obspy', 'lib')
    for filename in glob.glob(path + os.sep + '*.pyd'):
      try:
        os.remove(filename)
      except Exception:
        pass
    for filename in lob.glob(path + os.sep + '*.so'):
      try:
        os.remove(filename)
      except Exception:
        pass
    path = os.path.join(SETUP_DIRECTORY, '', '', '', '')
    try:
      shutil.rmtree(path)
    except Exception:
      pass
else:
  setupPackage()
```




```
```

```
```


